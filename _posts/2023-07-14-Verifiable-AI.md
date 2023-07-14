---
layout: post
title: Finally, verifiable AI output
date: 2023-07-14
author: Hans-Carl
categories:
    - AI
---

# Finally, verifiable AI output

## Introduction

The hype about AI is still going strong. When ChatGPT started, people realized they could basically ask these systems anything they wanted and would get a reasonable answer. But also, that sometime these system would hallucinate and would give wrong information or completely make up things. Basically you were not able to trust the output of these systems and very often you were not able to verify the output as you could not inspect the internal workings of the system.

Multiple attempts have been made to improve this. This includes asking for step-by-step explanations, which allows you to verify at least the reasoning of the system. Or adversarial systems, which try to decide if an answer is plausible before presenting it to the user. 

One approach that seems to improve this situation is to allow an AI assistant to use tools, like calculators or databases. Basically, the AI only has to device what to do and does not have to do it itself. Combine with a verbose step-by-step output, the user is able to track and recreate and verify the steps taken by the AI. If the steps are correct, the result should be correct as well.

## Toolchain

<img src="../images/2023-07-14-Ai-has-computer.jpg" width="40%" align="right" alt="DALL-E/Bing Human using a Robot using a Computer" >

OpenAI did just that in a very comprehensive way. If you are a ChatGPT+ subscriber, you now have access to [Code Interpreter](https://openai.com/blog/chatgpt-plugins#code-interpreter). Basically, they created a computer for the AI to use. More specifically, ChatGPT has access to a small virtual machine, that can run Python code and that can load data and output files. 

When you ask ChatGPT to do a complex task, like to load a CSV file, analyze its data and create a chart, it will show you the code it creates before running it and also explains its reasoning. You can follow along step by step - if needed.

I tried it with a sample, where I downloaded a CSV dataset of [EPICA ice core drill data](http://www.climatedata.info/proxies/data-downloads/) and asked ChatGPT to help me analyze it. I intentionally chose a dataset that has close to 10 thousand rows and would not be feasible for the AI to analyze in token space.

![ChatGPT query with data](/images/2023-07-14-image.png)

ChatGPT started to use python to look into the data and realize it needs to skip some lines to get a better insight.

![Firts look at the data](/images/2023-07-14-image-1.png)

![Second look at the data](/images/2023-07-14-image-2.png)

I think it is remarkable, that it was able to prompt itself and try again in a different way.

This cycle kept going on for a while, where it started to collate the data, fixed a coding mistake it made on the way and finally created a chart. All without any intervention from my side.

![Alt text](/images/2023-07-14-image-3.png)

The best part: You can take and use all the code to recreate the steps yourself and verify the results. 

So: I am using an AI to help me solve a task. The AI is solving most of the logic/thinking and programs a computer to do the heavy lifting. I, as a user can supervision the process and review the results.  

## Conclusion

This feels like a change in quality for the use of an AI chat system. Until now, most chat-style systems were not able really help me with complex tasks. You could have them answer questions, write poems or code - but never knew what quality the results if you had to work on facts or data.

A tool like Code Interpreter allows to tackle complex, data driven problems using intuitive, intention driven prompts. 

The system can help offer a solution in the form of code AND run it to create the actual results. 

The data and the code can be reused outside the system to assert the quality of results. 

In short - You can now reliably use an AI system to help you analyze data or solve complex problems. 