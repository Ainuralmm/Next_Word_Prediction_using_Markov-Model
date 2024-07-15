**Markov Model**

Let’s understand what a Markov model is before we dive into it. Simply stated, Markov model is a model that obeys Markov property.

So, what is Markov property? In a process wherein the next state depends only on the current state, such a process is said to follow Markov property. For example, let’s say that tomorrow’s weather depends only on today’s weather or today’s stock price depends only on yesterday’s stock price, then such processes are said to exhibit Markov property. Mathematically speaking, the conditional probability distribution of the next state depends on the current state and not the past states. That is s(t) depends only on s(t-1), where s(t) is the state at time t. This is what is called as the first-order Markov model.

In general, if the current state of a system depends on n previous states, then it is called n-th order Markov model.

A sequence of events which follow the Markov model is referred to as the Markov Chain.

**Next word prediction**

Now let’s take our understanding of Markov model and do something interesting. Suppose we want to build a system which when given an incomplete sentence, the system tries to predict the next word in the sentence. So, how do we take a word prediction case as in this one and model it as a Markov model problem? Treat every word as a state and predict the next word based on the previous state, as simple as that. This case is a perfect fit for Markov chain.

Wait, but how do you do that? Enter probability distribution. Let’s understand this better with a simple example. Consider the three simple sentences -

I like Photography.
I like Science.
I love Mathematics.


All the unique words from above sentences that is ‘I’, ‘like’, ‘love’, ‘Photography’, ‘Science’ and ‘Mathematics’ could form the different states. The probability distribution is all about determining the probability of transition from one state to another, in our case, it is from one word to another. In our scenario, it is clear from the above examples that first word always starts out with the word ‘I’. So there is 100% chance that the first word of the sentence will be ‘I’. For the second state, we have to choose between the words ‘like’ and ‘love’. Probability distribution now is all about determining the probability that the next word will be ‘like’ or ‘love’ given that the previous word is ‘I’. For our example, we can see that the word ‘like’ appears in 2 of the 3 sentences after ‘I’ whereas the word ‘love’ appears only once. Hence there is approximately 67% (2/3) probability that ‘like’ will succeed after ‘I’ and 33% (1/3) probability for ‘love’. Similarly, there is 50–50 chance for ‘Science’ and ‘fruits’ to succeed ‘like’. And ‘love’ will always be followed by ‘Mathematics’ in our case.

<img width="517" alt="Screenshot 2024-07-15 at 13 24 10" src="https://github.com/user-attachments/assets/80eb757a-f173-4f55-8853-151dc2d35b42">

Representing the above work Mathematically as conditional probabilities 

<img width="544" alt="Screenshot 2024-07-15 at 13 26 05" src="https://github.com/user-attachments/assets/72a34716-2d73-4556-ad42-6806cd10daf1">

This is how we build a probability distribution from a sample data.

**Generating Adele style song**

We will train a Markov model on a bunch of Adele song lyrics and then try to generate a new song lyrics from the model. A typical case of Markov chain. 
For the new song generation, we will make use of a 2nd-order Markov model. At first, we need to clean up the data and then train a Markov model on the cleaned up data.



