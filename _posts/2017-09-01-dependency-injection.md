---
layout: post
title: Newbie to Newbie - Dependency Injection
---

This is the first post in a series called Newbie to Newbie where I try to highlight new topics I have recently learned or struggled with and explain them to other newbies like me. My goal is to highlight these topics in an approachable way in as simple terms as possible. So if you're an experienced coder, sorry you are not my target audience. This is not intended to be an all encompassing delve into the nitty gritty details.  But rather an intro for newbies like me who just want to know what the heck people are talking about at a high level.

So let’s take a look at Dependency Injection. Don’t worry it’s not as painful as it sounds.
{:.center}
![]({{ site.baseurl }}/assets/blog-posts/injection.png)

I recently started learning the AngularJS architecture and as I was reading through documentation the term dependency injection kept coming up. (Now I know why. It’s kind of an important topic). But Angular was all new to me, and there was a lot of new terminologies to learn and wrap my head around. So what did I do? Like any good developer, I Googled it, of course. I like to first go to the official documentation, so I took a look at what the developer guide for AngularJS had to say:

> "Dependency Injection (DI) is a software design pattern that deals with how components get hold of their dependencies. The AngularJS injector subsystem is in charge of creating components, resolving their dependencies, and providing them to other components as requested.”

#### Huh?

Reading it now, that makes sense but at the time I had no construct to relate this too. None the less this definition was not helping me understand the concept. So I continued my research . . .

Wikipedia’s explanation was a little better. But there is still a lot of technical jargon going on here - services, client, state ... and they used the term dependency and injection within the definition. But we are starting to get a picture of what’s going on. Albeit a fuzzy picture.

> “In software engineering, dependency injection is a technique whereby one object supplies the dependencies of another object. A dependency is an object that can be used (a service). An injection is the passing of a dependency to a dependent object (a client) that would use it. The service is made part of the client's state. Passing the service to the client, rather than allowing a client to build or find the service, is the fundamental requirement of the pattern.”

So we are giving something, the dependency, to some other something (the client), so the client can use it. Ok. But what exactly is a dependency specifically? How do I use this in Angular?

So I continued my research . . . and I came across this video from Anthony Alicea that I found very helpful.

<div style="width:100%;height:0;padding-bottom:56%;position:relative;">
  <iframe width="100%" height="100%" style="position:absolute"  src="https://www.youtube.com/embed/ejBkOjEG6F0?start=2040" frameborder="1" allowfullscreen></iframe>
</div>


The discussion on dependency injection begins around minute 34. Then it all clicked at 35:50. The discussion continues through the end of the video. Go ahead and watch. (Aside - this is a great intro video to the Angular architecture. If you have the time I suggest watching the entire video.)

Wait, wait, wait. Hold the phone. You mean dependency injection is just taking an object, treating it as a variable and passing into another function, as an argument, for that function to use. Omg, that’s amazing! (I think I actually said that when I watch the video the first time). This is not something new. Passing a function or an object into another function - I had been doing dependency injection and not even known it.

That’s it! Well damn AngularJS, why didn’t you just say so? I know what you are thinking. Isn’t that just what the two previous definitions were saying. Well, yes … but it seems so complicated and he made it sound so easy.

Let’s take a look at an example from a recent project of mine to see how it relates to Angular.
{% highlight javascript %}
function HomeCtrl(Room, Message) {
  code ...
  code ...
  code ...
}
{% endhighlight %}

Here is dependency injection at work: Room & Message are services that are injected or passed into the home controller function. The Room and Message services are housed in their own JS files and contain their own logic. But since that they are passed into HomeCtrl they are available to the home controller for use with out home control having to contain their respective logic. 

See I told you Dependency Injection is not as painful as it sounds. Dependency injection is a prevalent feature of the AngularJS architecture and the concept is a little more detailed than what I have presented here. For further documentation on AngularJS’s dependency injection system refer to the [developer guide](https://docs.angularjs.org/guide/di). I hope this article was helpful in introducing this important topic. If you have other topics you would like me to discuss feel free to contact me and in the message just reference the Newbie to Newbie series.
