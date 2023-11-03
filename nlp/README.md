# nlp stack

```bash

┌─────────────────────────┐
│                         │
│                         │
│                         │                               ┌──────────────────┐
│                         │                               │                  │
│                         │                               │                  │
│                         ├──────────────────────────────►|                  │
│                         │                               │                  │
│                         │                               │                  │
│                         │                               │                  │
│                         │                               │                  │
└─────────────────────────┘                               │                  │
                                                          └──────────────────┘

 ```




desc

## install|usage

---


### [decisiontree 0.5.0](https://github.com/igrigorik/decisiontree)

ID3-based implementation of the M.L. Decision Tree algorithm

<details>
<summary><i>example</i></summary>


```ruby
require 'decisiontree'

attributes = ['Temperature']
training = [
  [36.6, 'healthy'],
  [37, 'sick'],
  [38, 'sick'],
  [36.7, 'healthy'],
  [40, 'sick'],
  [50, 'really sick'],
]

# Instantiate the tree, and train it based on the data (set default to '1')
dec_tree = DecisionTree::ID3Tree.new(attributes, training, 'sick', :continuous)
dec_tree.train

test = [37, 'sick']
decision = dec_tree.predict(test)
puts "Predicted: #{decision} ... True decision: #{test.last}"

# => Predicted: sick ... True decision: sick

# Specify type ("discrete" or "continuous") in the training data
labels = ["hunger", "color"]
training = [
        [8, "red", "angry"],
        [6, "red", "angry"],
        [7, "red", "angry"],
        [7, "blue", "not angry"],
        [2, "red", "not angry"],
        [3, "blue", "not angry"],
        [2, "blue", "not angry"],
        [1, "red", "not angry"]
]

dec_tree = DecisionTree::ID3Tree.new(labels, training, "not angry", color: :discrete, hunger: :continuous)
dec_tree.train

test = [7, "red", "angry"]
decision = dec_tree.predict(test)
puts "Predicted: #{decision} ... True decision: #{test.last}"

# => Predicted: angry ... True decision: angry
```
</details>


### [scalpel 0.2.1](https://github.com/louismullie/scalpel)

Scalpel is a sentence segmentation tool for Ruby. It allows you to split a text into an array of sentences. It is simple, lightweight, blazing fast and does not require any domain-specific training. It works well even in the face of ill-formatted texts.

<details>
<summary><i>example</i></summary>

```ruby
require 'scalpel'
Scalpel.cut("some text")
```
</details>

### [fuzzy-string-match 1.0.1]()
<details>
<summary><i>example</i></summary>

calculate Jaro Winkler distance.

```ruby
require 'fuzzystringmatch'
jarow = FuzzyStringMatch::JaroWinkler.create( :native )
p jarow.getDistance(  "jones",      "johnson" )
```
</details>

### [fuzzy_tools 1.0.0](githubrepo)
<details>
<summary><i>example</i></summary>


Easy, high quality fuzzy search in Ruby.

```ruby
require 'fuzzy_tools'

books = [
  "Till We Have Faces",
  "Ecclesiastes",
  "The Prodigal God"
]

# Search for a single object

books.fuzzy_find("facade")                                   # => "Till We Have Faces"
books.fuzzy_index.find("facade")                             # => "Till We Have Faces"
FuzzyTools::TfIdfIndex.new(:source => books).find("facade")  # => "Till We Have Faces"

# Search for all matches, from best to worst

books.fuzzy_find_all("the")                             # => ["The Prodigal God", "Till We Have Faces"]
books.fuzzy_index.all("the")                            # => ["The Prodigal God", "Till We Have Faces"]
FuzzyTools::TfIdfIndex.new(:source => books).all("the") # => ["The Prodigal God", "Till We Have Faces"]

# You can also get scored results, if you need

books.fuzzy_find_all_with_scores("the") # =>
# [
#   ["The Prodigal God",   0.443175985397319 ],
#   ["Till We Have Faces", 0.0102817553829306]
# ]
books.fuzzy_index.all_with_scores("the") # =>
# [
#   ["The Prodigal God",   0.443175985397319 ],
#   ["Till We Have Faces", 0.0102817553829306]
# ]
FuzzyTools::TfIdfIndex.new(:source => books).all_with_scores("the") # =>
# [
#   ["The Prodigal God",   0.443175985397319 ],
#   ["Till We Have Faces", 0.0102817553829306]
# ]
```
</details>

