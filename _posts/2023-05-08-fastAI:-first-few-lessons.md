<!-- prettier-ignore -->
* TOC
{:toc}

## Some Familiar Useful Tools

Many of the tools introduced so far I have some level of familiarity with. I have used Python since
my first semester at uni four years ago, I've also used Jupyter Notebooks before in both the IoT
course I took at NUS and also in the Deep Learning course I am taking right now. I have used git for
most of my projects that involve any programming since after my intro coding course \(CSSE1001\)
that I took in my first semester of uni however, I have not used it with other collaborators very
much and have mostly just used it for version control in my own projects. It seems I probably won't
be doing any collaborating with other students for this project either so that may be something to
investigate more another time.

## Some Unfamiliar Useful Tools

There are also some new tools I was not previously familiar with. The most important is probably
GitHub Codespaces. This is nice since [Dr Lovell](https://researchers.uq.edu.au/researcher/327), the
course coordinator for this course has setup a nice linux environment with GPU acceleration in the
cloud for us. It means all have access to the same hardware and we don't have to muck around
installing all the necessary libraries as this has been done for us. This is great since it still
allows us to customise our environment but when it comes to marking the marker can just use our
codespace and not have to worry about installing all of the same software/libraries that we used for
compatibility. I am a big fan of this as a tool for teaching courses like this.

## Efficiently Cleaning Data

The first few videos have been quite broad and mostly just high level examples of what deep learning
can do. The most interesting thing I learned so far was Jeremy's approach to cleaning data. In his
example, he scraped some images from an image search on [Duck Duck Go](https://duckduckgo.com/) to
find examples for his image classifier. Of course, some images just would not load and those could
be fairly easily removed with fastAI's high level API. What was more interesting is how Jeremy
handled the harder case which is images that load but are not valid examples. In general, cleaning
data is important and not necessarily trivial. If you were scraping large datasets in this way you
might spend days or weeks if you were to clean the data by going through each image individually and
ensuring that it is valid. Instead, Jeremy takes a much more pragmatic approach. Since he is using a
pre-trained network \(and presumably only training a small number of extra layers at the end\) and
the majority of images collected are probably valid he chooses to train the model on the "dirty"
data first. I found this quite unexpected however the reason seems to be that given most of the
scraped images _are_ valid, the model should still do fairly well at classifying the valid images.
Therefore, once it is trained, the images that were classified with the largest error are likely to
be the invalid images. The fastAI API makes it easy to view these images and move them to a
different category or delete them entirely if necessary. While for a large dataset it may be likely
that _some_ invalid images make it through, being able to find the worst examples efficiently like
this is quite clever I thought and I will keep it in mind in the future.

Despite this, there are probably some limitations to this technique. If the model is not pre-trained
it may still work but probably not as well, especially if you accidentally overfit. It is also not
guaranteed to remove all the invalid images unless you look through all of them which would defeat
the purpose. The fact that you are sorting by the error may introduce some bias when reviewing the
invalid images which may affect how you choose to classify them. The harder examples of invalid
images are less likely to be found since they will have lower error and you may not view them. This
may make the model worse at hard examples.

## Some Recommended Improvements for the Course (ELEC4630)

I really think that the previous parts of the course could take some of the tools from this part on
fastAI and standardise them a bit more. The most obvious choice in my opinion would be to swap from
only supporting MATLAB in the tutorials/practicals and instead use Python. I think this makes a lot
of sense since Python appears to be more popular in industry since it is free whereas MATLAB
requires a paid license. Furthermore, it has a plethora of libraries for traditional computer vision
techniques such as [OpenCV](https://opencv.org/) and [scikit-image](https://scikit-image.org/) that
are efficient because they are written in C/C++ while still providing similar APIs to MATLAB
functions. This is similarly true for numerical and machine learning applications with libraries
such as [NumPy](https://numpy.org/), [pandas](https://pandas.pydata.org/),
[scikit-learn](https://scikit-learn.org/stable/index.html), [PyTorch](https://pytorch.org/) and
[TensorFlow](https://www.tensorflow.org/) making Python the defacto standard for data analysis and
machine learning.

[Jupyter Notebooks](https://jupyter.org/) which are also extensively used in these fields and for
teaching and communication are also available in Python and could serve as self-documenting code
that may even be appropriate for replacing submitted reports if held to a high standard
\(alternatively, they are still great for documenting progress\). This can be used in conjunction
with GitHub Codespaces to make verifying code easy for the markers. Jupyter Notebooks also work as
great interactive tutorial exercises which this course currently lacks. I really think that moving
forward these are compelling reasons to bite the bullet and consider updating the delivery of the
first half of this course.

Anyways, that's my two cents. I mean you could probably argue C++ would work since most of the same
Python libraries are written in it but Python provides such a nice high level interface for this
stuff.
