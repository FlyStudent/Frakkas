![Frakkas Logo](https://cdn.discordapp.com/attachments/940650918385418350/953339712234459218/Logotype4x.png)

# Frakkas, ISART Digital student game engine

**Frakkas** is a game engine produced for a student project. 

The name of the engine comes from the french word '_fracas_', which means something that hits, crashes, and breaks everything. It is like our beat 'em up game.

This git repository is **source code only**. 

## Summary
1. [Engine goal](#goal)
2. [Game technical challenges](#game-technical-challenges)
3. [Engine technical challenges](#engine-technical-challenges)
4. [Engine tools](#engine-tools)
    - [CLion](#clion)
    - [OpenGL](#opengl)
    - [SDL2](#sdl2)
    - [miniaudio](#miniaudio)
    - [ImGui](#imgui)
    - [Assimp](#assimp)
    - [JoltPhysics](#joltphysics)
5. [Editor tools](#editor-tools)
    - [Reflection](#reflection)
    - [Serialization](#serialization)

# Engine goal : the game _**Frakkarena**_ <a name="goal"></a>

Our goal is to produce an **endless Beat ’em up**, or a **Warriors** game, inspired by the _Sanctuary Onslaught_ mode of _Waframe_. 3D space, with a camera that flies over the action from above **(Top Down)**. The goal will be to get a good score.

> The game opens on a simplistic menu and directly brings the player to the gameplay part. You control a character, armed with a melee weapon. In a medium-sized, closed map, you have to survive against waves of randomly appearing enemies, and for that, you have to play well with your running and a sword attack or two.

The universe would be fantasy-futuristic, it will be colored thanks to a cartoon effect and neon light. The sets are not the objective of the project, **the universe is a secondary objective**.
There will be only 2 types of enemies, and their number will compensate for their minimal intelligence.
A timer defines the time it takes to survive the waves of enemies in the arena, once finished, the player must reach a portal to go to another map, and we start again.

> _Game available on itch.io: [Frakkarena](https://fura-i.itch.io/frakkarena)_

# Game technical challenges

- Offer the player a smooth and enjoyable gameplay
- Have a large number of enemies on the stage at the same time, and manage multiple collisions
- Load scenes quickly
- Add graphics or particle effects to style the game
- Add animations to energize the action

# Engine technical challenges

- Efficient resource loading
- A solid component/Actor system for gameplay
- An optimal architecture to divide the program into several practical libraries (rendering, sound, physics)
- Optimized rendering to display as many enemies as possible at the same time
- Add graphic effects with shaders or frame buffers easily
- Multiple and varied visual effects
- A simple, clear, and precise user interface

# Engine tools

- [CLion](#clion)
- [OpenGL](#opengl)
- [SDL2](#sdl2)
- [miniaudio](#miniaudio)
- [ImGui](#imgui)
- [Assimp](#assimp)
- [JoltPhysics](#joltphysics)

## CLion - _Clang_ <a name="clion"></a>

We decided to make a _**CMake**_ project, so the _**CLion**_ IDE seems to us the best option for this type of project.
It will be used under Windows as well as under Linux with the _**Clang**_ compiler and the _**Ninja**_ generator[.](VS-vs-CL) _**Visual Studio**_ is not a problem, however, CLion has some advantages in addition to an interface close to Visual Studio.

The generation of executables via CMake allows us better control of the architecture of the project, as well as a better understanding. The interface of this IDE is lighter, and we are talking about a more efficient intellisense than that of Visual Studio.

## OpenGL

The API used to communicate with the GPU is _**OpenGL 4.5**_.

It has the advantage of being very well documented. Its pipeline seems clear to us and fairly well managed by the API, we won't have to worry about the Rasterizer for example. It is sufficiently optimized and allows instantiation, it will not pose optimization problems. It is an API compatible with Windows and Linux.

_**Vulkan**_ is a much more scalable API and requires more knowledge of the rendering pipeline as well as the optimizations we need. As part of our project, we believe that we do not have the time to dive into such optimizations, and our engine does not have such ambitions that we wish to modify the slightest parameters of our processors.

Same for _**DirectX 12**_.

_**DirectX 11**_ is not compatible outside of Windows, and after a few tries its implementation is not as simple as OpenGL, despite sometimes clearer and more intuitive functions than OpenGL.

## SDL2

For the windowing API and communication with the user, we chose _**SDL2**_. The programming interface is not very different from _**GLFW**_, even if the management of inputs requires better organization, little time will be taken to integrate it.

As SDL is a library for making games, it contains the integration of audio and more complete management of gamepad inputs than GLFW (vibrations).

**GLFW** has less interesting specifics for our game.

## miniaudio

For audio management, we will use _**miniaudio**_. This is a single file library for audio playback and capture.

We intended to use _**SDL_mixer**_ but we had problems implementing it. Since the audio isn't an essential part of our engine we decided to change the library.

## ImGui

ImGui is very popular and has a lot of documentation and extensions, which will be very useful to have a quality UI in our engine.
ImGui also integrates an IO which can return time, delta time, basic inputs, etc….

## Assimp

We were used to _**tinyobjloader**_. But it only does .obj files, which are very few used files in the big video game industry.
_**assimp**_ makes several types, and especially _**animations**_ (.fbx or .dae). It is a very standard library for model parsing, so there is a lot of documentation on the internet.

## JoltPhysics

JoltPhysics is a very recent library (1st release January 23), Open Source, which was used on the game Horizon Forbidden West (Guerrilla Games).  
It is a very complete library that supports multi-core and multithreading, with performance equivalent to, or even superior to, _**PhysX**_.

This library is also coded very properly. There is a strict convention with prefixes, and every function is documented so that we can understand them easily. It is really comfortable to learn about the Jolt architecture.

# Editor tools

- [Reflection](#reflection)
- [Serialization](#serialization)

## Reflection

We decided to make our own Reflection system. It is a way to avoid heavy Reflection data generated by reflection libraries, and since we code our script in c++, it is not really complex to have a simple c++ class reflection. Moreover, it is really interesting to learn how we can do it.  
Actually, we reflect only Component data, and to do this, we are creating a `ClassMetaData` variable for each component class, in which we add a `DataDescriptor` for each component's field. This `ClassMetaData` variable is static.  
The annoying part is to reflect the fields. Even with macros that shorten the component declaration, the user has to enumerate all the informations of the field.

Anyway, you can see what it looks like [here](component-scripting)


## Serialization

We decided to make our own Serialization with our own scene text file. This is a simple and interesting solution, we will be able to modify the ways we do it really fast, and parsing the game scenes from text files will be more optimized than from .json or .XML.

We didn't choose tinyxml for XML parsing because it is not very helpful, and XML files could be complicated for git merge.  
Json was our second choice, with the nlohmann json header-only library, but we can't choose how the JSON is written, less free that creates our own scene file.

Learn about our scene files -> [here](scene-file-presentation)

[Head of page](#frakkas-isart-digital-student-game-engine)
