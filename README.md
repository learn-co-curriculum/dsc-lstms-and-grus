
# LSTMs and GRUs

## Introduction

In this lesson, you'll learn about two advanced types of neurons that typically outperform basic RNNs, **_Long Short Term Memory Cells_** and **_Gated Recurrent Units_**! You'll explore the problems they solve that increase their effectiveness compared to traditional vanilla RNNs, and compare and contrast the two neurons types to get a feel for what exactly they do and how they do it!

## Objectives

You will be able to:

- Explain why vanishing and exploding gradients exist when training RNNs 
- Describe the basic architecture and function of a GRU 
- Describe the architecture and function of an LSTM cell 


## RNNs and Gradient Problems

One of the biggest problems with standard Recurrent Neural Networks is that they get **_Saturated_**. The problem with this it that they use a sigmoid or tanh activation function, and there are large areas of each function where the derivative is very, very close to 0. When the derivatives are low, this means the weight updates are small, which means that the "learning" of the model slows to a crawl! This happens because after many, many weight updates, many weights will have been pushed into an extremely positive or extremely negative value. All you have to do is get past -5 or +5 to get to very small values. 

<img src='images/new_vanishing_gradient.png'>

When gradients are close to 0 because the values are extremely low, this is called **_Vanishing Gradient_**. Similarly, networks can also get to the point where the gradients are much, much large, resulting in massive weight updates that cause the model to thrash between 1 extremely wrong answer and another. When this happens, it is called **_Exploding Gradient_**. In practice, you can easily solve exploding gradients by just "clipping" the weight updates by bounding them at a maximum value. However, there's no good solution for vanishing gradients!

An intuitive way to think of this in terms of Information Theory -- the network is trying to encapsulate too much information from all of the time steps. Take a look at the following diagram, which you saw in the previous lesson. Pay attention to the colors that represent each word:

<img src='images/unrolled.gif'>

Notice how the further along the sequence goes, the less overall area the navy blue color (for the first word, "What") gets. As each new word in the sequence gets processed, the amount of "room" the RNN has to remember things gets saturated. It turns out, remembering too many things is a pretty surefire way to get your model to crash and burn. This makes it hard for dealing with long-term dependencies in the data. For instance, consider the following sentence:

> "Marilyn studied in France during the summer and fall semesters of college in 2016. As a result, she speaks fluent {_}"

If you were to use a traditional RNN to predict the next word in this sentence, it would likely have trouble figuring out the answer because of the number of time steps between the word to predict and the word that contains the information necessary to make a prediction, "France". 


## Remembering and Forgetting

This is where the modern versions of RNNs come in. In practice, when building models for sequence data, people rarely use traditional RNN architectures anymore. Instead they make use of **_LSTMs_** and **_GRUs_**. Both of these models can be thought of as special types of neurons that can be used in an RNN. Although they work a little differently, they have the same strength -- the ability to **_forget information_**!  By constantly updating their internal state, they can learn what is important to remember, and when it is okay to forget it. 

Consider the word prediction example you just looked at. You clearly need to remember the word "France", but there are plenty of words in between France and the word you need to predict that aren't that important, and you can safely ignore, such as "during the", "and", "of", etc. Furthermore, let's assume that the model learns enough to answer this question, but the next thousand words in the sequence is about something completely different. Do you really still need to hold on to the information about where Marilyn studied? How can you tell when you need to remember something and when you need to forget something? This is where GRUs and LSTMs have different approaches. Let's take a quick look at how they both work. 


## Gated Recurrent Units (GRUs)

**_Gated Recurrent Units_**, or **_GRUs_**, are a special type of cell that passes along it's internal state at each time step. However, not every part of the internal state is passed along, but only the important stuff! GRUs make use of two "gate" functions: a **_Reset Gate_**, which determines what should be removed from the cell's internal state before passing itself along to the next time step, and an **_Update Gate_**, which determines how much of the state from the previous time step should be used in the current time step. 

The following technical diagram shows the internal operations of how a GRU cell works. Don't worry about trying to understand what every part of this diagram means. Internally, its just some equations for the update and reset operations, coupled with matrix multiplication and sigmoid functions. Instead, focus on the the $S_t$ line, which moves from left to right and denotes the state being updated and passed onto the next layer. 

<img src='images/new_gru.png' width="400">

## Long Short Term Memory Cells (LSTMs)

**_Long Short Term Memory Cells_**, or **_LSTMs_**, are another sort of specialized neurons for use in RNNs that are able to effectively learn what to remember and what to forget in sequence models. 

LSTMs are generally like GRUs, except that they use three gates instead of two. LSTMs have: 

* an **_Input Gate_**, which determines how much of the cell state that was passed along should be kept
* a **_Forget Gate_**, which determines how much of the current state should be forgotten
* an **_Output Gate_**, which determines how much of the current state should be exposed to the next layers in the network

As you can see, they essentially accomplish the same thing as GRUs, but they do it in a slightly different way. Both models do a great job learning patterns from sequences, even when they are long and extremely complex! You'll find a diagram of a LSTM cell below. Just like with GRUs, don't worry about what the symbols mean or the math behind it. You can always pick that up later if you're curious. Instead, try to focus on how the information flows through this diagram from left to right, and where the various gates are for each function performed!

<img src='images/new_LSTM3_chain.png' width="800">

There's no good answer yet as to whether GRUs or LSTMs are superior to one another. In practice, GRUs tend to have a slight advantage in many use cases, but this is far from guaranteed. The best thing to do is to build a model with each and see which one does better. 

## Summary

In this lesson, you learned about how LSTMs and GRUs can help the models avoid problems such as vanishing and exploding gradients when working with large sequences of data. You also learned about the structure of LSTMs and GRUs, and how they are able to "forget" information!
