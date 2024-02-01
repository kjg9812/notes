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

# 2/1/24
