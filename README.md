# Autodesk Forge Viewer – Getting Started Tutorial

* Introduction
  - [Audience](#Audience)
  - [What do you need for your project?](#WhatDoYouNeed)
  - [What are you going to achieve in this workshop?](#WhatAreYouGoingToAchieve)

* [Prerequisites](prerequisites.md#Prerequisites)
  - [Familiar with git?](prerequisites.md#FamiliarWithGit)
  - [Install Node.js](prerequisites.md#InstallNodeJs)
  - [Install a code editor](prerequisites.md#InstallCodeEditor)
  - [Get the sources](prerequisites.md#GetTheSources)

* [Chapter 1 – Get ready with the Model Derivative API](chapters/chapter-1.md#Chapter1)
  - [Create an App](chapters/chapter-1.md#CreateAnApp)
  - [Prepare a model](chapters/chapter-1.md#PrepareAModel)
  - [Get your App Ready!](chapters/chapter-1.md#CreateYourApp)

* [Chapter 2 – Translate a model and Export it to another format](chapters/chapter-2.md#Chapter2)
  - [Request the Model Derivative to produce OBJ](chapters/chapter-2.md#RequestOBJ)

* [Appendix A – More sample and demos](appendix/appendix-a.md)
* [Appendix B – Troubleshooting](appendix/appendix-b.md)


<a name="Audience"></a>
## Audience

This documentation is designed for people familiar with [JavaScript](http://www.ecma-international.org/publications/standards/Ecma-262.htm) programming and object-oriented programming concepts.
You should also be familiar with the Autodesk Viewer and Model Derivative technology from a user's point of view. You can play with the technology [here](https://360.autodesk.com/viewer) – simply Drag 'n Drop a 2D/3D file,
and enjoy the result in your browser with no extension or plug-in installed on your computer or device.

This conceptual documentation is designed to let you quickly start exploring and developing applications with the Autodesk Viewer and Model Derivative API.


<a name="WhatDoYouNeed"></a>
## What do you need for your project?

The Autodesk Viewer and Model Derivative API web service consists of two APIs. The first API ([Model Derivative API](https://developer.autodesk.com/en/docs/model-derivative/v2/overview/)) is a REST API which enables users to represent and share their designs in different formats, as well as extract valuable metadata. The Second API ([Viewer](https://developer.autodesk.com/en/docs/viewer/v2/overview/)) – is a WebGL-based, JavaScript library for 3D and 2D model rendering. 3D and 2D model data may come from a wide array of applications, such as AutoCAD, Fusion 360, Revit, and many more.

Depending on your needs, you may prefer to write a server or a desktop application to consume the REST API. Your choice will be mainly based on how many files you need to translate, the frequency and how you're going to share results with others. In this workshop, we will only focus on a command line (desktop) tool.


<a name="WhatAreYouGoingToAchieve"></a>
## What are you going to achieve in this workshop?

When you finish the workshop you will be able to:

- View 2D/3D models in a browser or device without plug-in or additional software.
- Create and run Node.js application .
- Identify resources for learning more about the Autodesk Viewer and Model Derivative API.

The tutorial guides you through the entire process of building a simple application.
Experiments at the end of each step provide suggestions for you
to learn more about the Autodesk Viewer and Model Derivative API and the application you are building.


=========================
[Start](prerequisites.md)
