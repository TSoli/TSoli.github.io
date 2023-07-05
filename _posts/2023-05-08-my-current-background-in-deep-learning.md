<!-- prettier-ignore -->
* TOC
{:toc}

## Intro

While this blog is to discuss what I learn in ELEC4630 about deep learning, this won't be the first
time I have learned about deep learning and so this intro blog will just discuss what my background
is.

## Introduction to Internet of Things (CS3237) @ NUS

My first exposure to deep learning was on my exchange in the second half of last year \(2022\) at
NUS where I took a course that introduced me to IoT. At the time, I chose the course because I had
enjoyed learning about embedded systems and had heard that this was a common application area for
embedded systems. The course did briefly go through some basics about embedded systems however it
did not assume much background so there was not too much depth. Instead, the course was quite broad
and covered the basics for many different parts of IoT. As a result, a significant portion of the
course was dedicated to machine learning and in particular, deep learning.

It began by introducing some basics of statistical learning and machine learning like linear
regression, decision trees, support vector machines e.t.c. Eventually neural networks were
introduced at a high level including Multi-Layer Perceptrons \(MLPs\), Convolutional Neural Networks
\(CNNs\), Recurrent Neural Networks \(RNNs\) and Generative Adversarial Networks \(GANs\). This was
quite interesting but the focus was more on high level understanding without too much of the
underlying mathematical details.

The programming for these neural nets made use of the [scikit-learn](https://scikit-learn.org/) and
[keras](https://keras.io/) libraries for Python and for our final project these are the frameworks
we used.

For the project we were attempting to show a proof of concept for a smart suitcase \(Smartcase\)
that could automatically follow you. To do this, the idea was to make a model that could identify
people based on their shoes. We used an existing model \(CNN\) for detecting shoes that we found
online and then added our own CNN with some of our own training data so that it could identify my
shoes. The robot would then turn to face me when it saw my shoes in the camera.

## Fencing Movement Classification

Since I really enjoyed the course and wanted to stay overseas for a bit longer, I asked the
professor if he knew of any internships or research projects and he told me about a Research
Assistant position available at the Singapore Sport Institute. The project involved attaching
Inertial Measurement Units \(IMUs\) which contain an accelerometer, gyroscope and magnetometer to
fencing athletes and using the collected data to classify fencing movements. The results were
presented in a GUI which I designed using [PyQt](https://wiki.qt.io/Qt_for_Python) so that the video
could be viewed along with the model's predictions. The main challenge was the limited amount of
data since it was still being collected while I designed the model. By the end of it, I had example
data for each movement and some bouts from eight athletes \(and more was to be collected after I
left\).

I first tried extracting my own features from the data such as peaks, troughs, median, e.t.c for
windows of data and passing this through an MLP. The results were fairly mediocre so I tried an RNN
since we were dealing with sequences of data so this seemed appropriate. This took much longer to
train, even with the limited data, and the results were not much better. Finally, I tried a
relatively simple CNN and this worked quite well and was able to achieve ~75-80% accuracy on data
from an unseen athlete.

## Deep Learning (STAT3007) @ UQ

Following my time on exchange I had grown more interested in learning about deep learning so I
decided I would take UQ's deep learning course the next semester when I got back \(right now!\).
This course has been quite interesting and definitely filled in some more of the mathematical
details about the models I had been using. It is also taught using [PyTorch](https://pytorch.org/)
which I had not used before however, it seems it is becoming increasingly popular in research and
industry compared to the main competitor, [TensorFlow](https://www.tensorflow.org/) which is what
Keras uses as a backend.

Currently, I am also working on a project for this course where our team is attempting to take
pre-trained backbones like [DINO](https://github.com/facebookresearch/dino) and add our own head to
perform pose estimation on using the [COCO Dataset](https://cocodataset.org/).

## Image Processing and Computer Vision (ELEC4630)

That takes us up to now and this course which is about image processing and computer vision. The
first half has been about traditional techniques which has been interesting and seems to be most
useful in cases where a high level of interpretability in the model is important \(something that
deep learning generally lacks\). On the other hand, deep learning is really where state-of-the-art
performance is possible currently. I am keen to get a new perspective on deep learning from the
fastAI course and continue to dive deeper into these models.
