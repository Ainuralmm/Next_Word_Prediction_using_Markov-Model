**Markov Model**

Let’s understand what a Markov model is before we dive into it. Simply stated, Markov model is a model that obeys Markov property.

So, what is Markov property? In a process wherein the next state depends only on the current state, such a process is said to follow Markov property. For example, let’s say that tomorrow’s weather depends only on today’s weather or today’s stock price depends only on yesterday’s stock price, then such processes are said to exhibit Markov property. Mathematically speaking, the conditional probability distribution of the next state depends on the current state and not the past states. That is s(t) depends only on s(t-1), where s(t) is the state at time t. This is what is called as the first-order Markov model.

In general, if the current state of a system depends on n previous states, then it is called n-th order Markov model.

A sequence of events which follow the Markov model is referred to as the Markov Chain.

The Markov Property in mathematical notation as follows:
<img width="637" alt="Screenshot 2024-07-15 at 20 54 11" src="https://github.com/user-attachments/assets/5eb4781d-6732-4a16-8333-9901ec3a487a">

The state space of a Markov chain, which is the set of all possible states, can be anything: letters, numbers, weather conditions, baseball scores, or stock performances. The transition from one state to another is governed by a transition matrix, which represents the probability distribution of the state's transitions. The sum of probabilities in each row of the matrix will be one, implying that this is a stochastic matrix.

Markov chains have a wide range of applications in various fields. They are used in statistical models of real-world processes, such as studying cruise control systems in motor vehicles, queues or lines of customers arriving at an airport, currency exchange rates, and animal population dynamics. In data science, a common application of Markov chains is text prediction, which is commonly used in the tech industry by companies like Google, LinkedIn, and Instagram. And in this project we will focus in implementing MC in next word prediction. But before let's talk about the order of a Markov chain.

The order of a Markov chain refers to the number of preceding states that influence the probability of a given state in a stochastic process. There are three main types of Markov chains:

1)_First-order Markov chain_ — In this type of Markov chain, the probability of a state depends only on the immediately preceding state. The transition probability is represented as <img width="94" alt="Screenshot 2024-07-15 at 21 02 03" src="https://github.com/user-attachments/assets/fe22bf3d-04c4-4b14-baef-0136a3de7f17">

<img width="195" alt="Screenshot 2024-07-15 at 21 06 59" src="https://github.com/user-attachments/assets/8826e01b-9b98-4ea1-a488-31afbcd27ad6">


2)_Second-order Markov chain_ — A second-order Markov chain takes into account two preceding states, with the transition probability represented as <img width="122" alt="Screenshot 2024-07-15 at 21 03 08" src="https://github.com/user-attachments/assets/d4685537-06ed-4936-99a3-42f5675ea0e3">

<img width="211" alt="Screenshot 2024-07-15 at 21 08 18" src="https://github.com/user-attachments/assets/205fa2e0-37ea-4018-b74b-b0c709a12cdf">

Higher, nth-order chains tend to "group" particular notes together, while 'breaking off' into other patterns and sequences occasionally. These higher-order chains tend to generate results with a sense of phrasal structure, rather than the 'aimless wandering' produced by a first-order system



3)_Variable-order Markov model (VOM)_ — This model extends the concept of Markov chains by allowing the number of conditioning random variables to vary. It can approximate and generate strings using fewer conditional probabilities compared to higher-order Markov chains.

In practical settings, there is often insufficient data to accurately estimate the order of a Markov chain. However, understanding the order of a Markov chain is crucial for determining the degree to which the generated text will resemble the source text. Higher-order Markov chains can generate more complex and diverse sequences, while first-order Markov chains tend to produce simpler and more repetitive patterns.


**Next word prediction**

<img width="1193" alt="Screenshot 2024-07-15 at 23 12 49" src="https://github.com/user-attachments/assets/9260778f-838f-4431-8259-849767b39af3">

Now let’s take our understanding of Markov model and do something interesting. Suppose we want to build a system which when given an incomplete sentence, the system tries to predict the next word in the sentence. So, how do we take a word prediction case as in this one and model it as a Markov model problem? Treat every word as a state and predict the next word based on the previous state, as simple as that. This case is a perfect fit for Markov chain.

Wait, but how do you do that? Enter probability distribution. Let’s understand this better with a simple example. Consider the three simple sentences -

_I like Photography.
I like Science.
I love Mathematics._


