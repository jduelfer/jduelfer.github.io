---
layout: 	post
title: 		"SyntaxNet: Understanding the Parser"
date: 		2016-08-20 11:00:00 +0200
categories:	SyntaxNet, Tensorflow
---

In this post, I will try to dive into why we need a parser and how to use Parsey McParseface. 

<br>

### Parsey McParseface
____________________________________

<br>

You can run the `demo.sh` file that comes packed with SyntaxNet immediately after building it. This is the famous Parsey McParseface, which is basically a Natural Language parser trained while you building SyntaxNet. You can pass any sentence into it and it will give you an ASCII tree representing the syntactic structure of the sentece. To do this, in terminal `cd` into `models/syntaxnet` and then execute the following command:

{% highlight bash %}
echo 'Bob brought the pizza to Alice.' | syntaxnet/demo.sh
{% endhighlight %}

where `'Bob brought the pizza to Alice.'` could be any sentence that you want, and you get an output like:

{% highlight bash %}
Input: Bob brought the pizza to Alice .
Parse:
brought VBD ROOT
 +-- Bob NNP nsubj
 +-- pizza NN dobj
 |   +-- the DT det
 +-- to IN prep
 |   +-- Alice NNP pobj
 +-- . . punct
{% endhighlight %}

You get an incredibly accurate parsing of a sentence in seconds. But having to execute from your command line everytime you want to parse a sentence isn't very practical, so let's put it into a Python script.

### Python Script
____________________________________

<br>

Assuming that you followed my last tutorial, you should have a directory structure like the following:
{% highlight bash %}
- your_directory
	- models
	- venv
{% endhighlight %}

`cd` back to `your_directory` and create a new python file with `touch app.py`. Of course, printing out an ASCII tree that represents the syntactic structure isn't very practical for developing, so we want to get the parsed sentences into memory where we can do more analysis. In order to parse the tree, we are going to import [NLTK](http://www.nltk.org/), the Natural Language Toolkit for python that allows us to do much more processing with natural language than SyntaxNet alone. You need to install nltk with `pip install nltk`. For now, we are going to use its [Conll Corpus Reader](http://www.nltk.org/_modules/nltk/corpus/reader/conll.html) to parse the ASCII tree into a reeusable format. 

We also want to create our own `demo.sh` file to call the parser. To do this, `cd` into `models/syntaxnet/syntaxnet` and `touch your_bash_file.sh`. If you are on OSX, the file probably won't have permissions yet, so you need to run `chmod +x your_bash_file.sh`. In this file, you can copy and paste the same code from `demo.sh`, or change any of the parameters that you want. Later, I will dive into this, but for now we are just going to use a copy of `demo.sh`:

{% highlight bash %}
PARSER_EVAL=bazel-bin/syntaxnet/parser_eval
MODEL_DIR=syntaxnet/models/parsey_mcparseface
[[ "$1" == "--conll" ]] && INPUT_FORMAT=stdin-conll || INPUT_FORMAT=stdin

$PARSER_EVAL \
  --input=$INPUT_FORMAT \
  --output=stdout-conll \
  --hidden_layer_sizes=64 \
  --arg_prefix=brain_tagger \
  --graph_builder=structured \
  --task_context=$MODEL_DIR/context.pbtxt \
  --model_path=$MODEL_DIR/tagger-params \
  --slim_model \
  --batch_size=1024 \
  --alsologtostderr \
   | \
  $PARSER_EVAL \
  --input=stdin-conll \
  --output=stdout-conll \
  --hidden_layer_sizes=512,512 \
  --arg_prefix=brain_parser \
  --graph_builder=structured \
  --task_context=$MODEL_DIR/context.pbtxt \
  --model_path=$MODEL_DIR/parser-params \
  --slim_model \
  --batch_size=1024 \
  --alsologtostderr \
{% endhighlight %}

Now that we have our own file to call the parser, we need to wrap this in a Python script. 

{% highlight python %}
# natural language toolkit
import nltk
import subproces
import os

os.chdir('models/syntaxnet')

# call a subprocess and store the printed tree in parsed_ascii_tree.txt
subprocess.calssomsl([
	"echo 'Bob brought the pizza to Alice.' | syntaxnet/your_bash_file.sh 1>parsed_ascii_tree.txt"
], shell=True)

# use nltk to parse the ascii tree into an array
## the third parameter is what column types we want to collect from the conll tree. This is all of them.
corpus_reader = nltk.corpus.reader.conll.ConllCorpusReader('', 'parsed_ascii_tree.txt', ['words', 'pos', 'tree', 'chunk', 'ne', 'srl'])

# convert reader into a multidimensional array
corpus_grid = corpus_reader._grids()
print(corpus_grid)
{% endhighlight %}

And there you go, you have wrapped Parsey McParseface in a reusable Python script ready for an application. 

<br> 

### Usefulness (Warning: no code and mostly Linguistics talk)
________________________________

So what can a Parser do for us?


















