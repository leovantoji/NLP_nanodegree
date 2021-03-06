## Intro to NLP
- Even though computers cannot understand unstructured texts like humans, computers can analyse documents to identify: 
  - Frequent & rare words.
  - Tone & sentiment of the message.
  - Clusters of documents.
- **Context** is everything when it comes to NLP.
- Common **NLP pipeline** includes:
  - Text processing.
  - Feature extraction.
  - Modelling.

## Text processing
- Text processing generally includes:
  - Cleaning (e.g. remove HTML tags, etc.).
  - Normalisation (e.g. convert texts to lower case, remove punctuations and extra spaces, etc.).
  - Stop word removal (e.g. remove words that are too common, etc.).
  - Identify different parts of speech (e.g. which words are nouns, verbs, or named entity, and convert words into canonical forms using stemming and lemmatisation).
- Remove HTML tags using **Beautiful Soup** library.
  ```python
  from bs4 import BeautifulSoup
  soup = BeautifulSoup(r.text, 'html5lib')
  print(soup.get_text())
  ```
- It's better to **remove punctuation characters with a space**. This approach makes sure that words don't get concatenated together, in case the original text did not have a space before or after the punctuation.
- **Tokenisation** is simply splitting each sentence into a sequence of words.
  ```python
  words = text.split()
  
  # use NLTK
  from nltk.tokenize import word_tokenize
  words = word_tokenize(text)
  ```
- Sentence tokenisation with `nltk`.
  ```python
  from nltk.tokenize import sent_tokenize
  sentences = sent_tokenize(text)
  ```
- List of **stop words** in English from `nltk`.
  ```python
  from nltk.corpus import stopwords
  print(stopwords.words('english'))
  
  # remove stop words
  words = [w for w in words if w not in stopwords.words('english')]
  ```
- **Part-of-speech tagging** with `nltk`.
  ```python
  from nltk import pos_tag
  # tag parts of speech
  sentence = word_tokenize('I always lie down to tell a lie.')
  pos_tag(sentence)
  ```
- Visualise the **parse tree**.
  ```python
  for tree in parser.parse(sentence):
    tree.draw()
  ```
- **Named entity recognition** is often used to index and search for news articles.
  ```python
  from nltk import pos_tag, ne_chunk
  from nltk.tokenize import word_tokenize
  
  # recognise named entities in a tagged sentence
  ne_chunk(pos_tag(word_tokenize('Antonio joined Udacity Inc. in California.')))
  ```
- **Stemming** is the process of reducing a word to its stem or root form. Stemming is meant to be a **fast and crude operation** carried out by applying very simple search and replace style rules. `nltk` has language-specific stemmers.
  ```python
  from nltk.stem.porter import PorterStemmer
  
  # reduce words to their stems
  stemmed = [PorterStemmer().stem(w) for w in words]
  ```
- **Lemmatisation** is another technique to reduce words to a normalised form using a dictionary to map different variants of a word back to its root. For example, `was`, `were`, and `is` can be reduced to `be`.
  ```python
  from nltk.stem.wordnet import WordNetLemmatizer
  
  # reduce words to their root form. Default pos is noun.
  lemmed = [WordNetLemmatizer().lemmatize(w) for w in words]
  
  # lemmatise verbs by specifying pos
  lemmed = [WordNetLemmatizer().lemmatize(w, pos='v') for w in lemmed]
  ```
- Summary:
  ![text_processing_summary](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/text_processing_summary.png)

## Spam Classifer
- The basic idea of **Bag of Words (BoW)** is to take a piece of text and count the frequency of words in that text. It is important to note that the BoW concept **treats each word individually** and the **order in which the words occur doesn't matter**.

