---
title: "StutterNet Project"
date: 2024-08-15
author: Yuhua Su
---

[Hugging Face Space demo app 🤗 ](https://huggingface.co/spaces/huazai676/StutterRecognition)

[Github repository](https://github.com/huazai6161/EC523-Final-Project)

[Paper](https://arxiv.org/abs/2105.05599)

## Abstract

Stuttering, also known as stammering, is a speech disorder characterized externally by involuntary repetitions and prolongations of sounds, syllables, words, or phrases as well as involuntary silent pauses or blocks in which the person who stutters is unable to produce sounds. This project adopts StutterNet, a time-delay neural network (TDNN) suitable for capturing contextual aspects of the disfluent utterances, and apply it on SEP-28k, a dataset containing over 28k clips labeled with five event types including blocks, prolongations, sound repetitions, word repetitions, and interjections, to tackle stuttering recognition task. We scale the original model and use ensemble learning to improve the performance. In the end, we design a ad-hoc algorithm generating predicton from categorized probabilities.

## Model

We adopt mel spectrogram with 80 filterbanks and sample rate of 16000 as feature extractor (differing from author of original paper, who use MFCC). Then, treating the task as a multiple binary classification problem, the features are fed into TDNN, which consists of five time delay layers with the first three focusing on the contextual frames of $$(t-2, t-1, t, t+1, t+2), (t-2, t, t+2), (t-3, t, t+3)$$ with dilation of 1, 2 and 3 respectively. Then follow fully connected layers for classification. Each layer is followed by ReLU activation function.
[![stutternet.png](https://imgos.cn/2024/08/16/66bee4a535ec8.png)](https://imgos.cn/2024/08/16/66bee4a535ec8.png)

## Dataset

We train and test the model on [SEP28K dataset](https://github.com/apple/ml-stuttering-events-dataset/), a dataset containing over 28k clips labeled with five disfluency event types including blocks, prolongations, sound repetitions, word repetitions, and interjections. Audio comes from public podcasts largely consisting of people who stutter interviewing other people who stutter.

[![屏幕截图 2024-08-16 135026.png](https://imgos.cn/2024/08/16/66bee7b042553.png)](https://imgos.cn/2024/08/16/66bee7b042553.png)

## Training details

We trained two models and scaled the second model. The mean value of two outputs is then fed into sigmoid function for binary classification. Since each sample can contribute to multiple classes with negative labels, 

## Generating prediction
