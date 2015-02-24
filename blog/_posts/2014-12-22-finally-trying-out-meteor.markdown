---
layout: post
title:  Exploring Meteor
date:   2014-12-22 22:49:56
categories: technology, makersquare, san francisco, javascript
---
Now that Makersquare is on holiday break, I finally have had some time to breathe and check out some new things. I can't believe we've only been in the program for about 4 or 5 weeks now. The amount of work we have and the pace we keep makes it feel like we've all been there for months. Either way, it's been nice to slow down and relax a little bit the last few days.

Today I finally got around to trying <a href="http://www.meteor.com" target="blank">Meteor</a>, which is something I've wanted to try ever since I heard about it at the end of last year. Some people refer to Meteor as a Javascript framework, and in a sense that's true, but it's really much more than that. Meteor is a full-stack ecosystem. There is virtually zero separation from writing front-end or back-end code. It's all one cohesive piece. The easiest (although still oversimplied and disserving) comparison I could make is that it feels a bit like Rails for Javascript, but this is only in the sense that it is a complete suite of tools to build full web-apps, right out of the box.

Regardless, of what Meteor may or may not be, it is a lot of fun. I just went through the tutorial application build, and it only took me about 15min to not only get a basic to-do list application (including database) running, but also deployed to the web! I should also mention that this was only half the tutorial. Apparently in the second half, it will walk you through deploying your app to both Android and iOS in just minutes, without writing any extra code! That's going to be something for me to try tomorrow.

I think what is craziest about Meteor (to me, at least), is how it approaches the client/server relationship. In Meteor, when you trigger an event or action, the client reflects that immediately. It does not wait for a response from the server to reflect that change. In fact, it takes it further than that. Unless there is a specific file you're waiting on from the server, like an image, Meteor will actually act as if the database change has already been processed and keep allowing for blazingly fast and responsive user interaction. Meteor will only act on the database change in a visible way if it is returned an error during that process.

Meteor also makes is indcredibly easy for you to deploy your apps for testing. You can <a href="http://kennys_test_app.meteor.com" target="blank">view my test app here</a> to check it out. Play around with it. If you want. If you really want to see what I mean about how fast and immediately responsive Meteor is, try opening the app simultaneously in two different windows. You'll see that the tasks you add and delete are reflected immediately in both browsers!

Below is ALL OF THE CODE that it took to create that app (minus the CSS). Pretty crazy.

HTML
{% highlight html %}
<!-- simple-todos.html -->
<head>
  <title>Todo List</title>
</head>

<body>
  <div class="container">
    <header>
      <h1>Todo List</h1>

      <!-- add a form below the h1 -->
      <form class="new-task">
        <input type="text" name="text" placeholder = "Type to add new tasks" />
        </form>
    </header>

    <ul>
      {{#each tasks}}
        {{> task}}
      {{/each}}
    </ul>
  </div>
</body>

<template name="task">
  <li class="{{#if checked}}checked{{/if}}">
    <button class="delete">&times;</button>

    <input type="checkbox" checked="{{checked}}" class="toggle-checked" />

    <span class="text">{{text}}</span>
  </li>
</template>
{% endhighlight %}

Javascript
{% highlight javascript %}
// simple-todos.js
Tasks = new Mongo.Collection("tasks");

if (Meteor.isClient) {
  // This code only runs on the client
  Template.body.helpers({
    tasks: function(){
      return Tasks.find({}, {sort: {createdAt: -1}});
    }
  });
  Template.body.events({
    "submit .new-task": function(event){
      var text = event.target.text.value;
      Tasks.insert({
        text: text,
        createdAt: new Date()
      });
      event.target.text.value = "";
      return false;
    }
  });
  Template.task.events({
    "click .toggle-checked": function(){
      Tasks.update(this._id, {$set: {checked: ! this.checked}});
    },
    "click .delete": function(){
      Tasks.remove(this._id);
    }
  });
}
{% endhighlight %}


For a much better explanation of what Meteor is, and what it isn't, check out <a href="http://javascriptissexy.com/learn-meteor-js-properly/" target="blank">this blog post.</a> Richard continues to give some of the cleanest and level explanations/tutorials into Javascript related topics.

I will definitely be doing more work with Meteor in the near future, and after this brief experiment, I am considering building my large personal project in Meteor as well!
