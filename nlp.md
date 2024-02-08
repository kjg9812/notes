# 1/23/24
java -jar mae
ctrl-leftclick
first assignment is to label adjectives, along with their position and morphology

# 1/30/24
### Lowest level syntactic processing (text)
- tokenization and segmentation
    - given a sentence, determine the words or word like units that it consits of
- part of speech tagging
    - apply a set of part of speech tags to a set of tokens

### Low level syntactic processing
- named entity tagging
    - mark boundaries of names of type PERSON, ORGANIZATION, FACILITY, GPE, LOCATION,...
    - <ENAMEX TYPE = "PERSON">Adam Meyers </ENAMEX> works for <ENAMEX TYPE="ORGANIZATION">New York University</ENAMEX>
- chunking
    - mark verb groups and/or noun groups, convenient approximations of syntactic units
    - less expensive processors and nlogn, but lose some detail
    The book with the blue cover will end up on the shelf.
        - [NG The book] with [NG the blue cover][VG will end up] on [NG the shelf]

### Parsing: High level syntactic processing
- n^3 time

### semantics - ish
- semantics - a wide range of topics loosely referring to meaning
- ex. the word "bank"
    - some are financial, some are geographic detail, etc.

### word sense disambiguation
- interesting characterizations of word senses, use WordNet

### predicate argument structure
- meaning is expressed by saying which two things mean the same thing
- linguists have employed systems to characterize predicatable paraphrases
- semantic role labeling is popular to do this
    - "They were eaten by a giant clam" and "A giant clam ate them"
        - patient relationship between eat and they
        - agent relationship between eat and clam
    - "John took a walk to the store" and "John walked to the store"
        - mean the same
### anaphora
- coreference
    - two phrases mean the same thing
    - "Though big blue won the contract, this official is suspicious of IBM"
        - Big blue and IBM the same
- other varieties
    - john ate a sandwich and mary ate one also [type coref]
    - the amusement park is very dangerous. the gate has sharp edges. the rides have not been inspected. [bridging anaphora]
    - this book is valuable, but the other book is not [other coref]
        - same type but cant be the same book

### discourse argument structure
- adverbs, subordinate/coordinate conjunctions, among other words link clauses

### semantic parsing
- Professor's version is called GLARF

### role of manual annotation
- manually encode systems so that you can fine tune task definitions and guidelines
- several annotators must agree on classification most of the time
- if humans cant agree, it is unlikely that a computer can do the task at all
- used to create answer keys to score system output
    - recall
    - precision
    - F-score
ex.
correct = 8 // correct answers supplied by system
system = 10 //answers supplied by system
answer = 12 // answer key
correct/system == 8/10 // this is precision, what percentage of the system answers is correct
corrrect/answer = 8/12 // this is recall, percentage of right answers

### manual annotation in supervised statistical ML
- divide the corpus into sub corpora
    - a training corpous to acquire statistical patterns
    - a test corpus to measure system performance
    - a development corpus similar to test corpus

## lecture 2 slides
### formal language
- set of strings of symbols
- formal language can model a phenomenon eg. written english modeled after the sounds of english
- ex. all combinations of the letters A and B: ABAB,AABB,AAAB,etc
- ex. mathematical equations: 1+2=5, 2+3=4
- ex. musical notation

### formal grammar
- set of rules that matches all and only instances of a formal language
- in CS, formal grammars are used to generate and recognize formal languages (eg. programming languages)
- parsing a string of a language entails
    - recognizing the string and recording the analysis showing it is part of the language
    - a compiler translates from language X to language Y
- consists of:
    - N: finite set of nonterminal symbols
        - symbols that can be replaced by other symbols
    - T: finite set of terminal symbols
        - symbols that cannot be replaced by other symbols
    - R: set of rewrite rules eg. XYZ -> abXzY
        - replace the symbol
    - S: special nonterminal that is the start symbol

what is good grammar? chomsky hierarchy defines types of formal grammar
- from least expressive, most efficient processors to most expressive, least efficient

### regular expressions
- standardized way of representing search strings
'(BB)|([^B]{2})' //to be or not to be

# 2/6/24
### HMM and Part of Speech Tagging
HMM is describing the probability of the next part of speech based on the current state
Markov chain means each set of possibilities is only based on the current state
    - whatever is happening now directs what happens next

### viterbi algorithm
- two words: fish and sleep

