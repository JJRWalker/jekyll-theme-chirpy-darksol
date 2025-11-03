---
title: Software From Scratch
description: "Introdution post and justification for working from scratch."
author: june
categories: [Programming, Opinion]
tags: [Opinion, Ethos]
math: true
mermaid: true
pin: true
---

## The Problem
Part of the reason I am starting to document the knowledge I have accumilated while working on software, is that in recent years, I have found searching for information online to be increasingly difficult.


In my opinion, the widespread use of generative AI has only decreased the signal to noise ratio on the internet.


Along with this, in the past I have become frustraited by searching for an answer to a question, only to find someone answering along the lines of "why would you want to do that?" or "use this pre-built solution".


while I appreciate when people take the time to answer questions and share their knowledge. Often I find they don't really deal with the root of the question in their anwser.


It seems to me a lot of people are focused on results and not the process of getting there. Which doesn't make them villains, but it further abstracts the fundimental knowledge from the people using it.


Not to be too dramatic, but eventually this can lead to lost knowledge, or at the very least, information that's only held by industry employees at certain institutions. Making it harder for people just getting started to find answers that will help them most during their development.


This doesn't mean none of us know how to do our jobs, but quite often the people who do have a deep understanding, aren't able to share that knowledge because they're too busy working.


## The Solution
While I am by no means an authority on how to write the best software. I do want to share what knowledge I do have in order to help the people who want it.


For me the biggest leaps in competency as a programmer have come from getting down to the basics, and learning the base set of instructions required to solve each problem.


In doing so you understand what impact you're having when you write high level code, what you should and really shouldn't be doing in a hot loop.


This also gives you the added bonus of your code doing exactly what you need it to do in your problem space. Generic libraries are writen to solve generic problems, which while useful, can mean doing steps you don't need it to do. Unreal for example, while a great tool for a lot of people, it has a lot of features that maybe your project doesn't need. While you can turn some of these off, you cannot opt out of core features like memory management easily. While it is possible to avoid internally. If you don't use UObjects and allow the memory to be managed by the engine, you'll have a hard time creating assets in editor, loading them and modifying their properties.


With this "From Scratch" approach, I've found that my mindset has changed when writing all code, not just at a low level. To me, the ideal solution now is: The fewest number of steps to solve the problem.


When I was starting out, I would try and come up with this complex solution to solve all cases of a problem, when in reality, what I needed to do is reduce the problem down to it's simplest form, and then solve that.


Quite often we overcomplicate solutions to problems because we feel like they need to be complex, when they don't.


A big turning point for me as a programmer was finding other programmers who felt this way. It wasn't an immediate explosion in my mind, but more of a gentle pull on a thread to straighten it out overtime. For example Casey Muratori's Handmade series, along with the handmade network it spawned, was (and still is) a great resource for learning how to find solutions to problems.


While we probably don't have the exact same opinions on how to write code, the core ideology is the same: 


*Write code that does the thing you want it to do.*


Which sounds reductive, but if you learnt programming like I did, you probably picked up the habit of overthinking problems. 


A lot of how I was taught to code was through an incredibly structured Object Oriented mindset. This is not to say all Object Oriented approaches are objectively wrong, but quite often the kind of OOP you're taught is, in my opinion, overly complex.


You don't really need a class heirarchy or a web like map of your application in your mind. You just need data, and functions that operate on that data, resulting in a different output. Simplifying the problem in your head like this, often leads to you, and the computer, doing less work.



Another justification for working from scratch is: Learning and implementing a solution, often takes the same amount of time as learning how to use a third party library.


This is not always the case though. In some situations, like graphics for example, it is far more time efficient to use an API like: Vulkan, OpenGL or DirectX. But more often than not, writing it myself, I find:

- I gain more knowledge
- The application runs smoother
- I have a cleaner codebase


I haven't really gone into that last point yet. To explain it a bit more, often when working with third party libraries, I find myself writing a lot of code to interface with that library. It probably doesn't give me the data exactly as I want, so I need to write an additional function that translates the data layout I got, into the data layout I need. Commonly this is the case in graphics where I need to send data to the GPU in a specific layout, or I need a texture to be stored in top down and the API gave it to me in bottom up or something along those lines.


Removing this step, means less code, and ultimately less code is cleaner code.


While I could go on and on about my thoughts on this, I'll leave it here for the moment. Maybe getting more in-depth about specifics the future. For now, I just wanted to write a "quick" summary of why I choose to do things this way to add context to future posts.