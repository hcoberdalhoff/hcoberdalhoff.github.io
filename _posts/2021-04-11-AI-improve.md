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
AI is everywhere. It can by now detect human cancer, recognize images and speech much better than most humans. More mundane examples are the voice assistants in our phones or Amazon giving you better recommendations based on your buying history. Also: Damn good search results by Google that seem to understand the most probable context of your search request.

Let's look into this more deeply. You can easily find examples, where your input to a search request was slightly off, but still the machine got you what you wanted. If you google "black horse song" you will get results for Katy Perry's song "**Dark** Horse". Google is actually so sure about this result, that it does not even offer some form of "maybe you really meant **black** horse" alternative.

Ok, so Google search is smart about replacing certain searches based on probable alternatives. That may be something where human developers were smart and added features to look into more probable semantic alternatives for searches with weak results. Maybe. But maybe this is part of something different - namely: understanding context better as AI.

Most (perceptive) AI research and implementations of the last years did focus on recognizing singular images or recognizing speech. The [ImageNet](https://en.wikipedia.org/wiki/ImageNet) dataset is up until today an industry standard benchmark for visual perception based on recognizing real world objects from pre-annotated images. Similar benchmarks exist for recognizing language and written text. 

What these benchmarks do not cover is giving a benchmark on the handling of "hidden/indirect information" or "context" as we know it. Sure there probably are less well known tests out there that include these but at least I do not know of prominent announcements for better understanding "context" - at least outside of specific scenarios like your voice assistant waiting for a follow up command after asking about something. But it seems like you can now do that test on your own at home.

Let me give you this example from personal experience:

Google Photos allows you to search for people in your pictures. For this, Google uses the photos you took, looks for faces and uses an AI to group/label pictures that are probably from the same person. So, one evening when I did search for pictures of my son, my expectation was to only see pictures where my son's face is actually visible. But this was not the case. I got this:

![2021-04-11-img1.png](/images/2021-04-11-img1.png)

In this example you can clearly see some pictures of my son's face - the boy in the red jacket. But what you can also see are pictures of him from behind - where you can not see his face. Since he is not wearing this jacket on all my other pictures, I don't think that the AI learned to wrongly identify all red jackets as my son. Rather it seems as if the machine was smart enough to understand the shared context (time and space) of those pictures as this information is well available as timestamp and GPS data in the files. In this place and this time my son was wearing a red jacket. The machine seems to understand that these photos are part of a larger context/scene and by extension, it can "find" my son by looking for his clothes in this scene - even if no face is visible. To me this was a positive but grave realization. 

One thing (among many others) that currently separates AI from "general artificial intelligence" is the limited scope of most current AI capabilities regarding broad understanding of context. They can either recognize or label things they were trained on, they can inversely produce content based on labels. But at least until recently these systems were more or less limited to the singular data points like a single picture or in broader terms: They were limited to a defined input to which they should give a hopefully valuable output like a classification. This is often not enough to solve real world problems on its own. 

To give you a well known example that needs a lot of context to function: Self driving cars need to understand their surroundings. Cars, roads, people walking, its current speed and traffic signs. Sensors and AI are used to "perceive" this real world but in many cases the resulting data is integrated in a more traditional computer simulation model of the environment to create the needed "context". Another AI will use this model's data to decide its next action. (yes, this is oversimplified). So the car can to some degree foresee stuff happening even if its sight is temporarily blocked a truck or other cars.  

Read [Computer Vision at Tesla by Jeremy Cohen](https://heartbeat.fritz.ai/computer-vision-at-tesla-cd5e88074376) if you want more detail about the car example.

Did you notice? The context in the car example is artificially created by updating a computer model (i.e. using old school computers), not generally as part of the perception itself. Machine Learning (ML) Models have the unpleasant property to not be readable by humans or classic programs in most cases. These ML Models just are a heap of numbers ("weights") shuffling the input into the correct output. Some modern variants allow to have a short term memory (more precisely: loopback/reuse of output as input) or adversarial ML networks, trying to falsify the results and only allowing results that are high quality and coherent. Both methods in the end massively broaden the capabilities of the models and create something like context awareness. An example would be [GPT-3](https://en.wikipedia.org/wiki/GPT-3) that can create long, coherent texts given a common topic. These models also seem to generate "context" on the fly for example when used as chat bots (with frighteningly good results). The difference here is - this is an inherent property of that ML model not an "behind the scenes" classic computer program only able to handle predefined objects.  

Another set of recent examples that actively focusses on spatial and continuation (time) context (see a paper by Li et al, [Neural Scene Flow Fields for Space-Time View Synthesis of Dynamic Scenes](https://www.cs.cornell.edu/~zl548/NSFF/) uses a series of different pictures from the same scene to train a model representing this scene and allows to generate new 3D perspectives of that scene. This model will even fill missing pieces.

**Input:** You can see a bush and through the leaves you can see parked cars. You don't see the floor the cars stand on.

**Output** (view from a different perspective): You can see the bush and a parking lot with asphalt on the ground. The AI "filled the gaps" with the most probable thing it has seen on previous training data: "Multiple Cars standing" is most often depicted in a parking lot - or a traffic jam.

So maybe, I should not have been too surprised about Google's ability to recognize my son, even if it can not see his face. This AI used is just more advanced than I anticipated. It has not only an understanding of people in a picture, but of a series of images belonging to a common scene and that people tend to not switch clothes in freezing weather.

But there is a catch to this. That kind of "understanding" is in our human understanding reserved to deduction, an active process strictly reserved to intelligence or at least to predefined (classic) algorithms on predefined objects. "This kid with a red jacket is probably the same boy as in the picture before". Is this a subtle step to general intelligence? Maybe. Or it is just the next iteration, the next layer of perception. More probable. We humans also do not need to "actively" think, where someone went in a scene just because we can only see them from behind. Our brain learned to keep track of that person in that moment. Google's AI just learned to take the results of the "outer layer" of perception about faces as input and use it in a next layer of perception about people in time and space.

If you look at those picture one instinctively thinks that deduction is needed to get the connection of "the boy I'm looking for wears a red jacket" and "no one else is wearing the same jacket" to get "the kid whose face I can not see, wearing a red jacket, is probably the boy I am looking for". But actually, no, no binary deduction is needed - it is just a result of the more or less automated process inherent to neural networks; real and artificial ones.

At least for me this was helpful to better understand what will happen in the next iterations of improvements in this field. We will get a better understanding of the inner workings of things we strictly attribute to humans like "general intelligence" or "social skills" or maybe even "understanding of self" - by simply observing that some of these traits, at least in tiny fractions, can be learned by a heap of numbers. Inversely we might see which areas simply refuse work well with our current understanding of ML. From this we humans might learn where mother evolution probably did not shy away from some tricks and not-so-strict applying of patterns to create desired outcomes in us.

Let us hope that we humans have more tricks up our sleeve - that we still have to learn about. Or in the worst case of understanding that most of our abilities, including reflection of oneself, are simply emergent features of enough complexity - to find peace and clarity.  

---
This was originally published on [LinkedIn](https://www.linkedin.com/pulse/ais-almost-imperceptible-improvement-hans-carl-oberdalhoff/)
