---
title: Photomosaic
layout: post
category: projects
description: Photomosaic generator written in Python.
---
<h2 id="what-it-does">What it Does</h2>

A photographic mosaic, or a photomosaic, is an image made up of smaller images. 
This Python program takes a pool of images and turns them into a photomosaic. 
The algorithm it uses is pretty simple, but the results are quite passable. Though I haven't tested it too much, since I lacked images for the pool. 

<h2 id="how-it-works">How it Works</h2>

### The Image Pool
A large image pool is required for a photomosaic to be effective. At the time, all I had available were the scattered family photos in my hard drive. It totalled to around ~2,000 images, which was just barely enough to generate a mosaic of 1,600 tiles. 

### The Process
Before generating a photomosaic, the program first needs to process the image pool and create a database with all the necessary data of each tile necessary for the creation of a photomosaic. This database is needed so that matching sections of the source image with each tile could be much faster, since all the data is already stored, and no longer needed to be generated in runtime. 

With the image pool ready, the program subdivides the source image into tiles (called source tiles from now on). These tiles are going to be replaced by images in the pool (pool tiles). The program then traverses through the subdivided source image and tries to find the best matching tile from the pool to place. 

Matching the colors isn't done simply by comparing the average colors of the whole tile. When comparing a source tile with a pool tile, the source tile is further subdivided into a 3&times;3 matrix. *Then* the comparison can begin. Computing the average colors of each subtile in the 3&times;3 matrix with the data in the image pool database (which also holds the average colors of the 3&times;3 matrix of each pool tile), the program can find the best matching pool tile to place. Of course, increasing this second subdivision could greatly improve the matching capabilities of the software. That would have to be something for me to work on next. 

![First Result]({{site.repo}}images{{page.url}}/photomosaic3.jpg)

The first results were barely recognizable. All the best matching tiles were placed in the top right corner of the image, since that's where the program starts traversing. And since it only uses each pool tile once, it leaves the rest of the image to receive "leftovers". I resolved this by starting in the center, where the best pool tiles could be used where it was needed most, the face in a portrait. 

![Second Result]({{site.repo}}images{{page.url}}/photomosaic1.jpg)

Much better. If I had a better pool, maybe it could be better. 

<h2 id="results">Results</h2>

![Result 1]({{site.repo}}images{{page.url}}/photomosaic2.jpg)

![Result 2]({{site.repo}}images{{page.url}}/photomosaic4.jpg)

![Result 3]({{site.repo}}images{{page.url}}/photomosaic5.jpg)

![Result 4]({{site.repo}}images{{page.url}}/photomosaic6.jpg)

<h2 id="improvements">Improvements</h2>

- Increasing the subdivision of the source and pool tiles. 
- Reuse tiles when it hasn't been used within a threshold. 
- Allow pinpointing where to start traversing. 
- A GUI could help with the above. 

I'll work on those when I have the time. I haven't touched this project in over a year. 

<h2 id="source">Source</h2>

The source is on [Github](https://github.com/john2x/photomosaic). It's a little messy though, sorry. 

