---
layout: post
title: BlocJams
thumbnail-path: "assets/bloc-jams/laptop-album-view.png"
short-description: A Spotify-like web based music player built using JavaScript & JQuery.

---

{:.center}
![]({{ site.baseurl }}/assets/bloc-jams/album-player.png)

### BlocJams Demo
JavaScript, JQuery, CSS, HTML5

### Project

BlocJams is a prototype digital music player, like Spotify (minus the commercials). BlocJams was my first front-end project. The focus was on using HTML5, CSS styling, DOM scripting, JQuery, and JavaScript. BlocJams also introduced best-practices for responsive design, CSS transitions, and project management through Git and GitHub.

### Challenge

To have a truly complete user experience, BlocJams needed a player bar to allow the user to play/pause songs, skip songs and fully functional seek bars. Two different seek bars are used, one to adjust the progress of the song and one to control the volume of the song. For optimization, the seek bar functionality for song control and the volume control is achieved using the same code. The seek bars were developed to respond to mouse events to click or drag the thumb to a new location within the seek bar.

### Solution

First, the seek bars need to be configured to recognize a clicking event.

{% highlight javascript %}
var setupSeekBars = function() {
   var $seekBars = $('.player-bar .seek-bar');

   $seekBars.click(function(event) {
       var offsetX = event.pageX - $(this).offset().left;
       var barWidth = $(this).width();
       var seekBarFillRatio = offsetX / barWidth;

  if($(this).parent().attr('class') == 'seek-control') {
    seek(seekBarFillRatio * currentSoundFile.getDuration());
  } else {
    setVolume(seekBarFillRatio * 100);
  }
  updateSeekPercentage($(this), seekBarFillRatio)
});
{% endhighlight %}

Both seek bars are selected using the JQuery `$('.player-bar .seek-bar')` selector. The seek bar to be updated will be determined by the target of the event. On a click event, the seekBarFillRatio is calculated as a ratio of the offsetX and the width of the target element. OffsetX is calculated using the JQuery-specific `event.pageX`  which gives the horizontal coordinate where the event occurred minus the horizontal (left) offset distance of the target element, which refers to the seek bar that was clicked. To differentiate functionality between the two different seek bars, we check if the parent class for the element clicked is the seek-control for the song player. If so a seek function is called to set a new position within the song file. Otherwise, it is the volume control seek bar that has been activated, and the set volume function is called to set the new volume level. The fill ratio and the seek bar to be updated ``($this)`` are then passed to the updateSeekPercentage( ) function.

The updateSeekPercentage( ) function updates the width of the progress bar and position of the thumb in the view. Like most music players the seek bar also serves a progress bar. Accordingly, the updateSeekPercentage( ) function is also called to update the seek bar and thumb position as the song plays.

The drag event uses the same calculations and seek/ volume control, but dragging the seek bar's thumb requires three different user events: mouse down, mousemove and mouseup.

{% highlight javascript %}
var setupSeekBars = function() {
  var $seekBars = $('.player-bar .seek-bar');

     ...

 $seekBars.find('.thumb').mousedown(function(event){
    var $seekBar = $(this).parent();

    $(document).bind('mousemove.thumb', function(event){
       var offsetX = event.pageX - $seekBar.offset().left;
       var barWidth = $seekBar.width();
       var seekBarFillRatio = offsetX / barWidth;

      if($seekBar.attr('class') == 'seek-control') {
        seek(seekBarFillRatio * currentSoundFile.getDuration());
      } else {
        setVolume(seekBarFillRatio * 100);
      }

      updateSeekPercentage($seekBar, seekBarFillRatio);
      });

     $(document).bind('mouseup.thumb', function() {
         $(document).unbind('mousemove.thumb');
         $(document).unbind('mouseup.thumb');
     });
   });
 };

{% endhighlight %}

The drag event is associated with moving the thumb on the seek bar by using ``$seekBars.find('.thumb')``. Since the mousedown event is attached to both the song and the volume control, here which seek bar activated is set to be the parent of ``('.thumb').mousedown``. As soon as the mouse is clicked down the event handler controls both the mousemove and mouse up events. When the mouse moves the .bind method attaches the event handler directly to the seek bar and calculates the seekBarFillRatio, calls the seek or set volume functions according to which parent element is attached to the mousedown event and calls updateSeekPercentage( ). Here the mousemove event is attached to the $(document) so the user can drag the thumb after mousedown, even if the mouse leaves the small space of the seek bar element. This provides a better user experience.  When the mouse is released the .unbind method removes the previously attached event handler for both mousemove and mouseup.

### Results

Users can send messages in one seamless action.

<div style="width:100%;height:0;padding-bottom:56%;position:relative;">
  <iframe width="100%" height="100%" style="position:absolute" src="https://www.youtube.com/embed/ai-pEQcX1BQ" frameborder="1" allowfullscreen></iframe>
</div>

<div style="width:100%;height:0;padding-bottom:56%;position:relative;">
  <iframe width="100%" height="100%" style="position:absolute" src="https://www.youtube.com/embed/mMnGyyK7l70" frameborder="1" allowfullscreen></iframe>
</div>


### Conclusion

Creating this functionality in BlocJams gave me a greater understanding of DOM scripting. After developing the seek bars for BlocJams I will no longer take for granted the simplicity of seeking when I am listening to my favorite songs.
