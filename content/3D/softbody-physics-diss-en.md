+++
title = 'Realtime Softbody Physics - Postgraduate Dissertation Project'
date = 2024-05-04T13:41:00Z
publishDate =  2024-09-08T13:41:00Z
draft = false
tags = ['3D','C++','PC','University','Physics','Dissertation','Featured']
ShowToc = true
+++

This project was my postgraduate dissertation project, titled 'Realtime Soft Body Simulation using a Spring-Mass approach'. This post can be considered a condensed summary of the dissertation document, and more detailed information can be found at  the following link:

[The Dissertation document can be found here](https://github.com/CharlieHart0/Softbody-Physics-Dissertation/blob/main/Charlie_Hart_PG_Dissertation-6.pdf)

Soft bodies are physically simulated objects capable of deformation - changing shape in response to forces experienced. Most physical objects in computer simulations are rigid bodies, which do not experience deformation, but soft body physics systems are required in order to simulate complex structures in a physically accurate way, such as clothing.

 In this project, different approaches to soft body physics were investigated, before one was chosen and implemented, alongside a system to render the deformable objects in real time. 

The implemented systems were then tested, and results showed that at least 64 cube shaped soft bodies were able to be simulated simultaneously, and that the dynamic mesh rendering method used to render deforming meshes was able to render 64 soft bodies in real time at 75.8 frames per second. 



## Video

{{< youtube oM44gNdXReA >}}

## Screenshots

Motion of simulated objects like these softbodies can be difficult to show in still images, so I reccommend watching the video above!

<img title="" src="https://i.imgur.com/gRLUkwf.png" alt="">

<img title="" src="https://i.imgur.com/6o4Tux4.png" alt="">

<img title="" src="https://i.imgur.com/uDWyoQa.png" alt="">

## Development / Implementation

### Physics

The soft body simulation approach chosen was the spring-mass approach, where point masses are constrained by a network of spring constraints. 

The spring constraints implemented acted as both compression and extension springs, where the difference between the current distance between points, and the resting position between the points, is proportional to the force applied to both points in order to return them to the resting position. The spring constant is the value used for this proportional relationship, in force units per distance units.

Additionally, a damping factor is used to limit the energy in the system. Depending on how aligned the velocities of the point masses are to the line passing between them, a small force acting against the spring's force is applied, in order to prevent a runaway system being supplied more and more energy from gravity without any energy sink in this ideal simulation, lacking air resistance or the loss of thermal energy from a material being deformed.

Point masses were created at each vertex of a polygonal mesh, and spring constraints were generated between points at the location of each edge on the mesh, with the resting length of each spring set to the distance between the two vertices. The desired total mass of the softbody object and the number of point masses generated is used to equally distribute the mass among the points.

#### Stability

As most polygonal meshes are not designed to be used in this way, they can often be unstable, with edges between vertices unable to form a semi-rigid structure in the desired shape, and deforming into a more stable configuration. This was particularly notable in concave meshes, where overhangs would fold into the object to become more stable. 

With most simple convex meshes, stability could be achieved by adding springs between vertices that create coplanar faces.

<img title="" src="https://i.imgur.com/kVkIn49.png" alt="">

### Rendering

The existing rendering system used in the engine was not capable of rendering deformable meshes - only skeletal animation was able to move vertices relative to one another, which was not appropriate for soft body objects, able to deform in many different ways, according to the physics system, separate from the GPU.

Two methods of rendering the softbodies were used, a wireframe rendering approach, mainly used during development and testing of the physics portion of the project, and a second method, in which a new mesh was generated and uploaded to the GPU for each softbody, every frame, along with calculating the normals of each face's new location, scale and orientation. 

This second method is far from ideal, as sending a new mesh from the CPU to the GPU potentially hundreds or thousands of times per frame would be very slow. Improvements are suggested in the further work section below.

## Results

As shown in the YouTube video at the top of the page, tests were carried out using an increasing number of softbody objects each time, until unexpected behaviour occurred. 

### Physics

Regardless of the rendering method used, the physics simulation began to behave unexpectedly at around 80 objects simulated concurrently. Beyond this point, the physics system would begin to drop physics frames, resulting in wild and unrealistic behaviour from the softbodies.

### Rendering

It was found that when more than 32 softbodies were rendered concurrently, the GPU memory usage of the fully rendered dynamic mesh method began to be higher than that of the simplified wireframe rendering method. Additionally, beyond this point, the frame render time began to be much higher using the dynamic mesh rendering, than the wireframe rendering.

However, these values were still fairly low, with the most GPU memory used being 2343 MB, only about 200 MB more than the amount used rendering a scene with no softbodies, and the highest frame render time recorded reaching 13ms, still below the 16.666ms required to reach 60 frames per second. 

In the tests carried out, the physics simulation would consistently fail before the rendering became an issue in any way, as the number of objects increased. This is likely to be the case regardless of the meshes used for the softbodies, as the same mesh is used to generate both the graphical and physical representations of the softbody objects. 

## Future Work

- With the current rendering method, a new mesh is uploaded to the GPU each frame, for each soft body, and is not optimal. Instead, a vertex shader could be written and used which could take the new vertex positions as uniform variables, and calculate the normal vectors in parallel on the GPU. 

- Currently, only the point masses that form the soft body provide collision detection and resolution for  the softbodies. This is sufficient for collisions against large, flat geometry, but wouldn't function when colliding with rough terrain which would be able to enter the gaps between points. In order to improve the collision detection, more point masses could be added to each face, or alternately, collision detection could be added for each triangular plane on the soft body. The latter method would likely be more efficient, but would be unable to deform around smaller obstacles, like the former method could.

- Using polygonal meshes, designed to represent objects graphically, as the structure of a physics object introduces some issues. Notably, convex shapes are not stable using the current method of constructing a network of masses at each vertex location, and springs to connect them. It would be useful to explore algorithms that could determine the stability of a given mesh when converted to a spring-mass soft body, and locate effective pairs of points to add spring connections to, in order to stabilise the object.

### 