## Part of Speech Tagging with Hidden Markov Models
- [From Wikipedia](https://en.wikipedia.org/wiki/Part-of-speech_tagging). In corpus linguistics, **part-of-speech-tagging (POS tagging)** is the process of marking up a word in a text (corpus) as corresponding to a particular part of speech, based on both its definition and its context - i.e., its relationship with adjacent and related words in a phrase, sentence, or paragraph. For example:
  - Sentence: Mary has a little lamb.
  - Noun (N): Mary
  - Verb (V): has
  - Determinant (Dt): a
  - Adjective (Ad): little
  - Noun (N): lamb
- Part-of-speech tagging is often used to help disambiguate natural language phrases because it can be done quickly with high accuracy. Tagging can be used for many NLP tasks like determining correct pronunciation during speech synthesis (for example, _dis_-count as a noun vs dis-_count_ as a verb), for information retrieval, and for word sense disambiguation.
- **Lookup tables** don't work very well with words having different tags.
- [From Wikipedia](https://en.wikipedia.org/wiki/Hidden_Markov_model). **Hidden Markov Model (HMM)** is a statistical Markov model in which the system being modeled is assumed to be a Markov process with unobservable (i.e. hidden) states.
- `λ = (A,B)` specifies an HMM in terms of an **emission** probability distribution `A` and a **state transition** probability distribution `B`.
  - The emission probabilities give the conditional probability of observing evidence values for each hidden state.
  - The transition probabilities give the conditional probability of moving between states during the sequence. 
- [From Wikipedia](https://en.wikipedia.org/wiki/Viterbi_algorithm). The **Viterbi algorithm** is a dynamic programming algorithm for finding the most likely sequence of hidden states—called the Viterbi path—that results in a sequence of observed events, especially in the context of Markov information sources and hidden Markov models (HMM). 

## Feature extraction and embeddings
- A **document-term matrix** is a mathematical matrix that describes the frequency of terms that occur in a collection of documents. Each document is a row, and each term is a column.
- **Count of common words** is a commonly used approach to match similar documents. Nonetheless, this approach has an **inherent flaw**. As the size of the document increases, the number of common words tend to increase even if the documents talk about different topics.
- **Cosine similarity** is a measure of similarity between two non-zero vectors of an inner product space that measures the cosine of the angle between them. This metric is used to measure how **similar the documents** are irrespective of their size. [Machine Learning Plus](https://www.machinelearningplus.com/nlp/cosine-similarity/).
![cosine_similarity_formula](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/Cosine-Similarity-Formula-1.png)
- **TF-IDF** (term frequency - inverse document frequency): *tfidf(t, d, D) = tf(t, d) x idf(t, D)*. TF-IDF is an innovative approach to assigning weights to words that signify their relevance in the document.
  - Term frequency *tf(t,d)* is the ratio between the raw count of a term, *t*, in a document, *d*, divided by the total number of terms in *d*.
  - Inverse document frequency *idf(t, D)* is the logarithm of the total number of documents in the collection, *D*, divided by the number of documents where *t* is present.
  - It is a way to score the importance of words (or "terms") in a document based on how frequently they appear across multiple documents.
  - If a word appears frequently in a document, it's important. Give the word a high score. But if a word appears in many documents, it's not a unique identifier. Give the word a low score. Therefore, common words like `the` and `for`, which appear in many documents, will be scaled down. Words that appear frequently in a single document will be scaled up.
- **Word embedding** is the collective name for a set of language modelling and feature learning techniques in natural language processing where words or phrases from the vocabulary are mapped to vectors of real numbers.
- The core idea of **Word2Vec** is that a model, which is able to predict a given word given neighbouring words or vice versa, is likely to capture the contextual meanings of words very well.
  - Neighbouring words: **Continuous Bag of Words**.
  - Middle word: **Continuous Skip-gram**.
- Properties of **Word2Vec**:
  - Robust, distributed representation.
  - Vector size independent of vocabulary.
  - Train once, store in lookup table.
  - Deep learning ready.
- **GloVe** (Global vectors for word representation) is an approach that tries to **optimise the vector representation of each word** by using **co-occurence statistics**.
- **t-SNE** (t-Distributed Stochastic Neighbouring Embedding) is a **dimensionality reduction technique** that can map high dimensional vectors to a lower dimensional space.

## Topic Modelling
- **Topic modelling** is a type of statistical modelling for discovering the abstract topics that occur in a collection of documents. **Latent Dirichlet Allocation (LDA)** is an example of topic model and is used to classify text in a document to a particular topic. [From MonkeyLearn](https://monkeylearn.com/blog/introduction-to-topic-modeling/)
- In statistics, **latent variables** (hidden variables) are variables that are **not directly observed** but are rather **inferred from other observed variables**. 
  - *BoW model* without latent variables has 500K parameters.
  ![Bag_of_Words](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/bag-of-words-quiz.png)
  - *Latent variable model* has only 15K parameters.
  ![Latent_variables](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/how-many-parameters-quiz.png)
- An **LDA** model **factors the BoW** model into **2 matrices**:
  - The first matrix indexes **documents by topic**.
  - The other matrix indexes **topics by word**.
- LDA is used to classify text in a document to a particular topic. It builds a topic per document model and words per topic model, modelled as Dirichlet distributions.
  - Each document is modelled as a multinomial distribution of topics and each topic is modelled as a multinomial distribution of words.
  - LDA assumes that the every chunk of text we feed into it will contain words that are somehow related. Therefore choosing the right corpus of data is crucial.
  - It also assumes documents are produced from a mixture of topics. Those topics then generate words based on their probability distribution.
- [From Wikipedia](https://en.wikipedia.org/wiki/Beta_distribution). In probability theory and statistics, the **beta distribution** is a family of continuous probability distributions defined on the interval *\[0, 1\]* parametrised by two **positive shape parametres**, denoted by *α* and *β*, that appear as exponents of the random variable and control the shape of the distribution. The **generalisation to multiple variables** is called a **Dirichlet distribution**. 
  ![Dirichlet_distributions](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/dirichlet_distributions.png)
  ![LDA](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/lda.png)
  ![Topic_model](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/topic_model.png)

## Sequence to Sequence
- [Keras blog](https://blog.keras.io/a-ten-minute-introduction-to-sequence-to-sequence-learning-in-keras.html): **Sequence-to-Sequence (seq2seq)** learning is about training models to convert sequences from one domain (e.g. sentences in English) to sequences in another domain (e.g. the same sentences translated to French). For example: `the cat sat on the mat` → seq2seq → `le chat etait assis sur le tapis`.
  ![seq2seq](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/seq2seq.png)
- **Applications** of seq2seq: Machine translation, question-answer, news-summary, captioning text, etc.
  ![captioning_text](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/captioning_text.png)
- **Seq2Seq Architecture**:
  - The inference process is done by handling inputs to the encoder.
  - The encoder summarises the inputs into a context variable or state.
  - The decoder proceeds to take the context and generate the output sequence.
  ![seq2seq_architecture](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/seq2seq_architecture.png)
  ![seq2seq_architecture_in_depth](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/seq2seq_architecture_in_depth.png)
- A seq2seq model works by **feeding one element of the input sequence at a time to the encoder**.

## Deep Learning Attention
- [LiLianWeng's github](https://lilianweng.github.io/lil-log/2018/06/24/attention-attention.html) Attention can be broadly interpreted as a vector of importance weights. In order to predict one element, such as a pixel in an image or a word in a sentence, we estimate using the attention vector how strongly it is correlated with other elements and take the sum of their values weighted by the attention vector as the approximation of the target.
- Limitation of seq2seq models which can be solved using attention methods:
  - The **fixed size of the context matrix** passed from the encoder to the decoder is a **bottleneck**.
  - **Difficulty of encoding long sequences** and **recalling long-term dependancies**.
- The **size of the context matrix** in an attention seq2seq model depends on the **length of the inpput sequence**.
- Seq2Seq **without Attention**:
![seq2seq_wo_attention](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/seq2seq_wo_attention.png)
- Seq2Seq **with Attention**:
  ![seq2seq_w_attention](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/seq2seq_w_attention.png)
- Every time step in the decoder requires calculating an attention vector in a seq2seq model with attention.
- **Attention Encoder**:
  ![attention_encoder_unrolled_view](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/attention_encoder_unrolled_view.png)
- **Attention Decoder**:
  ![attention_decoder_unrolled_view](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/attention_decoder_unrolled_view.png)
- **Context Vector Generation**:
  ![attention_context_vector_generation](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/attention_context_vector_generation.png)
- **Additive attention** (Bahdanau et al., 2015) uses a one-hidden layer feed-forward network to calculate the attention alignment where **v<sub>a</sub>** and **W<sub>a</sub>** are learned attention parameters. Analogously, we can also use matrices **W<sub>1</sub>** and **W<sub>2</sub>** to learn separate transformations for **h<sub>i</sub>** and **s<sub>j</sub>** respectively, which are then summed:
  ![additive_attention_formula1](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/additive_attention_formula1.png)
  ![additive_attention_formula2](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/additive_attention_formula2.png)
- **Multiplicative attention** (Luong et al., 2015) simplifies the attention operation by calculating the following function:
  ![multiplicative_attention_formula](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/multiplicative_attention_formula.png)
  ![multiplicative_attention_detail1](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/multiplicative_attention_detail1.png)
  ![multiplicative_attention_detail2](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/multiplicative_attention_detail2.png)
- The **intuition behind using dot product** as a scoring method is that the dot product of two vectors  in word-embedding space is a **measure of the similartiy** between them.
- The simplicity of **not having a weight vector** in multiplicative attention comes the drawback of assuming the encoder and decoder have the same embedding space. Thus, this might work for text summarisation where both the encoder and decoder use the same language and the same embedding space. For machine translation, since each language tends to have its own embedding space, we might want to use a second scoring method.
  ![multiplicative_attention_detail3](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/multiplicative_attention_detail3.png)
- Full view of multiplicative attention decoding phase:
  ![multiplicative_attention_decoding](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/multiplicative_attention_decoding.png)
- Computer Vision applications:
  ![cv_attention_application](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/cv_attention_application.png)
  ![cv_attention_application2](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/cv_attention_application2.png)
- **Self-attention**: Without any additional information, however, we can still extract relevant aspects from the sentence by allowing it to attend to itself using self-attention (Lin et al., 2017)
- The **Transformer**:
  ![transformer](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/transformer.png)

## Speech Recognition
- **VUI pipeline** includes:
  - Speech recognition: Voice to text.
    - Acoustic Model → Language Model → Accent Model
  - Text to text: Text input reasoned to text output.
  - Text to speech.
- **Automatic Speech Recognition (ASR)**: The goal is to input any continuous audio speech and output the text equivalent. ASR should be **speaker independent** and have **high accuracy**.
- **Models in speech recognition** can be divided into acoustic models and language models.
  - The **acoustic model** solves the problem of **turning sound signals** into some kind of **phonetic representation**.
  - The **language model** houses the **domain knowledge of words, grammar and sentence structure** for the language.
- The **challenges of ASR** are: 
  - Noises.
  ![noise](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/asr_challenge_noise.png)
  - Variability in pitch, volume and speed.
  ![asr_challenge_variability](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/asr_challenge_variability.png)
  ![asr_challenge_word_speed](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/asr_challenge_word_speed.png)
  - Ambiguity specific to languages.
  ![asr_challenge_language_knowledge](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/asr_challenge_language_knowledge.png)
  ![asr_challenge_spoken_vs_written](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/asr_challenge_spoken_vs_written.png)  
- **Signal analysis** refers to the audio signal produced by speech. Sound vibrations cause pressure waves in the air that can be detected with a microphone and transduced into a signal. Sinusoidal vibrations are created in the air when we speak. Higher pitches vibrate faster with a higher frequency than lower pitches.
  ![soundwave_to_audio](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/soundwave_to_audio.png)
- **Fourier analysis** is the study **decomposing mathematical functions** into sums of simpler trigonometric functions. Since sound comprises oscillating vibrations, we can use Fourier analysis and Fourier transforms to decompose an audio signal into component sinusoidal functions at varying frequencies. [More about Fourier Analysis](https://ibmathsresources.com/2014/08/14/fourier-transforms-the-most-important-tool-in-mathematics/).
- Our speech is made up of many frequencies at the same time. The signal is a sum of all the **component frequencies** stuck together. Component frequencies should be used as features. **Fast Fourier Transform (FFT)** is often used to break the signal into component frequencies.
  ![signal_component_freq](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/signal_component_freq.png)
  ![freq_comp](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/freq_comp.png)
- FFT can be used to create a **spectrogram** of an audio signal. A **spectrogram** is a **visual representation of the spectrum of frequencies of a signal** as it varies with time. Spectrograms are sometimes referred as **sonographs, voiceprints, or voicegrams** when applied to an audio signal. The **intensity of shading** indicates the **amplitude of the signal**.
  ![spectrogram](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/spectrogram.png)
- The **Mel Scale** was developed in 1937 and is based on human studies of pitch perception. At lower pitches (frequencies), humans can distinguish pitches better. 
  ![mel-scale](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/mel-scale.png)
- The **source/filter** model holds that the **source** of voices speech is dependent upon the **vibrations initiated in the vocal box**, and is unique to the speaker, while the **filter** is the **articulation of the words** in the forward part of the voice tract. The two can be separated through **Cepstrum analysis**.
  ![source-filter-model](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/source-filter-model.png)  
- The **source/filter** model motivates **Cepstral analysis**. 
  - The intuition is that the **source** *e(n)* is multiplied by the **filter** *h(n)* to form the signal *s(n)*: *s(n) = e(n) x h(n)*
  - This signal can be converted to the frequency domain through a discrete Fourier transform (DFT) (FFT can also be used): |*S(w)*| = |*E(w)*| ⋅ |*H(w)*|
  - Take the *log* and we can just add the source and filter instead of multiplying: log|*S(w)*| = log|*E(w)*| + log|*H(w)*|
  - By taking the **inverse discrete Fourier transform (IDFT)**, the signal can be split. This is the cepstrum *c(n)*: *c(n)* = IDFT(log|*S(w)*|) = IDFT(log|*E(w)*| + log|*H(w)*|)
  - Because we are splitting the logs of the frequencies, this is not the same as the original time domain, but rather now called the **quefrency** or **ceptral** domain. The vocal tract, or filter components, can be extracted now because they vary slowly and are concentrated in the lower quefrency region.
- **Mel Frequency Cepstrum Coefficient (MFCC)** analysis is the **reduction of an audio signal** to **essential speech component features** using both mel frequency analysis and cepstral analysis. The **range of frequencies** are **reduced** and **binned into groups of frequencies** that humans can distinguish. The signal is further **separated into source and filter** so that variations between speakers unrelated to articulation can be filtered away. [MORE ABOUT THIS](http://www.practicalcryptography.com/miscellaneous/machine-learning/guide-mel-frequency-cepstral-coefficients-mfccs/#steps-at-a-glance)
  ![mfcc](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/mfcc.png)
- **Changes in frequencies, deltas** and **changes in changes in frequencies, delta-deltas** might also be **meaningful features** in speech recognition.
- MFCC demo:
  ```python
  from python_speech_features import mfcc
  import scipy.io.wavfile as wav
  
  def wav_to_mfcc(wav_filename, num_cepstrum):
    """extract MFCC features from a wav file.
    Parameters:
    - wav_filename: filename with .wav extension.
    - num_cepstrum: number of cepstrum to return.
    
    Returns:
    - MFCC features for wav file.
    """
    
    # read .wav file
    (rate, signal_data) = wav.read(wav_filename)
    
    # extract mfcc features
    mfcc_features = mfcc(signal_data, rate, numcep=num_cepstrum)
    
    return mfcc_features
  ```
- **Phonetics** is a branch of linguistics for the **study of sounds of human speech**: physical properties, production, acoustics, articulation, etc.
- In any given language, a **phoneme** is the **smallest sound segment** that can be used to distinguish one word from another. For example, *bat* and *chat* have only one sound different but this changes the word. The phonemes in question are *B* and *CH*. What exactly these are and how many exist varies a bit and may be influenced by accents included. Generally, US English consists of 39 to 44 phonemes.
- A **grapheme** is the **smallest symbol** that distinguishes one written word from another. In US English, 26 letters and a space combine for 27 possible graphemes.
- A **lexicon** for speech recognition is a **lookup file** for converting speech parts to words. An example of this is **cmudict**, the Carnegie Mellon tool for speech recognition compatible with the open source **Sphinx** project. 
  ```
  AARDVARK AA R D V AA R K
  AARON EH R AH N
  AARON'S EH R AH N Z
  AARONSON EH R AH N S AH N
  ```
- **ARPAbet** is a set of phonemes developed by the Advanced Research Projects Agency (ARPA) for the Speech Understanding Project in the 1970s.
  
  |Phoneme|Example|Translation|
  |:-:|:-:|:-:|
  |AA|odd|AA D|
  |AE|at|AE T|
  |AH|hut|HH AH T|
  |ZH|seizure|S IY ZH ER|
- A **pangram** is a sentence using every letter of a given alphabet at least once.
- Hidden Markov Models are the primary probabilistic model in traditional ASR systems. 
- An **N-grams** is an ordered sequence of words.
  ![ngrams-numbers](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/ngrams-numbers.png)
- Probability of a series of words can be calculated from the chained probabilities of its history: 
  ![eqn-jointprob-words-in-sentence](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/eqn-jointprob-words-in-sentence.png)
- The probabilities of sequence occurences in a large textual corpus can be calculated this way and used as a language model to add grammar and contextual knowledge to a speech recognition system. However, there is a prohibitively large number of calculations for all the possible sequences of varying length in a large textual corpus. Therefore, to address this problem, we use the Markov Assumption to approximate a sequence probability with a shorter sequence:
  ![eqn-markov-assumption-ngrams](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/eqn-markov-assumption-ngrams.png)
- Some possible combinations may not exist in our probability dictionary but are still possible. We don't want to multiply in a probability of 0 just because our original corpus was deficient. This is solved through **smoothing**. A simple method is the **Laplace smoothing** with the **add-one** estimate where *V* is the size of the vocabulary for the corpus, i.e. the number of unique tokens.
  ![eqn-addone-bigram-smoothing](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/eqn-addone-bigram-smoothing.png)
- **Repeated multiplications of small probabilities** can cause **underflow problems** in computers when the values become too small. Therefore, we will calculate all probabilities in log space:
  *log(p<sub>1</sub> x p<sub>2</sub> x p<sub>3</sub> x p<sub>4</sub>) = log(p<sub>1</sub>) + log(p<sub>2</sub>) + log(p<sub>3</sub>) + log(p<sub>4</sub>)*
- **Connectionist Temporal Classification (CTC)** is a loss function useful for performing supervised learning on sequence data, without needing an alignment between input data and labels.

## Extracurricular
### Hyperparameters
- There are generally two types of hyperparameters:
  - Optimiser hyperparameters which are related to the optimisation and training process than to the model itself. These include learning rate, mini-batch size, and the number of training iterations or epochs.
  - Model hyperparameters which are involved in the structure of the model. These include the number of layers or model specific parameters for architecture.
- The **learning rate** is the single most important hyperparameter.
  ![learning_rate](https://github.com/leovantoji/Natural_Language_Processing_Nanodegree/blob/master/images/learning_rate.png)
- If the **mini-batch size** variable is too small, the training might be too slow. If the **mini-batch size** is too large, it could be computationally taxing (i.e. needs more memory) and could result in worse accuracy. Generally, 32, 64, 128, and 256 are potentially good starting values.
- The **number of training iterations** is a hyperparameter we can optimise automatically using a technique called **early stopping** (also "early termination").
- RNN Hyperparameters: cell structure like LSTM and GRU should both be tried in the task and compare. 
