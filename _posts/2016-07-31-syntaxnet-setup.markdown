---
layout: 	post
title: 		"SyntaxNet Setup"
date: 		2016-07-23 14:40:00 +0200
categories:	SyntaxNet, Tensorflow
---

This is the first in a series of posts related to a project I am working on that uses [SyntaxNet](https://github.com/tensorflow/models/tree/master/syntaxnet#getting-started), a model for [Tensorflow](https://www.tensorflow.org/), which is an Open Source library for Machine Learning. SyntaxNet is a tool derived from the field of [Natural Language Processing](https://en.wikipedia.org/wiki/Natural_language_processing) that comes ready for many different tasks. Although SyntaxNet is a very complex set of tools for Neural Networks, I will focus here on its parser, [Parsey McParceface](https://research.googleblog.com/2016/05/announcing-syntaxnet-worlds-most.html), which is said to possibly be the most accurate parser freely available today.

My goal for the series is to create a Python backend for a [bot](https://en.wikipedia.org/wiki/Internet_bot) that can be asked about beer-related information in order to analyze beer prices in a city, find craft breweries, festivals, and many other beer-related things. It is quite a big task, so I will slowly be writing more and more posts to follow my project and talk about things that I find. 

<br>

### Intro
________________

<br>

If you have not been through any of the [Tensorflow tutorials](https://www.tensorflow.org/versions/r0.10/tutorials/index.html), you definitely should. Although they require a little knowledge of Machine Learning, they explain everything that you need to actually build a network and take you through all the steps. They are pretty easy to setup and get running.

SyntaxNet is not for the feint of heart. It's definitely not building a network from scratch, but I have now spent a good couple hours battling to set it up and going through forum posts to find answers to why something is failing or how to actually use Parsey McParseface. Here, I will try and take you through the steps and provide you with answers and links to the posts where I found my answers. 

<br>

### Installation
________________

<br>

#### Virtualenv setup
I am setting up a [Virtual Environment](http://docs.python-guide.org/en/latest/dev/virtualenvs/) to manage my installations for this project. If you are kind of new to Python, you must learn this. It seems kind of tricky at first, but it makes managing Python a million times easier, I promise.

To install Virtualenv:

{% highlight bash %}

$ pip install virtualenv
$ cd my_project_folder
$ virtualenv venv

{% endhighlight %}

`virtualenv venv` is creating the virtual evironment and setting the name of the environment to `venv`. So if you don't like that name and want to call it `tensorflow` or `syntaxnet`, you should do that here. I will be using `venv`.

If you are managing your Python versions yourself (between 2.7 and 3.5), a way to handle that quickly is
{% highlight bash %}
$ virtualenv -p /usr/bin/python2.7
{% endhighlight %}
SyntaxNet requires Python 2.7! 

I have [Anaconda](https://www.continuum.io/downloads) for scientific computing. So, I had to change my default Python to 2.7. There are a couple different ways to do this, which can be read [in this Stackoverflow post](http://stackoverflow.com/questions/28436769/how-to-change-default-anaconda-python-environment).

Activate your virtual environment

{% highlight bash %}
$ source venv/bin/activate
{% endhighlight %}

<br>

#### Tensorflow
Now you have a clean environment and we must install everything that we need. Let's start with Tensorflow [virtualenv-installation](https://www.tensorflow.org/versions/r0.10/get_started/os_setup.html#virtualenv-installation). You might run into some hiccups, but you can battle the beast.

<br>

#### SyntaxNet
Now you are ready for the process of [SyntaxNet installation](https://github.com/tensorflow/models/tree/master/syntaxnet#installation). If you don't have [Homebrew](http://brew.sh/) installed, you need to install it, it will save you life. If you don't want to, there are other ways to install the packages you need as well.

You must set your JAVA_HOME with [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html). Use [this post](http://stackoverflow.com/questions/18144660/what-is-path-of-jdk-on-mac) if you have troubles setting the path to 1.8. 

Now you can download [Bazel](http://www.bazel.io/docs/install.html#mac-os-x) with Homebrew:
{% highlight bash %}
$ brew install bazel
{% endhighlight %}

Do the same for [Swig](http://www.swig.org/):
{% highlight bash %}
$ brew install swig
{% endhighlight %}

You should already have the protocal buffers if you installed Tensorflow, but follow the next step in the git page if you need to. 

Install asciitree for displaying the demo:
{% highlight bash %}
$ pip install asciitree
{% endhighlight %}

You should also have numpy, but you could run another pip command just in case:
{% highlight bash %}
$ pip install numpy
{% endhighlight %}

Now comes the beast. Once you have everything from above you have to build SyntaxNet
{% highlight bash %}
$ git clone --recursive https://github.com/tensorflow/models.git
$ cd models/syntaxnet/tensorflow
$ ./configure
$ cd ..
$ bazel test --linkopt=-headerpad_max_install_names \
    syntaxnet/... util/utf8/...
{% endhighlight %}







