# Character-Level-Language-Model
I have constructed a character-level LSTM with PyTorch. The network will train character by character on some text, then generate new text character by character. As an example, I will train on Anna Karenina. This model will be able to generate new text based on the text from the book.
<p> The network is based on Andrej Karpathy's <a href="karpathy.github.io/2015/05/21/rnn-effectiveness/">post on RNNs</a> and <a href="https://github.com/karpathy/char-rnn"> implementation in Torch.</a> </p>
<p> This code implements multi-layer Recurrent Neural Network (RNN, LSTM, and GRU) for training/sampling from character-level language models. In other words the model takes one text file as input and trains a Recurrent Neural Network that learns to predict the next character in a sequence. The RNN can then be used to generate text character by character that will look like the original training data. </p>
<p> As a working example, suppose we only had a vocabulary of four possible letters “helo”, and wanted to train an RNN on the training sequence “hello”. This training sequence is in fact a source of 4 separate training examples: 1. The probability of “e” should be likely given the context of “h”, 2. “l” should be likely in the context of “he”, 3. “l” should also be likely given the context of “hel”, and finally 4. “o” should be likely given the context of “hell”. </p>

<p> Concretely, we will encode each character into a vector using top_k encoding (i.e. all zero except for a single one at the index of the character in the vocabulary), and feed them into the RNN one at a time with the step function. We will then observe a sequence of 4-dimensional output vectors (one dimension per character), which we interpret as the confidence the RNN currently assigns to each character coming next in the sequence. Here’s a diagram: </p>
<p> <img src="https://raw.githubusercontent.com/geekquad/deep-learning-v2-pytorch/master/recurrent-neural-networks/char-rnn/assets/charseq.jpeg" width="800" height="600"> </p>
<p> <h2> LSTM: </h3> </p>
<P> An LSTM has a similar control flow as a recurrent neural network. It processes data passing on information as it propagates forward. The differences are the operations within the LSTM's cells. These operations are used to allow the LSTM to keep or forget information. </p>
<p> <h3> Below is an Image to show how LSTM works: </h3> </p>
<p> <img src="https://raw.githubusercontent.com/geekquad/deep-learning-v2-pytorch/master/recurrent-neural-networks/char-rnn/assets/charRNN.png" width="800" height="600"> </p>
<p> <h2> Sequence Batching: </h2> </p>
<p> To train on this data, I have created mini-batches for training. Remember out batches should be multiple sequences of some desired number of sequence steps. </p>
<ul>
  <li>The first thing is we need to discard some of the text so we only have completely full mini-batches. </li>
  <li>After that, we need to split arr into N batches. </li>
  <li>Now when we have this array, we can iterate through it to get our mini-batches. </li> 
</ul>
<p> <h3> This is how sequence batching works: </h3> </p>
<img src="https://raw.githubusercontent.com/geekquad/deep-learning-v2-pytorch/master/recurrent-neural-networks/char-rnn/assets/sequence_batching%401x.png">
<p> <h2> Sample Data: </h2> </p>

#### Input:
```
text[:500]
```

#### Output:
```
Chapter 1\n\n\nHappy families are all alike; every unhappy family is unhappy in its own\nway.
\n\nEverything was in confusion in the Oblonskys' house. The wife had\ndiscovered that the husband was carrying on an intrigue with a
French\ngirl, who had been a governess in their family, and she had announced to\nher husband that she could not go on living in the
same house with him.\nThis position of affairs had now lasted three days, and not only the\nhusband and wife themselves, but all the
members of their f
```