### [verbal_expressions 0.1.5](https://github.com/ryan-endacott/verbal_expressions)
<details>
<summary><i>example</i></summary>


Verbal Expressions is a library that makes constructing difficult regular expressions simple and easy!


```ruby
# Create an example of how to test for correctly formed URLs
tester = VerEx.new do
  start_of_line
  find 'http'
  maybe 's'
  find '://'
  maybe 'www.'
  anything_but ' '
  end_of_line
end

# Create an example URL
test_url = "https://www.google.com"

# Use it just like a regular Ruby regex:
puts 'Hooray!  It works!' if tester.match(test_url)
puts 'This works too!' if tester =~ test_url

# Print the generated regex:
puts tester.source # => /^(http)(s)?(\:\/\/)(www\.)?([^\ ]*)$/
```
</details>

### [engtagger 0.4.0](https://github.com/yohasebe/engtagger)
<details>
<summary><i>example</i></summary>


A Ruby port of Perl Lingua::EN::Tagger, a probability based, corpus-trained tagger that assigns POS tags to English text based on a lookup dictionary and a set of probability values.

```ruby
require 'engtagger'

# Create a parser object
tgr = EngTagger.new

# Sample text
text = "Alice chased the big fat cat."

# Add part-of-speech tags to text
tagged = tgr.add_tags(text)

#=> "<nnp>Alice</nnp> <vbd>chased</vbd> <det>the</det> <jj>big</jj> <jj>fat</jj><nn>cat</nn> <pp>.</pp>"

# Get a list of all nouns and noun phrases with occurrence counts
word_list = tgr.get_words(text)

#=> {"Alice"=>1, "cat"=>1, "fat cat"=>1, "big fat cat"=>1}

# Get a readable version of the tagged text
readable = tgr.get_readable(text)

#=> "Alice/NNP chased/VBD the/DET big/JJ fat/JJ cat/NN ./PP"

# Get all nouns from a tagged output
nouns = tgr.get_nouns(tagged)

#=> {"cat"=>1, "Alice"=>1}

# Get all proper nouns
proper = tgr.get_proper_nouns(tagged)

#=> {"Alice"=>1}

# Get all past tense verbs
pt_verbs = tgr.get_past_tense_verbs(tagged)

#=> {"chased"=>1}

# Get all the adjectives
adj = tgr.get_adjectives(tagged)

#=> {"big"=>1, "fat"=>1}

# Get all noun phrases of any syntactic level
# (same as word_list but take a tagged input)
nps = tgr.get_noun_phrases(tagged)

#=> {"Alice"=>1, "cat"=>1, "fat cat"=>1, "big fat cat"=>1}
```
</details>


### [jongleur 1.1.1](https://gitlab.com/RedFred7/Jongleur)
<details>
<summary><i>example</i></summary>


Launches, schedules and manages tasks represented in a DAG, as multiple processes

![sample ml pipeline](https://miro.medium.com/v2/resize:fit:640/format:webp/1*Jz2UVqrVYj1hIwHxOpw2VQ.png)

![example DAG diagram](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*3eEtimgIPEAuizSWtu6HyQ.png)

![another example DAG](https://dagshub.com/blog/content/images/2021/07/Tred-G.svg)

```ruby
# https://gitlab.com/RedFred7/Jongleur#usage
```
</details>

### [composable_operations 0.10.1](http://github.com/t6d/composable_operations)
<details>
<summary><i>example</i></summary>


Composable Operations is a tool set for creating operations and assembling multiple of these operations in operation pipelines.

```ruby
class DateExtractor < ComposableOperations::Operation

  processes :text

  def execute
    text.scan(/(\d{4})-(\d{2})-(\d{2})/)
  end

end
```
</details>

### [ferret 0.11.8.7](https://github.com/dbalmain/ferret)
<details>
<summary><i>example</i></summary>


Ferret is a super fast, highly configurable search library.

```ruby
require 'rubygems'
require 'ferret'

index = Ferret::I.new(:key => :id)

[
  {:id => '1', :text => 'one'},
  {:id => '2', :text => 'Two'},
  {:id => '3', :text => 'Three'},
  {:id => '1', :text => 'One'}
].each {|doc| index << doc}

puts index.size                       # => 3
puts index['1'].load.inspect          # => {:text=>"One", :id=>"1"}
puts index.search('id:1').to_s(:text)
    # => TopDocs: total_hits = 1, max_score = 1.287682 [
    #            3 "One": 1.287682
    #    ]
```
</details>

### [langchainrb](https://github.com/andreibondarev/langchainrb)

<details>
<summary><i>example</i></summary>



```ruby
```
</details>

### [tokenizers 0.4.1](https://github.com/ankane/tokenizers-ruby)
<details>
<summary><i>example</i></summary>


Fast state-of-the-art tokenizers for Ruby

```ruby
tokenizer = Tokenizers.from_pretrained("bert-base-cased")

# https://github.com/ankane/tokenizers-ruby#training
```
</details>


### [eqn 1.6.5](https://github.com/schneidmaster/eqn)
<details>
<summary><i>example</i></summary>


A gem to evaluate mathematical equations. Includes support for variables and functions.

```ruby
Eqn::Parser.parse('1 + 1')
# => <syntax tree is printed>
```
</details>

### [google_search_results 0.0.1](https://github.com/serpapi/google-search-results-ruby)
<details>
<summary><i>example</i></summary>


Get Google Search Results via SERP API. Hash, JSON, and HTML outputs supported.

```ruby
# https://github.com/serpapi/google-search-results-ruby#supported-search-engine
```
</details>

### [lemmatizer 0.2.2](http://github.com/yohasebe/lemmatizer)
<details>
<summary><i>example</i></summary>


Lemmatizer for text in English. Inspired by Python's nltk.corpus.reader.wordnet.morphy package.

```ruby
require "lemmatizer"

lem = Lemmatizer.new

p lem.lemma("dogs",    :noun ) # => "dog"
p lem.lemma("hired",   :verb ) # => "hire"
p lem.lemma("hotter",  :adj  ) # => "hot"
p lem.lemma("better",  :adv  ) # => "well"

# when part-of-speech symbol is not specified as the second argument,
# lemmatizer tries :verb, :noun, :adj, and :adv one by one in this order.
p lem.lemma("fired")           # => "fire"
p lem.lemma("slow")            # => "slow"
```
</details>

### [llm_memory 0.1.14](https://github.com/shohey1226/llm_memory)
<details>
<summary><i>example</i></summary>


LLM Memory is a Ruby gem designed to provide large language models (LLMs) like ChatGPT with memory using in-context learning. This enables better integration with systems such as Rails and web services while providing a more user-friendly and abstract interface based on brain terms.

![](https://user-images.githubusercontent.com/1880965/239105431-b77d0efa-3fec-4549-b98a-eae510de5c3d.png)

```ruby
docs = LlmMemory::Wernicke.load(:file, "/tmp/a_directory")
# docs is just an array of hash.
# You don't have to use load method but
# create own hash with having content and metadata(optional)
# docs = [{
#   content: "Hi there",
#   metadata: {
#     file_name: "a.txt",
#     timestamp: "20201231235959"
#   }
# },,,]

hippocampus = LlmMemory::Hippocampus.new
res = hippocampus.memorize(docs)

query_str = "What is my name?"
related_docs = hippocampus.query(query_str, limit: 3)
#[{
#   vector_score: "0.192698478699",
#   content: "My name is Mike",
#   metadata: { ... }
#},,,]

# ERB
prompt = <<-TEMPLATE
Context information is below.
---------------------
<% related_docs.each do |doc| %>
<%= doc[:content] %>
file: <%= doc[:metadata][:file_name] %>

<% end %>
---------------------
Given the context information and not prior knowledge,
answer the question: <%= query_str %>
TEMPLATE

broca = LlmMemory::Broca.new(prompt: prompt, model: 'gpt-3.5-turbo')
message = broca.respond(query_str: query_str, related_docs: related_docs)

...
query_str2 = "How are you?"
related_docs = hippocampus.query(query_str2, limit: 3)
message2 = broca.respond(query_str: query_str2, related_docs: related_docs)
```
</details>

### [ohm 3.1.1](http://soveran.github.io/ohm/)
<details>
<summary><i>example</i></summary>


Ohm is a library that allows to store an object in Redis, a persistent key-value database. It has very good performance.


```ruby
```
</details>

### [ohm-contrib 3.0.0](http://github.com/cyx/ohm-contrib)
<details>
<summary><i>example</i></summary>


Includes a couple of core functions such as callbacks, timestamping, typecasting and lots of generic validation routines.

```ruby
```
</details>

### [pdf_paradise 0.2.3](https://rubygems.org/gems/pdf_paradise)
<details>
<summary><i>example</i></summary>


This gem is called pdf_paradise. It attempts to be a swiss army knife, a wrapper, over pdf-related functionality in general, by tapping into external projects, such as prawn, qpdf and so forth. For more information, please have a look at the documentation of the project on the homepage of this gem, at: https://www.rubydoc.info/gems/pdf_paradise/

```ruby
```
</details>

### [beckett 0.1.0](https://github.com/coreyti/beckett)
<details>
<summary><i>example</i></summary>


A Markdown renderer, using Kramdown, to convert to JSON, HTML5 or a Ruby Hash.

```ruby
```
</details>

### [pragmatic_segmenter](https://github.com/diasks2/pragmatic_segmenter)
<details>
<summary><i>example</i></summary>


Pragmatic Segmenter is a sentence segmentation tool for Ruby. It allows you to split a text into an array of sentences. This gem provides 2 main benefits over other segmentation gems - 1) It works well even with ill-formatted text 2) It works for multiple languages


```ruby
text = "Hello world. My name is Mr. Smith. I work for the U.S. Government and I live in the U.S. I live in New York."
ps = PragmaticSegmenter::Segmenter.new(text: text)
ps.segment
# => ["Hello world.", "My name is Mr. Smith.", "I work for the U.S. Government and I live in the U.S.", "I live in New York."]

# Specify a language
text = "Այսօր երկուշաբթի է: Ես գնում եմ աշխատանքի:"
ps = PragmaticSegmenter::Segmenter.new(text: text, language: 'hy')
ps.segment
# => ["Այսօր երկուշաբթի է:", "Ես գնում եմ աշխատանքի:"]

# Specify a PDF document type
text = "This is a sentence\ncut off in the middle because pdf."
ps = PragmaticSegmenter::Segmenter.new(text: text, language: 'en', doc_type: 'pdf')
ps.segment
# => ["This is a sentence cut off in the middle because pdf."]

# Turn off text cleaning and preprocessing
text = "This is a sentence\ncut off in the middle because pdf."
ps = PragmaticSegmenter::Segmenter.new(text: text, language: 'en', doc_type: 'pdf', clean: false)
ps.segment
# => ["This is a sentence cut", "off in the middle because pdf."]

# Text cleaning and preprocessing only
text = "This is a sentence\ncut off in the middle because pdf."
ps = PragmaticSegmenter::Cleaner.new(text: text, doc_type: 'pdf')
ps.clean
# => "This is a sentence cut off in the middle because pdf."
```
</details>


### [pragmatic_tokenizer 3.2.0](https://github.com/diasks2/pragmatic_tokenizer)
<details>
<summary><i>example</i></summary>


A multilingual tokenizer to split a string into tokens.

```ruby
text = "\"I said, 'what're you? Crazy?'\" said Sandowsky. \"I can't afford to do that.\""

PragmaticTokenizer::Tokenizer.new.tokenize(text)
# => ["\"", "i", "said", ",", "'", "what're", "you", "?", "crazy", "?", "'", "\"", "said", "sandowsky", ".", "\"", "i", "can't", "afford", "to", "do", "that", ".", "\""]

# You can pass many different options to #initialize:
options = {
  language:            :en, # the language of the string you are tokenizing
  abbreviations:       ['a.b', 'a'], # a user-supplied array of abbreviations (downcased with ending period removed)
  stop_words:          ['is', 'the'], # a user-supplied array of stop words (downcased)
  remove_stop_words:   true, # remove stop words
  contractions:        { "i'm" => "i am" }, # a user-supplied hash of contractions (key is the contracted form; value is the expanded form - both the key and value should be downcased)
  expand_contractions: true, # (i.e. ["isn't"] will change to two tokens ["is", "not"])
  filter_languages:    [:en, :de], # process abbreviations, contractions and stop words for this array of languages
  punctuation:         :none, # see below for more details
  numbers:             :none, # see below for more details
  remove_emoji:        :true, # remove any emoji tokens
  remove_urls:         :true, # remove any urls
  remove_emails:       :true, # remove any emails
  remove_domains:      :true, # remove any domains
  hashtags:            :keep_and_clean, # remove the hastag prefix
  mentions:            :keep_and_clean, # remove the @ prefix
  clean:               true, # remove some special characters
  classic_filter:      true, # removes dots from acronyms and 's from the end of tokens
  downcase:            false, # do not downcase tokens
  minimum_length:      3, # remove any tokens less than 3 characters
  long_word_split:     10 # split tokens longer than 10 characters at hypens or underscores
}
```
</details>

### [ruby-spacy 0.2.2](https://github.com/yohasebe/ruby-spacy)
<details>
<summary><i>example</i></summary>


ruby-spacy is a wrapper module for using spaCy from the Ruby programming language via PyCall. This module aims to make it easy and natural for Ruby programmers to use spaCy. This module covers the areas of spaCy functionality for using many varieties of its language models, not for building ones.

|    | Functionality                                      |
|:---|:---------------------------------------------------|
| ✅ | Tokenization, lemmatization, sentence segmentation |
| ✅ | Part-of-speech tagging and dependency parsing      |
| ✅ | Named entity recognition                           |
| ✅ | Syntactic dependency visualization                 |
| ✅ | Access to pre-trained word vectors                 |
| ✅ | OpenAI Chat/Completion/Embeddings API integration  |

```ruby
require "ruby-spacy"
require "terminal-table"

nlp = Spacy::Language.new("en_core_web_sm")
doc =nlp.read("Apple is looking at buying U.K. startup for $1 billion")

rows = []

doc.ents.each do |ent|
  rows << [ent.text, ent.start_char, ent.end_char, ent.label]
end

headings = ["text", "start_char", "end_char", "label"]
table = Terminal::Table.new rows: rows, headings: headings
puts table
```

```markdown
| text       | start | end | label   |
|:-----------|------:|----:|:--------|
| 任天堂     | 0     | 3   | ORG     |
| 1983年     | 4     | 9   | DATE    |
| ファミコン | 10    | 15  | PRODUCT |
| 14,800円   | 16    | 23  | MONEY   |
```
</details>

### [tomoto 0.3.3](https://github.com/ankane/tomoto-ruby)
<details>
<summary><i>example</i></summary>


High performance topic modeling for Ruby

```ruby
model = Tomoto::LDA.new(k: 2)
model.add_doc(["tokens", "from", "document", "one"])
model.add_doc(["tokens", "from", "document", "two"])
model.add_doc(["tokens", "from", "document", "three"])
model.train(100) # iterations
model.summary
```
</details>

### [wordnet 1.2.0](https://github.com/ged/ruby-wordnet)

<details>
<summary><i>example</i></summary>


This library is a Ruby interface to WordNet®. WordNet® is an online lexical reference system whose design is inspired by current psycholinguistic theories of human lexical memory. English nouns, verbs, adjectives and adverbs are organized into synonym sets, each representing one underlying lexical concept. Different relations link the synonym sets.

```ruby
#!/usr/bin/env ruby

#
#	Find all the hypernyms of all senses of a given noun and display them in a
#	heirarchy
#

$LOAD_PATH.unshift "lib"
require 'wordnet'

raise RuntimeError, "No word specified." if ARGV.empty?

# Create the lexicon
lex = WordNet::Lexicon.new

# Look up the synsets for the specified word
origins = lex.lookup_synsets( *ARGV )


# Iterate over the synsets for the different senses of the word
origins.each.with_index do |syn, i|
	hypernyms = []

	# Traverse the hypernyms
	syn.traverse( :hypernyms ).with_depth.each do |hyper_syn, depth|
		indent = '  ' * depth
		hypernyms << "%s%s" % [ indent, hyper_syn ]
	end

	puts "\nHypernym tree for #{syn} (sense #{i + 1}):", *hypernyms
	puts "Tree has #{hypernyms.length} synsets."
end
```
</details>

###

