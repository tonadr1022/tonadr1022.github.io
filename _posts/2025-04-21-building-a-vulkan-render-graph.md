---
layout: post
title: Building a Vulkan Render Graph
subtitle: Eliminating (some) manual synchronization in my experimental renderer
tags: [graphics, vulkan]
---

Graphics APIs such as OpenGL, WebGPU, and DirectX 11 handle GPU synchronization under the hood. The cost of this is increased CPU overhead, since they need to handle any sequence of commands from the host. After using OpenGL for around a year, I decided to pick up Vulkan, which notably is not in the "I'll do all synchronization, so you get to have fun" group of APIs.

It went well, although after a handful of months, I became tired of manual synchronization, knowing that tracking individual resource states is not scalable for larger scale renderers that can handle dozens of passes (ie those found in game engines). Thus, I decided implementing a render graph would be an interesting and painful endeavor. Such a graph would completely eliminate the need to manually place barriers in between usage of GPU resources and open the door for further optimization, and it would help my renderer get one small step closer to those found in major game engines and production grade apps.

> [Here's my experimental Vulkan renderer](https://github.com/tonadr1022/vkrender2)

## Why render graphs?

Most implicit graphics API drivers implement just-in-time synchronization. For example, when a texture is bound for reading, the question arises: "is there a previous write occurring that needs to be waited on?", to which the driver answers with all sorts of optimized trickery. Vulkan, an explicit API, doesn't provide this. Before reading an image in a shader, a `VkImageMemoryBarrier2` struct needs to be filled out and placed so the GPU knows the memory should be visible and the read itself is ordered properly. Placing these barriers manually works fine for smaller apps and is what I've done for months, but when more passes and/or cross-queue operations are in the mix, a lot more time managing this state is necessary, making synchronization code more brittle and generally not fun at all.

A render graph solves this by providing a global view of a frame, so synchronization primitives can be placed optimally. It's a DAG, where passes are nodes, and resources are edges. The graph knows the usage of every resource, so it can decide when access needs to be synchronized. It also knows which passes use which resources, so passes can be reordered for optimal synchronization and removed if they don't contribute to the final output. The user defines the resources used by every pass, and the render graph compiles a set of synchronization commands to ensure each pass executes safely.

The bulk my implementation is heavily inspired by [Hans-Kristian Arntzen's implementation](https://themaister.net/blog/2017/08/15/render-graphs-and-vulkan-a-deep-dive/) in the [Granite renderer](https://github.com/Themaister/Granite), and [Marcell Kiss' render graph in Vuk](https://github.com/martty/vuk). This is my first attempt, so learning from their implementations has been crucial.

## Resources: Buffers and Textures

The first step in the process is determining inputs and outputs. In my graph, a conceptual "node" is a render pass, a set of commands that read and write a set of resources. Thus, resources are considered edges.

#### Textures

- currently **render graph** owned/generated, but user textures could be possible in future
- future-proofed for auto resource aliasing -- e.g. using a texture multiple times in the frame
- user specifies swapchain-relative or an absolute size

#### Buffers

- currently **user owned**, but graph generated buffers could be possible
- "proxy" buffer allows underlying physical buffer to be specified each frame, since graph is not rebuilt per-frame, enabling double-buffering/swapping buffers

## Graph Compilation and Execution

After all passes, resource accesses, and swapchain outputs are submitted, the graph can be compiled:

### 1. **Validation**

- Ensures inputs/outputs are satisfied
- Detects cycles, broken dependencies

### 2. **Render Pass Topological Sort**

- a pass writing to a resource must occur before any pass reading it
- Final sink node == swapchain write
- sort passes based on resource dependencies
- **Maybe in the future**: reordering passes to minimize wait time/pipeline stalls
- **Maybe in the future**: culling passes that don't contribute to a swapchain write, useful for debug views

### 3. **Barrier Building**

- any resource write in a pass becomes a **flush** barrier whose access/stage must be waited on by readers
- any resource read in a pass becomes an **invalidate** barrier, since GPU cache must be invalidated for it to be read in the given access/stage

### 4. **Resource Aliasing (Future (maybe lol...))**

- not implemented yet (hopefully yet)
- If Pass A and Pass B use similar textures but don't overlap, the same texture can be used for both
- Example: use the same physical texture for GBuffer texture 0 and tonemap passes since both may use R32G32B32A32Unorm and are of equal size.

### 5. **Execution**

- build and place the actual barriers based on invalidates/flushes of resources.
- call lambdas defined by each render pass to execute the frame.
- blit swapchain write images to the swapchain (Future: draw to swapchain if formats match)

## Defining Passes

This is likely to change at some point, but for now, passes can be defined like this, without any manual sync involved.

```cpp

graph.set_backbuffer_img("color_out");
auto &forward = graph.add_pass("forward");
auto color_out_handle = forward.add("color_out",
                                {.format = Format::R32G32B32A32Sfloat},
                                Access::ColorWrite);
auto depth_out_handle = forward.add("depth",
                                {.format = depth_img_format_},
                                Access::DepthStencilWrite);
forward.set_execute_fn([this, depth_out_handle,
                       color_out_handle](CmdEncoder& cmd) {
    auto* tex = graph.get_texture(color_out_handle);
    // set color attachment, start dynamic rendering, draw some triangles
});
graph.bake();

```

## Learnings and what's next

Generally speaking, my entire renderer is pretty jank, since I'm constantly experimenting and haven't made a comprehensive abstraction over raw Vulkan. It does produce some decent renders, however. This render graph certainly made it simpler to express GPU interaction, but much can be improved:

- Async compute and multiple queue support
- Rebuilding the render graph when passes change (currently I compile it once, so all passes always execute, not realistic)
- Faster implementation and/or use of a custom allocator (If it becomes slow in profiling, for now it's fine)
- Resource aliasing to reduce memory usage
- Pass reordering and culling
- Subresource level synchronization, currently the granularity is an entire image or buffer

## Mandatory Validation-Error-Free Cube (yes it renders an actual scene)

![Render Graph Cube](/assets/img/render_graph_cube.png)

## Conclusion

Hopefully this was interesting and insightful. I wrote this in one draft, and most of the reason for writing is to look back fondly in 20 years when I have a super mega renderer that makes this look trivial (lol). By the way, I wrote this article with ZERO AI, which feels rare in 2025.