All the unique words from above sentences that is ‘I’, ‘like’, ‘love’, ‘Photography’, ‘Science’ and ‘Mathematics’ could form the different states. The probability distribution is all about determining the probability of transition from one state to another, in our case, it is from one word to another. In our scenario, it is clear from the above examples that first word always starts out with the word ‘I’. So there is 100% chance that the first word of the sentence will be ‘I’. For the second state, we have to choose between the words ‘like’ and ‘love’. Probability distribution now is all about determining the probability that the next word will be ‘like’ or ‘love’ given that the previous word is ‘I’. For our example, we can see that the word ‘like’ appears in 2 of the 3 sentences after ‘I’ whereas the word ‘love’ appears only once. Hence there is approximately 67% (2/3) probability that ‘like’ will succeed after ‘I’ and 33% (1/3) probability for ‘love’. Similarly, there is 50–50 chance for ‘Science’ and ‘fruits’ to succeed ‘like’. And ‘love’ will always be followed by ‘Mathematics’ in our case.

<img width="517" alt="Screenshot 2024-07-15 at 13 24 10" src="https://github.com/user-attachments/assets/80eb757a-f173-4f55-8853-151dc2d35b42">

Representing the above work Mathematically as conditional probabilities 

<img width="544" alt="Screenshot 2024-07-15 at 13 26 05" src="https://github.com/user-attachments/assets/72a34716-2d73-4556-ad42-6806cd10daf1">

This is how we build a probability distribution from a sample data.

**Generating Adele style song**

We will train a Markov model on a bunch of Adele song lyrics and then try to generate a new song lyrics from the model. A typical case of Markov chain. 
For the new song generation, we will make use of a 2nd-order Markov model. At first, we need to clean up the data and then train a Markov model on the cleaned up data.

he training of the Markov model can be divided into the following stages -

1) Tokenisation
2) Building the state pairs
3) Determining the probability distribution
4) Let’s understand the procedure with a simple sentence -

_"I must have called a thousand times"_

At first, we need to perform tokenisation. Tokenisation is nothing but breaking down the sentence into words.

The second stage consists of forming the previous and current state pairs. Since we are building a 2nd-order Markov model, our previous state will consist of two words. For our example sentence, the pairs will be something like this -

_(I,must)-->have_

_(must,have)-->called_

_(have,called)-->a..._

Additionally, we have to consider two peculiar cases. Namely, the first word and the second word. Both of them will not have two previous words. So, we have to handle them differently. For the first word, we will just calculate the initial state distribution. And for the second word, we will treat it as a 1st-order Markov model, since it contains one previous word. Finally, for the end of the sentence, we will add an additional identification token ‘END’ and form pairs like

_(thousand times)-->END_

Once we have formed the state pairs, in stage 3 all we need to do is perform simple counts and calculate the probability of the next states possible for a given current state as before. For example, the word ‘I’ can be followed by the words ‘must’ or ‘have’. We need to build a probability distribution as follows -

_I --> [must, have]_

_{must: 1/2, have: 1/2}_

Once we have completed the training, we will have the initial word distribution, second-word distribution and the state transition distributions. Next to generate song all we need is to write a function to sample out from the above-created distributions.

That’s it. We are now ready to test out our song generator. One of the sample lyrics generated by our Markov model -

_i dont have to explain myself to you_

_out of all the things youd say_

_teetering on the edge of heaven and hell_

_i must have called a thousand times_

_high tides high tides high tides_

_when it fell you rose claim it_

_i know that its wrong_

_face it all cause youre all ive got left_

_but i set fire to the rain_

_where you go i give as good as i touched your face_

Yes, it like Adele and it didn’t make much sense... However, the sentences kind of make sense but the whole prose doesn’t connect properly. This is mainly due to the fact that Markov model only considers the previous state and neglects the past which indeed results in loss of information. This is what we refer to as the memoryless property of a stochastic process. 



**References:**
1) https://en.wikipedia.org/wiki/Markov_chain
2) https://klu.ai/glossary/markov-chain
3) https://cw.fel.cvut.cz/b172/_media/courses/bin/markov-chains-2.pdf
4) https://www.sciencedirect.com/science/article/pii/S0360544204002609
5) https://www.weberlo.com/guides/markov-chain-attribution-model
6) https://python.plainenglish.io/word-prediction-with-markov-chains-in-python-d685eed4b0b3
7) https://medium.com/ymedialabs-innovation/next-word-prediction-using-markov-model-570fc0475f96



