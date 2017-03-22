---
layout: 	post
title: 		"Cloud: Full Stack Thought Problem"
date: 		2017-03-22 12:00:00 +0200
categories:	Cloud, MEAN, LAMP
---

For a thought problem on Full Stack Development, some employees of a company have created a football team so they can play together after work or on weekends. The company wants us to develop a mobile tool to help organize the team's training sessions and matches. We are expected to develop the following:
- **Cloud Platform**
- **Web Based App**

We are provided with:
- **A Linux Server** with a Public IP address

Apart from the Linux server, We are given free rein over which technologies to use to develop the requirements. As good Cloud Engineers, we know that every problem may have multiple solutions, and the choice of technologies, frameworks, and languages is based  upon a range of factors:
> 1. Which languages/frameworks do I, or our team, know?
> 2. Which part of the development is most crucial to delivering the requirements?
> 3. What is the level of scalability needed for the future of the application?
> 4. How long do we have to complete our requirements?

These questions will help us decide on which languages or frameworks to use in the development of our application. But, before doing this, it is necessary to first declare the the entity-relationship model of our data.

<br/>

### Entity-Relationship Model
_______________________
From our requirements, we have obtained the following structure for our data:

<iframe frameborder="0" style="width:100%;height:213px;" src="https://www.draw.io/?lightbox=1&highlight=0000ff&edit=_blank&layers=1&nav=1&title=InformationStructure.html#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D0Bwm50wVp-OdWSkx3cXl2RXFyRzQ%26export%3Ddownload"></iframe>

As seen in the diagram, it is a fairly straightforward entity-relationship, with an *Agenda* object as the parent of a *Lineup* object. An *Agenda* may have two types, a match or a training session. In the case of a match-type, the *Agenda* will have a child *Lineup* with match type. However, in the case of a training session type, an *Agenda* will have a child *Lineup* with training session type. A *Lineup* is then comprised of various *Player* objects, representing the employees in our company.

