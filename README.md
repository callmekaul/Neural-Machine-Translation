# NEURAL MACHINE TRANSLATION MODELS

Developed as part of a research internship at IIT, Varansi. 

### RNNs

An RNN is created by applying similar weights recursively to a differentiable graph in topological order. In an RNN, all inputs are related and in each loop the relation between these inputs helps predict the next sequence.

![RNN Diagram](https://github.com/callmekaul/Neural-Machine-Translation/master/images/rnn.png?raw=true)

For input x(0), there is an output h(0), which is also passed to the next step along with the input x(1). This gives output h(1), which is the input for the next step and so on. This helps the RNN give context from previous inputs in the sequence.

### Attention

The attention mechanism is combined with RNNs to help them focus on a specific part of the input sequence and predict a specific part of the output sequence. This allows for more efficient learning. 
In our encoder-decoder model, the context vector passed from encoder carries the encoding of the entire sentence. The attention decoder can focus on different parts for each of the decoders own outputs. This is done by calculating a set of attention weights that are multiplied by the encoder vector. The result is the information about the input sentence that can help the decoder determine the output.

### Our Model

The model we have used is a sequence to sequence, or encoder-decoder model. It uses its own output as input for next steps. The encoder outputs a vector derived from input sequences of source language. This output, along with the context vector is fed into the decoder which in turn converts them into the output sequences.

![Seq2Seq Diagram](https://github.com/callmekaul/Neural-Machine-Translation/master/images/seq2seq.png?raw=true)

The encoder reads each symbol x in a sequence sequentially and changes the hidden state according to the equation :-

**h(t0) = f ( h(t-1) , x(t) )**

This continues until the encoder reaches the end of sentence token, at which point the hidden state is the summary of the entire sequence. 

The decoder of the model is trained to make an output sequence by predicting the next symbol yt, according to the given hidden state, C. The hidden state of the decoder is computed based on the equation:-

**h(t0) = f ( h(t-1), y(t-1), c )**

At every step of decoding, the decoder is given an input token and hidden state. The initial input token is the start of string token. The output of the encoder is used as the initial hidden state of the decoder.
Our decoder is also an attention decoder, hence it has the ability to focus on specific parts of the input sequence to generate specific parts of the output sequences, making it more efficient.

Attention weights are calculated through a feed-forward layer attn, using the decoder’s input and hidden state as inputs. Since the sentences can be of varying sizes, to create and train this layer we give it a maximum sentence length. Sentences of the maximum length use all the attention weights, while the shorter sentences only use the first few.

### Fine-Tuning

Fine tuning falls under transfer learning. It is the process by which we can utilize the training of an already trained machine learning model for some task. This model is fine-tuned to train a model on a second related task. For this paper, we have trained a base model and a new, fine-tuned model for translation on the same training data, and then compared their results using the same test data. 
The base model is an encoder-decoder model. For the new model, we train the encoder from scratch but take the decoder of the base model. The training is done in two parts. First, we freeze the decoder and train the model for some time. Then we unfreeze the decoder and train the model a little longer.

