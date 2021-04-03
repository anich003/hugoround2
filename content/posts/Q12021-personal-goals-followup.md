---
title: "Q1 2021 Personal Goals Followup"
date: 2021-04-03T11:29:10-07:00
draft: false
---

# 2021 Q1 Goals

1. [Finish Pikuma Game Development Course]({{< ref "#pikuma" >}}) :white_check_mark:

2. [Finish MIT Strang Linear Algebra Course]({{< ref "#linalg" >}}) :construction:

3. [Set a wedding date]({{< ref "#wedding" >}}) :white_check_mark:

4. [32 FNS workouts]({{< ref "#workouts" >}}) :white_check_mark: :white_check_mark:

   

* Add-on accomplishments!
  * Finished Nand2Tetris Part 1
  * Mozzy Eye Love You's
  * Therapy


## Finish Pikuma Game Development Course {#pikuma}

The course is an academic walkthrough the internals of a 2D Game Engine built on a tech stack of C++, SDL, and Lua/Sol. Starting from a blank source code file, the instructor walks the student through installing dependencies, setting up a build system with Make, organizing the engine code into the ECS pattern and finally moving game specific logic & scripts into Lua to allow rapid prototyping without needing to recompile the source code. By the instructor's own admission the resulting code is not production-ready and is built for the purpose of clarity and instruction - there are many optimizations that could be implemented but were beyond the scope of the course. At the end of the course, the student has a working 2D Game Engine that supports basic 2-dimensional physics (position, velocity and collisions), Sprite and Tilemap rendering, the (supposed) ability to handle updating and rendering "many" Entities simultaneously, and scripting via Lua. 

Overall, the course is very instructional. I'm not fluent in C++ but am comfortable with basic programming ideas like OOP and control flow. It was very instructive to see how basic data structures like lists, vectors, maps are used to manage important constructs in the game engine space. For example, the instructor explained that for performance reasons it is a good idea to keep data "packed" in a `std::vector` so that the CPU can retrieve all the data it may need for a particular *system* like calculating the next frame of movement. In order to keep this data packed but organized, he showed how `std::unordered_map` s could be used to "map" between vector indices and Entity IDs. Additionally, it was very helpful to observe how a game engine code base could be organized. There is a Game class that is responsible for initializing and coordinating system resources (especially SDL), an AssetStore responsible for loading and keeping references to Sprites and Fonts, a Registry for managing all of the game's Entities, Components and Systems. The instructors shows how publicly exposed functionality on each of these helper classes could be accessed by sharing `std::unique_ptr`s. There is even an EventBus to facilitate (synchronous) communication between different otherwise decoupled parts of the code base. 

Although I really enjoyed the course, I did have a few issues with it, mostly dealing with my personal sense of motivation and perhaps lack of background or context. The course, I think, makes the assumption that you already have experience with another game engine like Unity or Unreal. In other words, you may already have built a game using these other tools or have a game in mind you'd like to build, and are looking for a peek-under-the-hood to empower you to turn your idea into reality. This is not a game development course - this is a class on how to build a game engine. Because of this, I think there was a distinct lack of meaningful "motivation" for why certain architectural decisions were made. For example, I found myself simply nodding along at the hand-wavy explanations for why ECS was an improvement over traditional object-oriented approaches. In fact, much of the course is structured as a "code-along-with-me" experience, which, as I gain some experience as a professional developer find less and less appealing. There is a lot of benefit I think to the hard grind of discovering requirements and architectures that support these requirements that is really hard to instill in a short $20 online course. 

With that said, I think the course accomplishes what it set out to do - demonstrate how a 2D Game Engine works in general. Show how libraries are leveraged, how game engine data can be organized and how the machinery is laid out to produce pixels on the screen. I'm glad I took the course. I learned some interesting C++ syntax that I'm sure I'll never use professionally, how to use Make and Makefiles, and got a little bit better and compiling and interpreting compiler errors. Moving forward, I will probably take a few steps back and work on *ideation* and *execution.* Now that I have some examples of how to get something on the screen and how to capture user input, my next goals are to learn how to clearly articulate a concept (or the start of one) and to make progress towards executing on it. One example of this is  a traffic simulator that demonstrates to the user how traffic might behave when drivers take on specific driving profiles (e.g. aggressive lane switching, fast lane bogging, staggered).

### Architecture

#### ECS

