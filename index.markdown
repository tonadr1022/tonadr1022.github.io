---
title: Tony Adriansen
subtitle: CS @ UW-Madison
---

I'm a software and computer graphics enthusiast studying computer science at [UW-Madison](https://cs.wisc.edu){:target="\_blank"}. I interned at [Capital One](https://www.capitalone.com/tech/){:target="\_blank"} summer 2024, and I'm incoming at [Amazon](https://amazon.com){:target="\_blank"} summer 2025. I'm broadly interested in 3D rendering, full stack dev, and (eventually) making Minecraft 2.

Below are some of the projects I've worked on. All of my source code can be found on my [Github](https://github.com/tonadr1022){:target="\_blank"}. Also feel free to peruse my [blog](/blog).

### [ShaderShare](https://www.shader-share.com){:target="\_blank"}

<p style="margin: 0;">
A web app for sharing and discovering <a href="https://en.wikipedia.org/wiki/Shader" target="_blank">shaders</a>, heavily inspired by <a href="https://shadertoy.com" target="_blank">Shadertoy</a>. One day, I was messing around on Shadertoy and became frustrated with the lack of vim keybindings in the editor and inability to use custom textures from the internet, so I built my own. Tech stack: Next.js, WebGL2, Go, Postgres, Cloudflare R2. <a href="https://github.com/tonadr1022/shadershare" target="_blank">Source Code</a>
</p>
<p>
Here's the first shader ever on the site (hover it).
</p>

<iframe
  title="Shadershare Player"
  allowfullscreen
  allow="clipboard-write; web-share"
  width="640"
  height="360"
  style="border: none"
  src="https://www.shader-share.com/embed/shader/d16b208f-5e47-4855-987a-fed51782c86a"
></iframe>

### [TCraft Voxel Engine](https://github.com/tonadr1022/TCraft){:target="\_blank"}

<p style="margin-top: 0;">
A revamped Minecraft-style voxel engine written in C++ and OpenGL 4.6. It features an indirect rendering pipeline to reduce driver overhead, compute shader frustum culling and various rendering optmizations, block and terrain custumization (and building and mining them), and multithreaded terrain generation and meshing.
</p>

<a style="display: flex; justify-content: center;" href="https://github.com/tonadr1022/TCraft">
    <img src="/assets/img/tcraft_building.png" alt="TCraft Building" style="">
</a>

<iframe width="560" height="315" src="https://www.youtube.com/embed/videoseries?si=FyFUz2tZl2XnGsub&amp;list=PLHcOWdaLwRvwq5tjGYTkSlascX6TRfaxz" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

### [CubeChron](https://cube-chron.com){:target="\_blank"}

<p style="margin: 0;">
A speed-cubing application that allows users to track solves and statistics both offline and online. Frontend built with React.js, TypeScript and IndexedDB for offline storage. Backend wrtten in Go. This ia a rebuild of a previous iteration that used Next.js and GraphQL. <a href="https://github.com/tonadr1022/speed-cube-time" target="_blank">Source Code</a>
</p>

<a style="display: flex; justify-content: center;" href="https://cube-chron.com">
    <img src="/assets/img/cube-chron-small.jpg" alt="CubeChron - Speedcubing timer" style="">
</a>

### [OpenGL 4.6 Engine](https://github.com/tonadr1022/opengl_modern){:target="\_blank"}

<p style="margin: 0;">
A engine using C++ targeting OpenGL 4.6. I've focused on more modern techniques including direct state access, bindless textures, and AZDO (Approaching Zero Driver Overhead) techniques to optimize performance and reduce graphics driver overhead. It is very much a work in progress, but here are a few early pictures illustrating a PBR shader and basic shadow mapping.
</p>
<a style="display: flex;" class="project" href="https://github.com/tonadr1022/opengl_renderer">
    <img src="/assets/img/Sponza1.png" alt="4.6 Renderer Picture 1" style="width: 50%;">
    <img src="/assets/img/SponzaShadow.png" alt="4.6 Renderer Picture 2" style="width: 50%;">
</a>

### [OpenGL 4.1 Renderer](https://github.com/tonadr1022/opengl_renderer){:target="\_blank"}

<p style="margin: 0;" >
    <a style="margin: 0;" href="https://drive.google.com/file/d/12HNHJjyapD44S-P6WBDA-2trbGszb3au/view?usp=sharing" target="_blank">
Video Demo
    </a>
</p>
<p style="margin: 0;">
An early attempt at a 3D renderer using C++ and OpenGL. Began as a personal project and expanded into an extra project for Computer Graphics at UW-Madison (CS559).
</p>

<a style="display: flex;" class="project" href="https://github.com/tonadr1022/opengl_renderer">
    <img src="/assets/img/opengl_renderer_1.png" alt="Renderer Picture 1" style="width: 50%;">
    <img src="/assets/img/opengl_renderer_2.png" alt="Renderer Picture 2" style="width: 50%;">
</a>

### [OpenGL 4.1 Voxel Engine](https://github.com/tonadr1022/VoxelEngine3D){:target="\_blank"}

<p style="margin-top: 0;">
My first attempt at a voxel engine with optimized meshing, per-voxel ambient occlusion, and Minecraft-style lighting. My first significant project with C++ and OpenGL.
</p>

<iframe width="560" height="315" src="https://www.youtube.com/embed/Gp5DdJEja50?si=7CcDJ0-FSAuSgVp9" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

<a style="display: flex; justify-content: center;" href="https://github.com/tonadr1022/VoxelEngine3D">
    <img src="/assets/img/voxel_1.jpg" alt="CPU Raytrace 1" style="">
</a>

### [CPU Raytracer](https://github.com/tonadr1022/CPURayTrace){:target="\_blank"}

<p style="margin: 0;">
A learning experience using the <a href="https://raytracing.github.io">Ray Tracing in One Weekend</a> series. I Hope to revisit ray tracing to learn more advanced techniques and implement a voxel-based ray tracer.
</p>

<a style="display: flex; justify-content: center;" href="https://github.com/tonadr1022/CPURayTrace">
    <img src="/assets/img/cpu_raytrace_1.png" alt="CPU Raytrace 1" style="width: 50%;">
</a>
