# English-to-Hindi-NMT-Model-Using-RNN

## seq2seq Model
A seq2seq model is a language model that was first introduced by Google for machine translation. seq2seq model makes use of RNNs. LSTM is used in the version proposed by Google.
Two RNN layers work together as the main two components of the seq2seq model, i.e. the Encoder and the Decoder. The Encoder converts the input sequence into corresponding 
hidden vectors, and the Decoder takes as input the hidden vector generated by encoder, its own hidden states and current input word to produce the next hidden vector and 
finally predict the next word.

### 1. Encoder
The Encoder reads a sequence of k length in k timesteps. Each word of the sentence is the current input, x<sub>i</sub> for the model at each time step t<sub>i</sub>. The last 
state vectors, h<sub>k</sub> remember what sequence the network has read. These vectors are also known as the thought vectors. They summarize the whole input sequence in a 
vector form. y<sub>i</sub> represents the probability distribution of the next possible word at each time step t<sub>i</sub>. These outputs are discarded.
<br> For example, if the input sequence was ‘How are you?’, then the Encoder would look like this,
<div align="center"><img src="/images/encoder.png" height="300"></div>

### 2. Decoder (Training)
The Decoder generates the output sequence word-by-word. During the training of the network, a start token, e.g. &lt;start&gt;, is added to the beginning of the target 
sequence and an end token, e.g. &lt;end&gt;, is added at the end of the target sequence. The last internal states of the encoder, i.e. h<sub>k</sub> are set as the initial 
states of the Decoder. The start token, i.e. &lt;start&gt;, is given as the first input to the decoder. The first output is generated based on the &lt;start&gt; token and the 
initial state vectors. In training mode, rather than taking this output as the next input, the next word of the target sequence is taken as the next input for the decoder. This 
is known as teacher forcing method, where the actual output is taken as input at each time step rather than the predicted output. This helps the model to learn faster.
<br>For example, if we consider the previous example and let the target language be Hindi, then the target sequence would be ‘कैसे हो तुम?’. The Decoder would look like,
<div align="center"><img src="/images/decoder.png" height="300"></div>

### 3. Decoder (Testing)
Unlike the Encoder, the function of the Decoder during training and testing phase are different. In training mode, the actual output of one timestep is fed as the input for 
the next timestep, but in testing mode, the predicted output at one timestep is fed as the input for the next time step. During the testing phase, the Decoder will keep
generating output word-by-word until the end token, i.e. &lt;end&gt;, is generated.

### 4. Attention in seq2seq model
Attention mechanism tries to teach the model to only pay attention to the neighboring words not the whole text. To achieve this feature, we use a new interface between the
encoder and the decoder, called context vector, which represents the amount of attention given to words. In this paper we will look into the Bahdanau’s Additive Style
Attention. Here are the equations that are implemented:
<div align="center"><img src="/images/attention.png" height="300"></div>

## Dataset
The model is trained on a English-Hindi dataset containing nearly 11k sentence pairs in the format:
<br><br>how are you तुम कैसे हो 
<br>what time is it अभी ककतने बजे हैं
<br><br>The complete dataset is in lower case without any punctuation. The English-Hindi dataset has been collected from a variety of existing sources, like manythings.org/anki/,
OPUS and TDIL, and are from a variety of domain such as agriculture, entertainment, health, tourism and news.
<br><br>Some of the characteristics of the dataset are: 
<br>Number of English-Hindi Pairs - 11152
<br>English Vocabulary Size (Unique Words) - 15363 
<br>Hindi Vocabulary Size (Unique Words) - 18545
<br>Maximum English Sequence Length - 496
<br>Maximum Hindi Sequence Length - 397

## Encoder-Decoder Model Architecture
This section describes the architecture of the Encoder-Decoder Model. Both the encoder and the decoder consist of an Embedding layer with a embedding size of 256 and a GRU
layer with 1024 units. The decoder contains four additional Dense layers for attention and output.
<div align="center"><img src="/images/summary.png" height="600"></div>

## Results
For a test run and to get an idea of how the model performs, the model is trained on 100 English-Hindi pairs for 100 epochs using the sparse_categorical_crossentropy
loss function and TensorFlow’s AdamOptimizer(). It takes about 4-6 secs per epoch to train the network on a i5-8250U CPU. Even after training for such a short time on
only 100 samples, some words were still correctly translated, for example, ‘welcome’ got translated to ‘स्वागत’, ‘legs’ got translated to ‘पैर’, ‘who knows’ got translated 
to ‘ककसे पता’.