An **ECS** architecture is one in which things (cars, trees, tile, maps, UI elements) are modeled as **E**ntities with only an id. The data associated with entities are captured as **C**omponents that are  stored as contiguous arrays of structs. For example, a `TransformComponent` is a simple struct that holds position, rotation and scale information, while a `SpriteComponent` holds a reference to the `SDL_Texture` and the source rectangle. Notice components are *data-only*. **S**ystems, then, are pieces of code that operate on that data and otherwise should have no other state associated with them. For example, a `MovementSystem` would act on entities that have both a `TransformComponent` and a `RigidBodyComponent`, mutating the former using velocity and acceleration contained in the former. 

Choosing ECS vs OOP is based on the performance requirements of the application. OOP is arguably simpler to prototype and reason about but this will come at the cost of CPU performance. This is because more data than is actually needed for a given function will be retrieved when using the OOP approach. For example, in order to update an entity's position,  the CPU only needs access its current position and velocity. Since *all* of the data that describes the entity are stored together as a struct, retrieving the position and velocity will also cause the CPU to retrieve sprite, AI, animation, keyboard, sound. This fills up the CPUs cache line with data it doesn't care about for that particular function and will cause additional round trips to memory which is slow.

ECS reorganizes the game engine data so that pieces of data that are consumed together are stored together and increases the chance of a *cache hit,* removing the need to make a round trip to main memory. The CPU is able to pull down a chunk of data and operate on it in a continuous fashion. 

#### SDL

The Simple Direct Media Layer is a C library that gives C/C++ programs access to a system's graphics, audio and IO systems through a clean abstraction layer. This allows a C/C++ program written against SDL to run in any environment that has SDL installed. In the context of this course, SDL is used to launch and manage a *window* and *renderer*, used together to draw onto the screen, to load and retrieve images from the disk into in-memory *textures* that can be easily  drawn via the *renderer*, and to load and retrieve font files for, you guessed it, writing stuff on the screen. The SDL constructs that are used in this course are:

- SDL_Window
- SDL_Renderer

    *Holds the state required to draw onto a window. Drawing functions need access to this.*

- SDL_Texture

    *A block of GPU memory (?) that can be efficiently rendered onto the screen*

- SDL_Rect

    *A simple structure for holding (x,y) coordinates and (width, height)*

- TTF_Font

### Game Engine

The instructor describes the difference between the *engine,* the code machinery that executes logic, and the *data,* that contains the game-specific *configuration*. In other words, a good game engine, should be able to be pointed at one database of configurations to run a game, and then near seamlessly be pointed at a different database to run a different game. The game engine implements an API that game designers can use to build different games, all using the same core functionality.


## Finish MIT Strang Linear Algebra Course {#linalg}

I had started Gilbert Strang's Linear Algebra course on MIT Open Course Ware a few months back well before Q1 of 2021. I even bought the book after being fed up with trying to read content on my computer screen. Like most "new" things, progress was rapid in the beginning and started to slow once I started encountering difficult ideas or longer chapters (determinants...). I set a goal of finally finishing the course by the end of March 2021, partly out of a sense of academic curiousity but mostly out of a new need to "finish" things that I've started (see this blog post). 

I actually found it pretty slow-going to get back into the groove of studying the topic after having taken a long break. Some of the skills I had learned in earlier chapters (elimination, finding null spaces) were needed in the later ones. Because of the amount of friction I was encountering, I found myself pivoting into a new course that had much more structure, a shorter timeline and was brand new to me: Nand2Tetris. 

By the start of March, I had made very little progress towards completing this goal. But I did learn some that I should celebrate:

* I kind of know what eigenvectors and eigenvalues are now

  > *A matrix stretches and rotates vectors. Eigenvectors are only stretched ie scaled by their eigenvalue*

* I know why they can be useful

  >  *It's eaiser to track what repeated applications of a matrix does using its eigenvectors rather than using its natural column space*

* `e^iπ` is much easier to understand using the `exp(x)` function

  >  	`exp(x) = sum(x^n/n! for n in infinity)`


## Set a wedding date {#wedding}

We set a date: May 19th, 2021. 

It's also Pam's Lola's birthday

## 32 FNS Workouts {#workouts}

**19 workouts** in February and **17 workouts** in March!

This was actually an "easier" goal to hit than I originally anticipated. Once I stopped thinking in terms of the daily "burden" of going to the gym and instead focused on miniature goals like going 4x a week, hitting the larger goal (32 workouts in 2 months) became much more palatable. And obviously there was the added benefit of being able to celebrate weekly victories instead of having to wait for the end of the quarter.

There were definitely some ups and downs during executing on this but I am proud of the fact that I agreed on a system with myself, mostly stuck with it, and, when necessary, altered my routine to keep the spirit of that agreement intact. 

I will keep this pace up for Q2.

![February Workouts](/images/workouts-february.png)
![March Workouts](/images/workouts-march.png)