# Splitting Data for Machine Learning

1. TOC
{:toc}

## What are Machine Learning Models?

The goal of a machine learning model is to learn the underlying process that causes something to
happen. In order to do this, we provide inputs that are related to the expected results. For
example, if we want to predict a person's age we might provide information like their height,
weight, gender and ethnicity which are known as the input parameters of the model and are often
denoted $x_1, x_2, ..., x_n = \mathbf{X}$. In this case, we are trying to predict the age which is
our output often denoted $y$. The idea is that by providing lots of examples, we can design a system
to produce a function that can predict $y = f(\mathbf{X})$ with reasonable accuracy.

Of course, finding this function is not always trivial. Even given the previous example, how would
you design a function that predicts the age from the provided inputs? Is height more important than
weight? How much does ethnicity matter and how can we even measure it? Should the model be a linear
combination of these inputs or is the function a bit more complicated? In general, machine learning
helps us answer some of these questions but still leaves us to make decisions about others. In any
case, it provides the system to help us find a good estimate for this function.

## What's with all of the Different Parameters?

If you have read a little bit about machine learning you have probably heard the word *parameters* a
lot. Broadly speaking, we can break down the parameters in a machine learning model into three
categories: input, model and hyper - parameters.

### Input Parameters

Input parameters are the most straight forward and we touched on them before. They are the data we
collect and provide to our model in order for it to make predictions. In the previous example these
were the things like height, weight, ethnicity e.t.c and are usually denoted $\mathbf{X}$.

Input parameters can be broadly categorised as numerical or categorical. In our previous example,
things like height and weight would be numerical whereas ethnicity would be categorical. While
numerical input parameters can be used directly, the categorical ones must be converted to a
numerical representation. It is common to use integers starting from 0 and increasing by 1 in these
cases.

### Model Parameters

Model parameters are the parameters in the function that the model learns in order to fit to the
training data. For example, if we wanted to predict height, $y$ based on weight, $x$ then be may be
able to approximate it with a linear equation $y = ax + b$. In this case, the model parameters would
be $a$ and $b$ and our machine learning system would attempt to find these model parameters such
that the error between the actual data and the function is minimised. The details of this are
usually based on using gradient descent on the loss function but that will be a discussion for
another day. The main takeaway here is that these model parameters are automatically tuned using an
algorithm that we choose and they directly affect the output of the model.

Another important note is that the goal os for the model to have a model that can generalise well to
unseen data. Therefore, the goal of minimising the loss function on a training set is just a proxy
for this actual goal and it is important to note that the two are not the same \(more on this
later\).

### Hyperparameters

Hyperparameters are then the parameters or choices made by the programmer that affect how the model
is learned. Hyperparameters can refer to the features of the model --- for example, what order
polynomial to use for a regression problem --- or it can refer to parameters for the optimiser \(the
algorithm that helps to choose the best model parameters\) --- for example, the learning rate or
even kind of optimiser used to minimise the loss function which is itself another hyperparameter.

### In the Deep Learning Context

So overall, we have input parameters, $\mathbf{X}$ which are the data that the model uses to make
predictions, we then set some hyperparameters for our model in that help it to learn good model
parameters. In the context of deep learning, the input parameters are represented using some input
vector and then layers of nodes are specified \(commonly known as "neurons"\). The number of nodes
in each layer, number of layers and some properties of these nodes and the connections between them
are then some of the hyperparameters of the model. Usually these nodes compute the weighted sum of
the outputs of nodes from the previous layer and then apply an activation function \(another
hyperparameter\) to this sum. The weights, often denoted by $\mathbf{w}$ and so the goal of the
optimiser is often framed as

$\argmin_{\mathbf{w}} L((\mathbf{X}, y), \mathbf{w})$

where $L$ is the specified loss function.

![](/images/mlp.png)

## Splitting Data

### Train and Validation Sets

As mentioned above, when choosing a model, the goal is to maximise the generalisation performance
--- that is the performance on unseen data. As a result, it is common to split the available data
into sections so that some can be used to train the model and some can be reserved to test the
model. Normally, these sets are referred to as the train and validation sets, respectively. The
training set is passed through the model and the loss function is used with the optimiser in order
to tune the model parameters \(weights, $\mathbf{w}$\) and then the validation set is used to make
sure the model is generalising well to data it is has not been directly optimised for.

If the model's train set performance is significantly better than the validation set performance,
this indicates the model has learned specific features or noise in the training data and is not
generalising well to the unseen validation data. This is commonly referred to as overfitting and
indicates that the model is too expressive \(although a small generalisation gap is expected between
the train and validation sets\). There are many techniques that can be used to reduce overfitting
however, for this post the goal is just to point out how to identify it.

### So What is a *Test* Set?

Ontop of the train and test sets, it is also common to reserve a test set. While the goal of the
validation set was to ensure that the model parameters were not biased to overfit to the training
data, the test set is to ensure that the hyperparameters are not too biased towards the validation
set used. So, overall, we reserve some data for training which is used to optimise the model
parameters, the validation set is then used to gauge the model's performance and pick a model and
it's hyperparameters and the test set is used to ensure that the model is not biased towards the
hyperparameters and still has good generalisation performance.

### Do we Always Need a Test Set?

The test set is not always necessary and in some cases it is not used. For example, if you did not
perform any hyperparameter tuning and were happy with the initial model results, the hyperparameters
cannot be biased towards the validation data and so a test set is not necessary. Similarly, in cases
where the amount of data is limited, it can be hard to justify reserving a portion solely for
testing and instead,
[cross-validation](https://en.wikipedia.org/wiki/Cross-validation_(statistics)#:~:text=Cross%2Dvalidation%20is%20a%20resampling,model%20will%20perform%20in%20practice.)
is commonly used to get a good indication of the model's generalisation performance.