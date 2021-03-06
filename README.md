# TEXTGEN
Constructed a character-level LSTM with PyTorch. The network will train character by character on some text, then generate new text character by character. As an example, I will train on Anna Karenina. This model will be able to generate new text based on the text from the book.
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
<p> <h2> Hyperparameters: </h2> </p>
<p> Here are the Hyperparameters for the network: </p>
<h4> In defining the Model: </h4>
<ul>
  <li> n_hidden - The number of units in the hidden layers. </li>
  <li> n_layers - Number of hidden LSTM layers to use. </li>
</ul>
<h4> In training the Model: </h4>
<ul>
  <li> batch_size - Number of sequences running through the network in one pass. </li>
  <li> seq_length - Number of characters in the sequence the network is trained on. Larger is better typically, the network will learn more long range dependencies. But it takes longer to train. 100 is typically a good number here.. </li>
  <li> lr - Learning rate for training. </li>
</ul>

<p> <h2> Steps Followed: </h2> </p>
<ul>
  <li> Load in text data </li>
  <li> Pre-process that data, encoding characters as integers and creating one-hot input vectors </li>
<li> Define an RNN that predicts the next character when given an input sequence </li>
  <li> Train the RNN and use it to generate new text. </li>
 </ul>
<p> <h2> Sample Data: </h2> </p>

### Input:
```
text[:500]
```

### Output:
```
Chapter 1\n\n\nHappy families are all alike; every unhappy family is unhappy in its own\nway.
\n\nEverything was in confusion in the Oblonskys' house. The wife had\ndiscovered that the husband was carrying on an intrigue with a
French\ngirl, who had been a governess in their family, and she had announced to\nher husband that she could not go on living in the
same house with him.\nThis position of affairs had now lasted three days, and not only the\nhusband and wife themselves, but all the
members of their f
```
<p> <h2> Characters generated by the machine: <h2> </p>
  
### Input:
```
print(sample(loaded, 2000, top_k = 10, prime="This is Geekquad "))
```

### Output:
```

This is Geekquad to his way in it. The life in enting sounds, and the sudden obsires
of
his left flying close and simple sounds and bedoother's face in their despatro department in
the bails only one of outsider and from his bidget, and has handed his eyes, and as he should not give this;
then were sole feeling by now, to his
position--there was nothing but a teals still had to
sit down. Only filled her terrible
besige, seeing Lizaveta Petrovna to
his whole discussion so as for his what they were phusping in from the
curtsing in the
conversation with Alexey Alexandrovitch's head.



Chapter 22


When he had never come up to him, and that the predict of his caped the
coachman could see worse, so at that
duries of that, too, to small, but he had been both her hander.

"I beg now and conditions away, but if no one
can't except me of interest, but I cannot believe."

She listened, but she flung ooter out
of side, starting away from his hat, but she
would not look at her. The condessed official cut. He saw that
one one had all
finished that he had gone
up to his music and the trass.

"I had fault, why I
will go to her methods of the carriage, and she's so lending it all the weed.
The public little poars as to scrien and
thespefflengy, and it was many.
I'm asked.... Who is she to blame?" asked Levin, and his life,
and turning on its appulled atticul things.
 "I'm sitting for you and hear your lips tonoy?"
asked Sergey Ivanovitch,
washed, but it was not being caused in this spiritual comparisome,
cheeking at the sight of her presenter
interested had
been comprehending angrily.

"No, he wanted to ask that!"

"Told me
is that
me for the last moment."

Levin lrowed agreements on his coan and then two regumed, and
after dearing a steps in his father, and scared in with her
strings of the hotliey one
steps on her husband, who
was an ited of his heart of his across where he always had
fancied an
tame. "If these case, they less new sets-off illnassses is ig not so much as it must be that," said Se
```
 
 