A relational database would encapsulate this logic very well. Popular open source databases for Linux that we could choose from are [PostgreSQL](https://www.postgresql.org) or [MySQL](https://www.mysql.com).

In recent years, we have also seen a growth of NoSQL databases, such as MongoDB, which focus on providing JSON type database structures. They are focused on providing easy communication with the database and data, eliminating the need for ORMs in traditional relational database models.

However, before deciding on the database, it is necessary to analyze what languages and frameworks we will be using to develop our client-server model.

#### Client-Server Model
________________________
As we have a Linux server, we are going to serve an API and a mobile-first web based app. We can conceive of two reasonable architectures:
- We server an API off of one port with an appropriate language to communicate with the server. We then serve our client off of another port, allowing them to have independent architectures.
- We serve the API and the client off of the same port, using the same architecture.

Using a distinct architecture to serve the API and the client would mean that we could have two entirely independent applications. We could have the API served with language/framework x and the client served with language/framework y. Although this means that we are using various technologies for the same product, it would allow us to choose the most efficient languages/frameworks for each part of the application. We could then choose a language focused on manipulating data or creating highly efficient APIs for the backend, while using an entirely frontend oriented language for our client.

On the other hand, we could serve the API and the client with the same language/framework x. This would mean that the entire application is written in the same language and under the same architecture. It would allow us to develop the system quickly and efficiently, focusing on one framework for our development.

#### LAMP or MEAN
________________________
Confronted with these two options, we come across the debate between the classic web server LAMP (Linux Apache MySQL PHP/Pearl/Python) stack, and the MEAN (MongoDB Express Angular Node) application stack. The LAMP stack offers flexibility, customization, and cost efectiveness, all import for developing a high-availability heavy-duty dynamic web app. Components can be swapped in and out depending on the needs of specific parts of the application, making sure that we optimize every aspect of our application to support enterprise-level applications. So what would our requirements look like with the traditional LAMP stack?

LAMP can support both the Client-Server models that we have proposed. To take the first of the two (distinct architectures for the API and the client), we could use a language like Python or C++ to develop a highly optimized API. We could serve this with a traditional Apache server, or something more lightweight, like [Gunicorn](http://gunicorn.org/). On a separate port, we could develop our client using PHP to communicate with the server and any number of frontend frameworks, like Ember, Backbone, React, or Knockout.

There are a few reasons that I would not go with this division of architecture:
> - I am developing a solution with very few requirements, I should not overcomplicate with too many languages and frameworks.
> - The solution should be easily maintainable, I should not need experts in an array of different technologies.
> - This solution is not focused on a web-based app.
> - This solution would take a bit of time to setup and develop with all its interconnected parts.
> - I do not know PHP, maybe I should use languages I am more familiar with instead of learning something brand new for a relatively small requirement.

However, the LAMP stack is still not ruled out. What would the second option look like that serves the API and client off of the same port and under the same architecture? We must choose a language that supports a nice web driven framework to serve both our API and the static files for our client. Choosing the language is then dependent upon the factors mentioned above. Personally, I would choose Python, as it is a full-fledged programming language with a large community. It has been around for a long time and has numerous success stories, like [Google](http://google.com), [Youtube](http://youtube.com) and [Quora](http://quora.com), which are just a few that are written mainly in Python, confirming that it is a widely used, scalable language.

Using Python, we would choose a library or framework to develop a web app. Two widely used frameworks are [Flask](http://flask.pocoo.org) and [Django](https://www.djangoproject.com). Flask is more lightweight and easier to manage for small projects, so I would go with it. We then need [Gunicorn](http://gunicorn.org) and [Nginx](https://nginx.org/en/) for a production ready Server behind a Proxy. A strong relational database with easy integration with Python would be [PostgreSQL](https://www.postgresql.org), which also has many ORMs available if we think it necessary.

To serve the API, we could then use [Flask-RESTFUL](https://flask-restful.readthedocs.io/en/0.3.5/). For just how minimalist this framework can be, here is an example:
{% highlight Python %}
from flask import Flask
from flask_restful import Resource, Api

app = Flask(__name__)
api = Api(app)

class HelloWorld(Resource):
    def get(self):
        return {'hello': 'world'}

api.add_resource(HelloWorld, '/')

if __name__ == '__main__':
    app.run(debug=True)
{% endhighlight %}

We could then easily add "Resources" for any data required by the client.

Flask also comes with [Jinja2](http://jinja.pocoo.org/docs/2.9/) a very straightforward templating engine to serve static files with dynamic content with Python as the "Controller." We could use this templating engine, which a simple and unobstrusive Javascript framework like JQuery for minimal DOM manipulation. A mobil-first CSS library, like Bootstrap3 would allow the web app to be displayed nicely on any mobile phone.

With a LAMP stack, our choice of technologies would be the following:
- Linux OS
- Gunicorn Server with Nginx
- PostgreSQL
- Python

With this stack, we then have the option to build an API and a client serparately, a Python API using (Flask-RESTful)[https://flask-restful.readthedocs.io/en/0.3.5/] to serve the API and a decoupled application to consume the API. Or, we could have the application access the database and serve static HTML pages using Flask's Jinja2 templating. Using a mobile-first CSS framework, like Bootstrap, we would have our application displayable on mobile phones.

But is this really a mobile-first application? Is this traditional LAMP stack focused on the Client, optimizing resources for an enhanced mobile experience?

#### MEAN
_______________

The answer is no, the traditional LAMP stack is focused more on the enterprise-level customizable app. Of course, I believe that a highly skilled team could develop an incredible mobile-first web based app with enough time and energy. However, it would be quite complex and require experts in various levels of the stack.

Instead, we could develop our API and web based app all under the [MEAN Stack](http://mean.io/). With efficient CLIs for project initiation, the startup time to get a base application running smoothly is very minimal. Even better, it using Node.js and Express (for example) for communication and AngularJS for the client, meaning that all of the code base is under one language: Javascript. Even the database, [MongoDB](https://www.mongodb.com/), is a NoSQL database based on "JSON" documents, meaning that there is no need for a traditional ORM. We could then use Angular2, a framework developed entirely mobile-first, to develop the client.

Here is why I would choose a MEAN stack given the requirements:
> - NPM is a great package manager and allows me to set up my architecture quickly and efficiently.
> - Angular2 is mobile-first from the ground up, focusing on optimizing the web for mobile.
> - The Angular2 CLI helps you generate a working app following best practices in no time.
> - Node uses a great Javascript engine, event-based, long-polling, and very good at doing several things at the same time.
> - Everything under the same hood allows me to build an easily maintainable app very quickly, allowing my coworkers to have fun on the pitch as soon as possible!

As this is a thought problem on Full Stack Development, and I have written quite a bit about the various options available, here is a very nice tutorial on building an Angular2 web based app on the MEAN stack from a great community of Javascript developers: [https://scotch.io/tutorials/mean-app-with-angular-2-and-the-angular-cli](https://scotch.io/tutorials/mean-app-with-angular-2-and-the-angular-cli).