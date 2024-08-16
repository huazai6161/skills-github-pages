---
title: "StutterNet Project"
date: 2024-08-15
author: Yuhua Su
---

[HuggingFace Space demo app 🤗 ](https://huggingface.co/spaces/huazai676/StutterRecognition)

[Github repository](https://github.com/huazai6161/EC523-Final-Project)

[Reference Paper](https://arxiv.org/abs/2105.05599)

## Abstract

Stuttering, also known as stammering, is a speech disorder characterized externally by involuntary repetitions and prolongations of sounds, syllables, words, or phrases as well as involuntary silent pauses or blocks in which the person who stutters is unable to produce sounds. This project adopts StutterNet, a time-delay neural network (TDNN) suitable for capturing contextual aspects of the disfluent utterances, and apply it on SEP-28k, a dataset containing over 28k clips labeled with five event types including blocks, prolongations, sound repetitions, word repetitions, and interjections, to tackle stuttering recognition task. To improve the modeling capability, we scale the original model via channel multiplier and perform ensemble learning with multiple model checkpoints.  Moreover, we propose a sliding-window style algorithm to allow audio input of arbitrary length while observing the changing dynamics of class-wise probability clearly .

## Model

Differing from the MFCC feature used in the original paper, we adopt a mel spectrogram with 80 filter banks and 16 kHz sample rate for feature extraction . Besides, we formulate the task as multiple binary classification problems. The features are fed into TDNN, which consists of five time delay layers with the first three focusing on the contextual frames of (t-2, t-1, t, t+1, t+2), (t-2, t, t+2), (t-3, t, t+3) with dilation of 1, 2 and 3 respectively. Then,  the hidden states will pass through a multi-layer perceptron (MLP) to get classification results. We use the ReLU activation function for each layer by default.
[![stutternet.png](https://imgos.cn/2024/08/16/66bee4a535ec8.png)](https://imgos.cn/2024/08/16/66bee4a535ec8.png)
*Quote from [StutterNet: Stuttering Detection Using Time Delay Neural Network](https://arxiv.org/abs/2105.05599).*

## Dataset

We train and test the model on [SEP28K dataset](https://github.com/apple/ml-stuttering-events-dataset/), a dataset containing over 28k clips labeled with five disfluency event types including blocks, prolongations, sound repetitions, word repetitions, and interjections. The data was originally drawn from public podcasts consisting of individual interviewees who suffer from speech  disorders.

[![屏幕截图 2024-08-16 135026.png](https://imgos.cn/2024/08/16/66bee7b042553.png)](https://imgos.cn/2024/08/16/66bee7b042553.png)
*Distribution of annotations. Quote from [SEP-28k: A Dataset for Stuttering Event Detection From Podcasts With People Who Stutter](https://arxiv.org/abs/2102.12394).*

## Training details

We trained two models in total, where the second model is scaled by a channel multiplier of X. The mean value of two outputs is then fed into the sigmoid function for binary classification. Since each sample can contribute to multiple classes with negative labels, there is an extreme imbalance between positive and negative labels in each class. To tackle this problem, we empirically assign a larger weight to sample with positive label when designing loss function: [BCEWithLogitsLoss](https://pytorch.org/docs/stable/generated/torch.nn.BCEWithLogitsLoss.html) is used in our project instead of the classic cross-entropy loss.

## Generating prediction

We propose a sliding-window style algorithm to allow audio input of arbitrary length while observing the changing dynamics of class-wise probability clearly More specifically, two thresholds are set for detecting sudden increase or decrease in predicted probabilities. Details can be seen in [tutorial](https://github.com/huazai6161/EC523-Final-Project/blob/main/StutterRecognition.ipynb).
[![屏幕截图 2024-08-16 174042.png](https://krseoul.imgtbl.com/i/2024/08/16/66bf2080cb04a.png)](https://krseoul.imgtbl.com/i/2024/08/16/66bf2080cb04a.png)
*Prediction map*
[![屏幕截图 2024-08-16 174151.png](https://krseoul.imgtbl.com/i/2024/08/16/66bf2080cf5c6.png)](https://krseoul.imgtbl.com/i/2024/08/16/66bf2080cf5c6.png)
*Classification map*
