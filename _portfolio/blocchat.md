---
layout: post
title: BlocChat
thumbnail-path: "assets/bloc-chat/laptop.png"
short-description: A real-time chat room SPA built using AngularJS & Google's Firebase data source.

---

{:.center}
![]({{ site.baseurl }}/assets/bloc-chat/laptop.png)
![]({{ site.baseurl }}/assets/bloc-chat/mobile-small.png)

### Project

Angular is a JavaScript framework used to build single-page web applications that allow the user to dynamically interact with the application. Using Angular along with Google's Firebase to handle the back-end, was the perfect solution to create a responsive chat room prototype.

BlocChat creates a chat space where the user can send messages associated with different rooms. Users can also set their username, create new rooms and navigate between them to see different messages associated with each room, all in real-time.

### Challenge

The largest challenge I faced in this project was pulling together multiple parts of the framework to achieve a seamless single action for the user. This required a learning how to connect multiple components across the view, controller and services to achieve the desired result. One example of this was creating new messages and sending the data to Firebase.

### Solution

Getting new message to send required wiring together several components within the framework. Starting with the view.
{% highlight javascript %}
<form name="add-message" class="form-inline" ng-submit="home.send()">
  <div class="form-group">
    <input type="text" class="form-control" ng-model="home.newMessage"
     placeholder="Write your message here ..."/>
    <button type="submit" class="btn btn-default">Send</button>
  </div>
</form>
{% endhighlight %}

The `ng-model="home.newMessage"` directive binds the value of the text input to the content of the message.
The `ng-submit="home.send()"` directive calls the send method in the home controller to shuttle the data to the message service that interacts with Firebase.

Those two little lines of code do so much. But the interactions only build from there.

Within the home controllers send method, the roomId is connected to a previous method to set the current room the user is in. Capturing the time the message is sent requires another service, TimeFilter, which sets and filters the format appropriately. And setting the username required using Angular's cookie service.
{% highlight javascript %}
this.send = function() {
  Message.addMessage({
     roomId: this.currentRoom.$id,
     content: this.newMessage,
     sentAt: TimeFilter(),
     username: currentUser
    });
  this.newMessage = "";
};
{% endhighlight %}

As the Message service is called the addMessage function adds the new message data to the Firebase database. The messages are then filtered for the current room and displayed in the browser. All happening in real-time for the user.

### Results

Users can send messages in one seamless action.

<div style="width:100%;height:0;padding-bottom:56%;position:relative;">
  <iframe width="100%" height="100%" style="position:absolute" src="https://www.youtube.com/embed/-gdLcQw8fgs" frameborder="1" allowfullscreen></iframe>
</div>


### Conclusion

BlocChat was the first project I built from scratch using the Angular framework. With that came daily challenges and lessons learned. Utilizing user stories and testing for those specific outcomes helped break the project down into meaningful and manageable components. Overall, this project helped me demystify the Angular framework and how to put it into action. I also gained new skills working with the Firebase database and using Bootstrap to structure my layouts.
