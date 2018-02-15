---
layout: post
title: BlocJams - Angular Edition
thumbnail-path: "assets/bloc-jams/laptop-landing.png"
short-description: Refactoring our Spotify-like music player with AnjularJS.

---

{:.center}
![]({{ site.baseurl }}/assets/bloc-jams/laptop-landing.png)
![]({{ site.baseurl }}/assets/bloc-jams/mobile.png)

### BlocJams Demo
AngularJS, JavaScript, CSS, HTML5

### Project

BlocJams is a prototype digital music player, like Spotify (minus the commercials). Previously BlocJams utilized Javascript and jQuery for DOM manipulation. This project refactors the previous incarnation of BlocJams to create a single-page application using Angular JS.


### Challenge

This was my first introduction to the Angular framework. My greatest challenge in refactoring this project using Angular was shifting my thought process to "think in Angular." New vocabulary were initially road blocks to understanding the greater architecture: the model, the view, controllers, MV*, directives, two-way data binding, services, dependency injection, $scope, SPA's . . .  Angular should come with a "fancy words" warning label. Wading through all the terminology obscured the bigger picture.

### Solution

DOM Manipulation vs Angular Framework

In the jQuery implementation of BlocJams, the pages were laid out and then jQuery was used to make the pages dynamic. JQuery uses selectors to find DOM elements and event handlers, when triggered,  execute code to update or change the DOM. This is a reactive, granular way of thinking.  Whereas with Angular, you have to think like an architect: creating and connecting spaces, letting Angular do the work for you.

Let's compare how we played a song from the player bar using jQuery to the Angular implementation. It's a music player, so playing a song is kind of an essential function.

Let start with the original BlocJams play button:
{% highlight html %}
<div class="control-group main-controls">
  ...
    <a class="play-pause">
        <span class="ion-play"></span>
    </a>
  ...
</div>
{% endhighlight %}

That's it. The functionality of what is happening is completely obscured. You have to dive into the Javascript to see how the song actually gets played and how the view is updated to show the appropriate play or pause buttons, which requires DOM scripting.

First,  the appropriate elements have to be selected.
{% highlight javascript %}
var $playerBarPlayPauseButton = $('.main-controls .play-pause');
{% endhighlight %}

Then a click handler is called to toggle between play and pause.

{% highlight javascript %}
$(document).ready(function() {
 . . .
 $playerBarPlayPauseButton.click(togglePlayFromPlayerBar);
});
{% endhighlight %}

The click handler updates the HTML with appropriate play or pause button icon and plays or pauses the current sound file depending on if the current sound file is already paused or not.
{% highlight javascript %}
var togglePlayFromPlayerBar = function() {

  var songNumberCell = getSongNumberCell(currentPlayingSongNumber);

  if( currentSoundFile.isPaused() ) {

    songNumberCell.html(pauseButtonTemplate);

    $('.main-controls .play-pause').html(playerBarPauseButton);
    currentSoundFile.play();

  } else if( currentSoundFile ) {

    songNumberCell.html(playButtonTemplate);

    $('.main-controls .play-pause').html(playerBarPlayButton);
    currentSoundFile.pause();

  }
};
{% endhighlight %}

The Angular implementation of the same functionality is much more declarative.
{% highlight html %}
<a class="play-pause">
  <span class="ion-play"
    ng-show="!playerBar.songPlayer.currentSong.playing"
    ng-click="playerBar.songPlayer.play()">
  </span>
  <span class="ion-pause"
    ng-show="playerBar.songPlayer.currentSong.playing"
    ng-click="playerBar.songPlayer.pause()">
  </span>
</a>
{% endhighlight %}

From the view, you can see that the play icon (ion-play) shows when the current song is not playing and when clicked the songPlayer.play( ) function is called. (As you can probably guess the songPlayer.play( ) function plays the current song file.) Similarly, the pause icon (ion-pause) is displayed when the current song is playing and when clicked songPlayer.pause( ) is called, which pauses the current song file. Here playerBar is the controller that connects this view to the songPlayer service.

The above comparison also illustrates how Angular separates components of the application. The view presents the data (model), services perform tasks, DOM manipulation is achieved through directives and controllers connect the view to the data it needs.

### Results

<div style="width:100%;height:0;padding-bottom:56%;position:relative;">
  <iframe width="100%" height="100%" style="position:absolute"  src="https://www.youtube.com/embed/JPHPQR8roUg" frameborder="1" allowfullscreen></iframe>
</div>


### Conclusion

At the heart of it, learning to "think in Angular" comes down to really understanding that it is a framework, not a library. And therefore functions in a very different manner. jQuery does not dictate how you should organize your code. Where as Angular does. The code is dispersed over the view, controllers, services, and directives. When included in the module all of the components with in this framework are available and components can be injected into each other for use within other components using AngularJS's dependency injection mechanism. This is a fundamental paradigm shift in how you structure your code. Through this project, I was able to sort through all the new terminology and learn the greater overarching concepts of the AngularJS framework.
