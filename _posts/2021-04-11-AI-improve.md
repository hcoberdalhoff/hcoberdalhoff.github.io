---
layout: post
title: AI's almost imperceptible improvement
date: 2021-04-11
author: Hans-Carl
categories:
    - AI
    - Google
    - Machine Learning
---
AI is everywhere. It can by now detect human cancer, recognize images and speech much better than most humans. More mundane examples are the voice assistants in our phones or amazon giving you better recommendations based on your buying history. Also: Damn good search results by Google that seem to understand the most probable context of your search request.

Let's look into this more deeply. You can easily find examples, where your input to a search request was slightly off, but still the machine got you what you wanted. If you google "black horse song" you will get result for Katy Perry's song "**Dark** Horse". Google is actually so sure about this result, that it does not even offer some form of "maybe you really meant **black** horse" alternative.

Ok, so Google search is smart about replacing certain searches based on probable alternatives. That may be something where human developers were smart and added features to look into more probable semantic alternatives for searches with weak results. Maybe. But maybe this is part of something different - namely: understanding context better as AI.

Most (perceptive) AI research and implementations of the last years did focus on recognizing singular images or recognizing speech. The [ImageNet](https://en.wikipedia.org/wiki/ImageNet) dataset is up until today an industry standard benchmark for visual perception based on recognizing real world objects from pre-annotated images. Similar benchmarks exist for recognizing language and written text. 

What these benchmarks do not cover is giving a benchmark on the handling of "hidden/indirect information" or "context" as we know it. Sure there probably are less well known tests out there that include these but at least I do not know of prominent announcements for better understanding "context" - at least outside of specific scenarios like your voice assistant waiting for a follow up command after asking about something. But it seems like you can now do that test on your own at home.

Let me give you this example from personal experience:

Google Photos allows you to search for people in your pictures. For this, Google uses the photos you took, looks for faces and uses an AI to group/label pictures that are probably from the same person. So, one evening when I did search for pictures of my son, my expectation was to only see pictures where my son's face is actually visible. But this was not the case. I got this:

