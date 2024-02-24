# nlp stack

```bash

+------------------+      +--------+      +-----+
|---data science---+----->|  ruby  +----->| nlp |
+------------------+      +--------+      +-----+

```

---

# Knowledge Management:
Chroma: This embedding database suggests the intention to store and retrieve information in a way optimized for use by LLMs.

# LLM Integrations:
Aoororachain: This library specifically for integrating LLMs with Ruby projects aligns seamlessly with the overall theme.
Cohere and ollama-ai: Including powerful LLMs themselves reinforces this focus.

# Content Transformation:
beckett: The ability to render Markdown into formats like JSON and HTML5 suggests preparing content to be more easily processed or understood by LLMs.

# Data Handling:
stream_lines: The capacity to stream files line by line could be useful for efficient processing of large datasets to train or query the LLMs.

### [socrates](https://rubygems.org/gems/socrates/versions/0.1.26?locale=en)

Socrates is a micro-framework for building stateful conversational interfaces. It provides straight-forward state management, a clear pattern for modeling the states and conversational flow (transitions), and some helpers.

<details>
<summary><i>example</i></summary>
```ruby
class GetStarted
  include Socrates::Core::State

  def listen(message)
    case message.downcase
      when "help"
        transition_to :help
      when "age"
        transition_to :ask_for_name
      else
        transition_to :no_comprende
    end
  end
end

class Help
  include Socrates::Core::State

  def ask
    respond message: <<~MSG
      Thanks for asking! I can do these things for you...

        • `age` - Calculate your age from your birth date.
        • `help` - Tell you what I can do for you.

      So, what shall it be?
    MSG
    transition_to :get_started, action: :listen
  end
end

class NoComprende
  include Socrates::Core::State

  def ask
    respond message: "Whoops, I don't know what you mean by that. Try `help` to see my commands."
    transition_to :get_started
  end
end

class AskForName
  include Socrates::Core::State

  def ask
    respond message: "First things first, what's your name?"
  end

  def listen(message)
    # Transition to the next step while persisting the name for future retrieval.
    transition_to :ask_for_birth_date, data: { name: message }
  end
end

class AskForBirthDate
  include Socrates::Core::State

  def ask
    respond message: "Hi #{first_name}! What's your birth date (e.g. MM/DD/YYYY)?"
  end

  def listen(message)
    begin
      birth_date = Date.strptime(message, "%m/%d/%Y")
    rescue ArgumentError
      respond message: "Whoops, I didn't understand that. What's your birth date (e.g. MM/DD/YYYY)?"
      repeat_action
      return
    end
    # Transition to the next step while persisting the birth date for future retrieval.
    transition_to :calculate_age, data: { birth_date: birth_date }
  end

  private

  def first_name
    @data.get(:name).split.first
  end
end

class CalculateAge
  include Socrates::Core::State

  def ask
    respond message: "Got it #{first_name}! So that makes you #{calculate_age} years old."
    end_conversation
  end

  private

  def first_name
    @data.get(:name).split.first
  end

  def birth_date
    @data.get(:birth_date)
  end

  def calculate_age
    ((Date.today.to_time - birth_date.to_time) / 1.year).floor
  end
end
```
</details>


### [sad_panda](https://github.com/mattThousand/sad_panda)

sad_panda is a gem featuring tools for sentiment analysis of natural language: positivity/negativity and emotion classification.

Emotion Range: "anger", "disgust", "joy", "surprise", "fear", "sadness"

Polarity Range: 0 to 10

<details>
<summary><i>example</i></summary>

```ruby
require 'sad_panda'

SadPanda.emotion('my lobster collection makes me happy!')
=> :joy

SadPanda.polarity('I love cactuses!')
=> 10.0

sad_panda = SadPanda::Emotion.new('my lobster collection makes me happy!')
sad_panda.call
sad_pands.scores = {:anger=>0, :disgust=>0, :joy=>1, :surprise=>0, :fear=>0, :sadness=>0, :ambiguous=>0}

sad_panda.joy => 1
sad_panda.fear => 0
# same for all the other emotions
```
</details>

### [aoororachain](https://github.com/mariochavez/aoororachain)

Aoororachain is Ruby chain tool to work with LLMs.

<details>
<summary><i>example</i></summary>

```ruby
require "aoororachain"

# Setup logger.
Aoororachain.logger = Logger.new($stdout)
Aoororachain.log_level = Aoororachain::LEVEL_DEBUG

chroma_host = "http://localhost:8000"
collection_name = "ruby-documentation"

# You can define a custom Parser to clean data and maybe extract metadata.
# Here is the code of RubyDocParser that does exactly that.
class RubyDocParser
  def self.parse(text)
    name_match = text.match(/Name (\w+)/)
    constant_match = text.match(/Constant (\w+)/)

    object_match = text.match(/Object (\w+)/)
    method_match = text.match(/Method ([\w\[\]\+\=\-\*\%\/]+)/)

    metadata = {}
    metadata[:name] = name_match[1] if name_match
    metadata[:constant] = constant_match[1] if constant_match
    metadata[:object] = object_match[1] if object_match
    metadata[:method] = method_match[1] if method_match
    metadata[:lang] = :ruby
    metadata[:version] = "3.2"

    text.gsub!(/\s+/, " ").strip!
    [text, metadata]
  end
end

# A DirectoryLoader points to a path and sets the glob for the files you want to load. 
# A loader is also specified. FileLoader just opens and reads the file content. 
# The RubyDocParser is set as well. This is optional in case you data is very nice and needs no pre-processing.
directory_loader = Aoororachain::Loaders::DirectoryLoader.new(path: "./ruby-docs", glob: "**/*.txt", loader: Aoororachain::Loaders::FileLoader, parser: RubyDocParser)
files = directory_loader.load

# With your data clean and ready, now it is time to chunk it. The chunk size depends of the context size of the LLMs that you want to use.
# 512 is a good number to start, don't go lower than that. An overlap can also be specified.
text_splitter = Aoororachain::RecursiveTextSplitter.new(size: 512, overlap: 0)

texts = []
files.each do |file|
  texts.concat(text_splitter.split_documents(file))
end

# The final step is to create and store the embeddings.
# First, select an embedding model
model = Aoororachain::Embeddings::LocalPythonEmbedding::MODEL_INSTRUCTOR_L
# Create an instance of the embedder. device is optional. Possible options are:
# - cuda. If you have an external GPU
# - mps. If you have an Apple Sillicon chip (M1 to M2).
# - cpu or empty. It will use the CPU by default.
embedder = Aoororachain::Embeddings::LocalPythonEmbedding.new(model:, device: "mps")
# Configure your Vector database.
vector_database = Aoororachain::VectorStores::Chroma.new(embedder: embedder, options: {host: chroma_host})

# Embbed your files. This can take a few minutes up to hours, depending on the size of your documents and the model used.
vector_database.from_documents(texts, index: collection_name)
```
</details>

### [networkx 0.4.0](https://rubygems.org/gems/networkx/versions/0.4.0?locale=en)

A Ruby implemenation of the well-known graph library called "networkx".

<details>
<summary><i>example</i></summary>
```ruby
require 'networkx'

g = NetworkX::Graph.new
g.add_edge('start', 'stop')
```
</details>


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


### [stream_lines 0.4.1](https://rubygems.org/gems/stream_lines)

An API for streaming files from remote locations one line at a time.

<details>
<summary><i>example</i></summary>
```ruby
url = 'https://my.remote.file/file.jsonl'
stream = StreamLines::Reading::JSONLines.new(url)

stream.each do |row|
  # each row will be an Hash
end

# Supports all Ruby JSON::parse options
stream = StreamLines::Reading::JSONLines.new(url, symbolize_names: true)

stream.each do |row|
  # each row will be a Hash
end
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

A Ruby port of Perl Lingua::EN::Tagger, a probability based, corpus-trained tagger that assigns POS tags to English text based on a lookup dictionary and a set of probability values.

<details>
<summary><i>example</i></summary>

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

Launches, schedules and manages tasks represented in a DAG, as multiple processes

<details>
<summary><i>example</i></summary>

![sample ml pipeline](https://miro.medium.com/v2/resize:fit:640/format:webp/1*Jz2UVqrVYj1hIwHxOpw2VQ.png)

![example DAG diagram](https://miro.medium.com/v2/resize:fit:4800/format:webp/1*3eEtimgIPEAuizSWtu6HyQ.png)

![another example DAG](https://dagshub.com/blog/content/images/2021/07/Tred-G.svg)

```ruby
# https://gitlab.com/RedFred7/Jongleur#usage
```
</details>

### [composable_operations 0.10.1](http://github.com/t6d/composable_operations)

Composable Operations is a tool set for creating operations and assembling multiple of these operations in operation pipelines.

<details>
<summary><i>example</i></summary>

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

Ferret is a super fast, highly configurable search library.

<details>
<summary><i>example</i></summary>

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

Fast state-of-the-art tokenizers for Ruby

<details>
<summary><i>example</i></summary>

```ruby
tokenizer = Tokenizers.from_pretrained("bert-base-cased")

# https://github.com/ankane/tokenizers-ruby#training
```
</details>


### [eqn 1.6.5](https://github.com/schneidmaster/eqn)

A gem to evaluate mathematical equations. Includes support for variables and functions.

<details>
<summary><i>example</i></summary>
```ruby
Eqn::Parser.parse('1 + 1')
# => <syntax tree is printed>
```
</details>

### [google_search_results 0.0.1](https://github.com/serpapi/google-search-results-ruby)

Get Google Search Results via SERP API. Hash, JSON, and HTML outputs supported.

<details>
<summary><i>example</i></summary>
```ruby
# https://github.com/serpapi/google-search-results-ruby#supported-search-engine
```
</details>

### [lemmatizer 0.2.2](http://github.com/yohasebe/lemmatizer)

Lemmatizer for text in English. Inspired by Python's nltk.corpus.reader.wordnet.morphy package.

<details>
<summary><i>example</i></summary>

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

LLM Memory is a Ruby gem designed to provide large language models (LLMs) like ChatGPT with memory using in-context learning. This enables better integration with systems such as Rails and web services while providing a more user-friendly and abstract interface based on brain terms.

<details>
<summary><i>example</i></summary>

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

Ohm is a library that allows to store an object in Redis, a persistent key-value database. It has very good performance.

<details>
<summary><i>example</i></summary>
```ruby
```
</details>

### [ohm-contrib 3.0.0](http://github.com/cyx/ohm-contrib)

Includes a couple of core functions such as callbacks, timestamping, typecasting and lots of generic validation routines.

<details>
<summary><i>example</i></summary>
```ruby
```
</details>

### [hashie](https://github.com/hashie/hashie)

Hashie is a growing collection of tools that extend Hashes and make them more useful.

<details>
<summary><i>example</i></summary>
IgnoreUndeclared

This extension can be mixed in to silently ignore undeclared properties on initialization instead of raising an error. This is useful when using a Trash to capture a subset of a larger hash

```ruby
class Person < Trash
  include Hashie::Extensions::IgnoreUndeclared
  property :first_name
  property :last_name
end

user_data = {
  first_name: 'Freddy',
  last_name: 'Nostrils',
  email: 'freddy@example.com'
}

p = Person.new(user_data) # 'email' is silently ignored

p.first_name # => 'Freddy'
p.last_name  # => 'Nostrils'
p.email      # => NoMethodError
```
</details>

### [docsplit](https://documentcloud.github.io/docsplit/)

A command-line utility and Ruby library for splitting apart documents into their component parts: searchable UTF-8 plain text via OCR if necessary, page images or thumbnails in any format, PDFs, single pages, and document metadata (title, author, number of pages...)

<details>
<summary><i>example</i></summary>
```ruby
Docsplit.extract_pages('doc.pdf', :pages => 1..10)
Docsplit.extract_text(docs, :ocr => false, :output => 'storage/text')

```
</details>

### [pdf_paradise 0.2.3](https://rubygems.org/gems/pdf_paradise)

This gem is called pdf_paradise. It attempts to be a swiss army knife, a wrapper, over pdf-related functionality in general, by tapping into external projects, such as prawn, qpdf and so forth. For more information, please have a look at the documentation of the project on the homepage of this gem, at: https://www.rubydoc.info/gems/pdf_paradise/

<details>
<summary><i>example</i></summary>
```ruby
```
</details>

### [beckett 0.1.0](https://github.com/coreyti/beckett)

A Markdown renderer, using Kramdown, to convert to JSON, HTML5 or a Ruby Hash.

<details>
<summary><i>example</i></summary>
```ruby

markdown_text = <<-'TEXT'
# Chain-of-Thought Reasoning Without Prompting
#llm #rag #retrieval

https://arxiv.org/pdf/2402.10200.pdf

## secondary heading

### a third kind of heading

#### a fourth heading


##### a fifth heading
TEXT

=> "# Chain-of-Thought Reasoning Without Prompting\n#llm #rag #retrieval \n\nhttps://arxiv.org/pdf/2402.10200.pdf\n\n## secondary heading\n\n### a third kind of heading\n\n#### a fourth heading\n\n\n##### a fifth heading\n\n###### a sixth heading\n\n"
[4] pry(main)> Beckett::Document.new(markdown_text)
=> #<Beckett::Document:0x000077799b0bf680
 @content=
  "# Chain-of-Thought Reasoning Without Prompting\n#llm #rag #retrieval \n\nhttps://arxiv.org/pdf/2402.10200.pdf\n\n## secondary heading\n\n### a third kind of heading\n\n#### a fourth heading\n\n\n##### a fifth heading\n\n###### a sixth heading\n\n">
[5] pry(main)> x = Beckett::Document.new(markdown_text)
=> #<Beckett::Document:0x000077799b097478
 @content=
  "# Chain-of-Thought Reasoning Without Prompting\n#llm #rag #retrieval \n\nhttps://arxiv.org/pdf/2402.10200.pdf\n\n## secondary heading\n\n### a third kind of heading\n\n#### a fourth heading\n\n\n##### a fifth heading\n\n###### a sixth heading\n\n">
[6] pry(main)> x.to_hash
=> {:root=>
  {:children=>
    [{:node_name=>"ARTICLE",
=> {:root=>
  {:children=>
    [{:node_name=>"ARTICLE",
      :node_type=>1,
      :attributes=>{"id"=>"chain-of-thought-reasoning-without-prompting"},
      :children=>
       [{:node_name=>"HEADER", :node_type=>1, :attributes=>{"id"=>"chain-of-thought-reasoning-without-prompting"}, :children=>[{:node_name=>"#text", :node_type=>3, :node_text=>"Chain-of-Thought Reasoning Without Prompting"}]},
        {:node_name=>:P, :node_type=>1, :children=>[{:node_name=>"#text", :node_type=>3, :node_text=>"#llm #rag #retrieval"}]},
        {:node_name=>:P, :node_type=>1, :children=>[{:node_name=>"#text", :node_type=>3, :node_text=>"https://arxiv.org/pdf/2402.10200.pdf"}]},
        {:node_name=>"SECTION",
         :node_type=>1,
         :attributes=>{"id"=>"secondary-heading"},
         :children=>
          [{:node_name=>"HEADER", :node_type=>1, :attributes=>{"id"=>"secondary-heading"}, :children=>[{:node_name=>"#text", :node_type=>3, :node_text=>"secondary heading"}]},
           {:node_name=>"SECTION",
            :node_type=>1,
            :attributes=>{"id"=>"a-third-kind-of-heading"},
            :children=>
             [{:node_name=>"HEADER", :node_type=>1, :attributes=>{"id"=>"a-third-kind-of-heading"}, :children=>[{:node_name=>"#text", :node_type=>3, :node_text=>"a third kind of heading"}]},
              {:node_name=>"SECTION",
               :node_type=>1,
               :attributes=>{"id"=>"a-fourth-heading"},
               :children=>
                [{:node_name=>"HEADER", :node_type=>1, :attributes=>{"id"=>"a-fourth-heading"}, :children=>[{:node_name=>"#text", :node_type=>3, :node_text=>"a fourth heading"}]},
                 {:node_name=>"SECTION",
                  :node_type=>1,
                  :attributes=>{"id"=>"a-fifth-heading"},
                  :children=>
                   [{:node_name=>"HEADER", :node_type=>1, :attributes=>{"id"=>"a-fifth-heading"}, :children=>[{:node_name=>"#text", :node_type=>3, :node_text=>"a fifth heading"}]},
                    {:node_name=>"SECTION",
```

</details>

### [lingua](https://github.com/dbalatero/lingua)

Provides sentence splitting, syllable, and text-quality algorithms.

<details>
<summary><i>example</i></summary>
```ruby

x = Lingua::EN::Syllable.syllables("So the query is classifying, given the set of topics available. So the document is classified as audio processing. All of the chunks in the post-quist database table related to audio processing. Yeah, something like that.")

[16] pry(main)> x
=> 65
```
</details>

### [pragmatic_segmenter](https://github.com/diasks2/pragmatic_segmenter)

Pragmatic Segmenter is a sentence segmentation tool for Ruby. It allows you to split a text into an array of sentences. This gem provides 2 main benefits over other segmentation gems - 1) It works well even with ill-formatted text 2) It works for multiple languages

<details>
<summary><i>example</i></summary>
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

A multilingual tokenizer to split a string into tokens.

<details>
<summary><i>example</i></summary>
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

ruby-spacy is a wrapper module for using spaCy from the Ruby programming language via PyCall. This module aims to make it easy and natural for Ruby programmers to use spaCy. This module covers the areas of spaCy functionality for using many varieties of its language models, not for building ones.

<details>
<summary><i>example</i></summary>

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

### [epitome](https://github.com/McFreely/epitome)

A small gem to make your text shorter. It's an implementation of the Lexrank algorithm. You can use it on a single text, but lexrank is designed to be used on a collection of texts. But it works the same anyway.

<details>
<summary><i>example</i></summary>
```ruby
document_one = Epitome::Document.new("The cat likes catnip. He rolls and rolls")
document_two = Epitome::Document.new("The cat plays in front of the dog. The dog is placid.")

document_collection = [document_one, document_two]
@corpus = Epitome::Corpus.new(document_collection)

@corpus.summary(length=3)
@corpus.summary(5, 0.2)
```
</details>

### [tomoto 0.3.3](https://github.com/ankane/tomoto-ruby)

High performance topic modeling for Ruby

<details>
<summary><i>example</i></summary>
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

This library is a Ruby interface to WordNet®. WordNet® is an online lexical reference system whose design is inspired by current psycholinguistic theories of human lexical memory. English nouns, verbs, adjectives and adverbs are organized into synonym sets, each representing one underlying lexical concept. Different relations link the synonym sets.

<details>
<summary><i>example</i></summary>
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

### [chroma-db](https://github.com/mariochavez/chroma)

Chroma is the open-source embedding database. Chroma makes it easy to build LLM apps by making knowledge, facts, and skills pluggable for LLMs.

<details>
<summary><i>example</i></summary>
```ruby
require "logger"

# Requiere Chroma Ruby client.
require "chroma-db"

# Configure Chroma's host. Here you can specify your own host.
Chroma.connect_host = "http://localhost:8000"
Chroma.logger = Logger.new($stdout)
Chroma.log_level = Chroma::LEVEL_ERROR

# Check current Chrome server version
version = Chroma::Resources::Database.version
puts version

# Create a new collection
collection = Chroma::Resources::Collection.create(collection_name, {lang: "ruby", gem: "chroma-db"})

# Add embeddings
embeddings = [
  Chroma::Resources::Embedding.new(id: "1", embedding: [1.3, 2.6, 3.1], metadata: {client: "chroma-rb"}, document: "ruby"),
  Chroma::Resources::Embedding.new(id: "2", embedding: [3.7, 2.8, 0.9], metadata: {client: "chroma-rb"}, document: "rails")
]
collection.add(embeddings)
```
</details>
