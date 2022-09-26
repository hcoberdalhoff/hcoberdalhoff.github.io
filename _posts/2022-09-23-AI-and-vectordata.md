---
layout: post
title: Using AI to generate 3D model data
date: 2022-09-23
author: Hans-Carl
categories:
    - AI
---

## Image generation

Generative AIs are able to create images seemingly out of thin air by taking in a prompt, describing a desired scene and outputing an image of that scene.  Examples of this are Dall-E, Midjouney or Stable Diffusion. The obvious "secret" here is that these models have been trained on millions of existing images tags/text/descriptions of what is visible on these images. 

Oversimplified, what you do is, after training on images you invert the direction of how the model is used. You input describing text (aka "the prompt") and the model will create a probable but random image for this text. An adversarial network will check that only result with enough likelihood of being a good or valid result for the prompt will be presented.

![Midjourney imitating Art by Wilhelm Fick](https://github.com/willwulfken/MidJourney-Styles-and-Keywords-Reference/raw/main/Images/MJ_V3/MidJourney_Styles/Artists/Art_By_Wilhelm_Fick.png)
*Midjourney imitating Art by Wilhelm Fick*

In other words, the ML model contains knowledge of what we will probably accept as an actual image.

We will continue to oversimplify a lot in this posting. Please bear with me :)

## Using an ML model to compress data

It is possible to use the knowledge stored in an ML model to recreate an existing image. 

Matthias Bühlmann [demonstrates](https://matthias-buehlmann.medium.com/stable-diffusion-based-image-compresssion-6f1f0a399202) using StableDiffusion as a lossy image compression technology, similiar to JPG.

![Demostration of using StableDiffusion as image compressor - Matthias Bühlmann](https://miro.medium.com/max/1100/1*RxuQz8chZmHk8n2fwpgDsg.png)
*Demostration of using StableDiffusion as image compressor*

Basically you translate and reduce (encode) the image data to a lower dimension (i.e. minimal amount of data), so that the StableDiffusions model can recreate the original image from the encoded data with minimal error. The reduced data is called *latents*. See [variational autoencoders](https://en.wikipedia.org/wiki/Variational_autoencoder). 

The reconstruction (decoding) process is similiar to using a ML model to upscale images, but using the latent representation of the original image as source. 

![Diagram of encoding to latents - Wikipedia](https://upload.wikimedia.org/wikipedia/commons/thumb/4/4a/VAE_Basic.png/638px-VAE_Basic.png)
*Diagram of encoding to latents from Wikidpedia*



## Reconstructing 3D data

3D Voxel representation of scenes: OpenVDB (add images). VERY Big, Gigabytes
Contains time dimension.

Sparse structure (think run length encoding in 3D), sorted in a tree.

Nvidia uses a latent space representation to reduce this. Also: Understands time or continuity between frames to create vectors

https://arxiv.org/abs/2208.04448
https://developer.nvidia.com/rendering-technologies/neuralvdb 

Compare DLSS 3 - input is prior frame and geometry. Creates new frames and higher resolution.

## Possible applications

Video codec?

## Interpretation

Studies show, deep language models have similiar activation pattern to human brains, when answering questions. There seems to be a similarity. 
https://www.nature.com/articles/s42003-022-03036-1

Back to our topic - Currently there is a strong separation between the knowledge stored in the ML model and the latent data created by an var. autoencoder. By the definition of the autoencoder: The better the model "understands" the data given, the less information needs to be stored in the latent space. 

Let us take this to an extreme. Assume a model could recreate, say, a movie just from mentioning the name of the movie. Then we might have created something akin to memory (as in remembering) just for machines. Like when we are able to sing along to a song we know by hard. Streaming a movie in this case would be like tapping another beings memories - just in HD. Like when a smell will bring back a whole slew of memories of an prior event.