viterbi[s,t] =
max(s') viterbi[s,t-1] (probability of previous token)
P(s|s)
P(..fsdjklfkj)
- s is state, t is a token

in our training corpus
- if fish appears 8 times as a noun and 5 times as a verb
- sleep appears twice as a noun and 5 times as a verb
Noun
- p(fish|noun): 0.8
- p(sleep|noun): 0.2
....


# video notes on Part of Speech Tagging
### part of speech
    - pronoun
    - verb
    - determiner
    - noun
    - etc.
- how do we get a computer to identify these?
    - also if we have words, what should we expect next?
#### Methods for this
- rule based
    - if -> then
    - ex. if theres a noun, then there will be a verb
    - based on linguistics
    - so many cases to consider though
- stochastic (probability based)
    - hidden markov model
        - "i eat pizza"
            - each word is an observed variable
            - hidden variables are the part of speech
            - so observing words, what is the most likely part of speech? maximize the combinations of part of speech that gives us the highest probability of these words
    - MAX P(p1...pn,w1,...,wn)
        - p being parts of speech
        - w being words
        - say n is 5 for p
            - then combinations is 5^10 -> 10 million combinations
            - this is way too large esepcially since there are more than 5 parts of speech
        - in comes **viterbi algorithm**
# Notes on Hidden Markov Model
- ex. professor is one of two moods (happy or sad)
    - mood on a given day depends only on mood from previous day
    - if professor is happy on t-1 (previous day) -> 0.7 change she'll be happy today and 0.3 change she'll be sad today
    - if sad on t-1 -> 0.5 happy 0.5 sad today
    - these are called the transition probabilities
- emission probabilities
    - color shirt she's wearing depends on mood of a given day
    - new distribution
    - if happy (0.8 red, 0.1 green, 0.1 blue)
    - if sad (0.2 red, 0.3 green, 0.5 blue)
- so the markov model contains transition probabilities (hidden states) and the emission probabilities (these are what we actually observe)
    - hidden states directly affect observed states
- say its 3 days in the course and professor wears green, blue, red shirt on those 3 consecutive days
C1 = G, C2 = B, C3=R
    - what are the most likely moods of the professor on these days?
    - so based on what we're seeing, what is the most likely hidden state?
Maximize the probability (c1,c2,c3,m1,m2,m3)
    - 3 colors on each day and 3 moods for each day
    - which combinations of moods gives us the highest probability
        - so the proabbility of seeing this color sequence with these moods are maximized -> so its the most likely sequence of moods

this is the probability broken up
P(c3 | c2c1m3m2m1)
P(c2 | c1m3m2m1)
P(c1 | m3m2m1)
P(m3 | m2m1)
P(m2 | m1)
P(m1)
- but the probability of color shirt on a certain day only depends on the mood on a certain day -> this is the assumption
- and the probability of a mood on one day only depends on the mood of the previous day -> the markov assumption
P(c3 | m3)
P(c2 | m2)
P(c1 | m1)
P(m3 | m2)
P(m2 | m1)
P(m1 | s)
- so the probabilities become this
- we find that the highest probability of moods is {S S H} sad sad happy

# Notes on Viterbi Algorithm
- key idea of Part of Speech tagging is to use hidden markov model
- but how do we do this efficiently?
#### an example
"THE FANS WATCH THE RACE"
- THE is a determiner
- FANS WATCH and RACE can be nouns or verbs
- 8 possibly combinations for part of speech tagging
- so we have p1, p2, p3, p4, p5 , one for each word
- we want MAX P(p1,p2,p3,p4,p5, THE, FANS, WATCH, THE, RACE)
- broken down this is MAX $\prod \mathbb{P}(p_k|p_{k-1}) \prod \mathbb{P}(w_k|p_k)$
    - transmissions first product
    - emissions second product
- go back to HMM probability list if confused, you can see that part of speech only depends on part of speech from previous part of speech and word only depends on the part of speech
- we need data for transitions and emissions probabilities
    - these are from empirical data -> underlying training set where parts of speech are labeled
    - this ends up in a 2d table with parts of speech on one axis, words on another axis, and their probabilities
        - ex. determiner and THE has probability 0.2
        - ex. noun and WATCH has probability 0.3
        - in our case these highest probabilities are wrong for our sentence! the viterbi alg with solve this, these are just raw probabilities from data
#### where are we saving time with viterbi?
- this is that flow tree
![image](images/viterbi.png)
- THE FANS WATCH THE RACE
- START -> THE
    - first question: given start of sentence what is probability that first word is determiner and that is the word THE
    - two numbers, one from transition, one from emission
        - 0.8 first word determiner, 0.2 determiner and the word THE
    - multiply together to get 0.16
        - this number represents if we cut off the sentence right there, whats the probability of observing this information, so so far Determiner start and THE
- next pretend we assign FANS as a noun
    - whats the probability we transition from determiner to noun 0.9
    - whats the probability that the noun is FANS 0.1
    - multiply with previous 0.16 -> 0.0144
        - so this is THE FANS with FANS as a noun
- but what if FANS is a verb
    - 0.1 and 0.2 (same process as above) to get 0.0032 with THE FANS with FANS as a verb
    - this is lower prob, why not just stop this branch?
        - we dont know whats gonna come next, the prob could be higher later in this chain
- basically multiply part of speech transition (verb to noun, noun to verb, etc) and probability that the word is a certain part of speech (THE is a determiner, WATCH is noun or verb, etc)
- you can discontinue paths that have a lower probability if there are two pathways to the same node
    - ex DT NN NN has 0.026 and DT VB NN has 0.00048 so DT NN NN is smaller but we arrived at the same NN node
    - this is why Viterbi is more efficient!
- so get to the last word and you should have possibly multiple probabilities
    - choose the higher one
    - in this case it was [DT NN VB DT NN], which is correct
    - THE FANS WATCH THE RACE
