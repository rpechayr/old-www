---
layout: post
title: Interaction between iPhone apps
category: blog
---

Brent Simmons, in his <a href="http://inessential.com/2010/05/23/apps_that_work_together_on_ipad">latest blog post</a> talked about how iPad apps could work together, just like on Mac Os, Apple Scripts, services and apple events to allow applications to basically communicate with each others. The funny thing is this is actually possible on iPhone OS, but apparently not popular enough among developers to take advantage of that. This is even quite easy to do, as explained for <a href="http://inchoo.net/iphone-development/launching-application-via-url-scheme/">example here</a>. The idea is that an application can decide to open a URL, using the following method of UIApplication:

{% highlight objc %}

- openURL:(NSURL\*) url;
  {% endhighlight %}

What is even funnier is that this method is the one used to give calls, access maps, open URL in browser etc. The part not very well explored is that any application can register a particular URL scheme so that it can also respond to the above method when called from another application. Brent Simmons thesis is that this technology could be used by applications to become real services that other applications could use. For example, according to him, you can decide to subsribe to a blog via NetNewsWire from a tweet you read in Twitterrific app. All of this looks very nice but what popped up in my mind is that this technology could be used in some major processes. I will only cover one in this post: Authentification

<h2>Authentification</h2>
With Facebook recent support that they now support OAuth 2.0 and Twitter announce to <a href="http://apiwiki.twitter.com/Authentication">discontinue Basic Authentification as of June 2010</a> in order to support only OAuth, it seems that this mode of authentification will be used by more and more apps. For those not familiar with the subject:
<h3>Basic Authentification on Twitter:</h3>
<ul>
	<li>You just give your Twitter username and password to an Tweeter app so that it can access twitter APIs</li>
	<li>The client will just use them to send GET requests to twitter with your username and password as arguments</li>
	<li>The limit of this is that you must trust this application (or a web application) to give it your password.</li>
</ul>
<h3>OAuth:</h3>
<ul>
	<li>When the app tries to connect to Twitter, it sends an access request to twitter</li>
	<li>The user is sent to "the real twitter" with a message like "<em>this app </em>wants to have access to your Twitter account"</li>
	<li>Then the user tells twitter that <em>this app</em> is granted access or not to your twitter account</li>
	<li>Then twitter sends a message back to <em>this app </em>with an access token, in case the users accepts it.</li>
	<li>The access token is then used by the <em>this app</em> to access twitter, and the user can remove this access directly in twitter if needed</li>
</ul>
Oauth or similar authentification systems have the great advantage that you don't need to give away your password to any app because twitter is the only access authority.

On iPhone, OAuth is always a bit heavy because of the process itself of 1.connecting to the target site, 2. log in,  3.then grant access or not. For example, facebook has implemented a very <a href="http://wiki.developers.facebook.com/index.php/Facebook_iPhone_SDK">nice library</a> that allows any app to access facebook using facebook connect following OAuth scheme. iShareTunes uses this (but also shazam), and it looks like this:

![]({{ site.url }}/assets/facebook-login.png)

This is a nice solution, especially if you try to achieve this kind of results for Twitter which is much more difficult at the moment. But I see very big disadvantages of this method:

<ol>
	<li>It can very well be falsified so that you think this is facebook but you are actually giving your password to the [malicious] app. Don't forget that this happens within the application and you cannot be sure that the app uses the Facebook code as is.</li>
	<li>It is sooooo frustrating to have to type your username password all the time. Keyboard on mobiles are really not comfortable to use, and it is really important to minimize the usage of keyboard input for a user</li>
</ol>
The solution would be really simple! What if Facebook implemented this inside its own application? The process would them be:
<ol>
	<li>The user clicks on "post to Facebook" for example</li>
	<li>If signing in is necessary, the app calls Facebook using a URL scheme and its app_id already registered on Facebook (which is the case today)</li>
	<li>The Facebook app launches and asks you to grant or not access to iShareTunes</li>
	<li>You don't need to sign in because you already did for the Facebook app ! Only this makes the process mush better in terms of user experience</li>
	<li>There is no doubt on the fact that you are actually asked by Facebook to grant access to iShareTunes, which is great.</li>
	<li>When you have accepted, Facebook launches iShareTunes with a URL specified in the original call.</li>
</ol>
All of this is actually not a protocol, it would just be an implementation of current login protocols like OAuth. The only thing facebook would need to do is to wrap this within its <a href="http://wiki.developers.facebook.com/index.php/Facebook_iPhone_SDK">Facebook iPhone SDK</a>.

Imagine that this logic could be implemented by major companies, like Facebook, Twitter itself (this can be done using a webapp interface with no problem), and also Google. Personally I am fed up with giving my Google account password to newsrack and Google Docs clients, because I would really not like to have my gmail account hacked. I would prefer having Google managing this using OAuth.

To close this post I will formulate my own wishes regarding this question:

<ul>
	<li>Apple should just communicate much more on URL scheme for third part applications, even though it is already in the iPhone Programming Guide. This is actually part of the multitasking story.</li>
	<li>Facebook, Twitter, Google to implement it ?</li>
	<li>Why not having a built-in "Apple Connect" available using OAuth (or whatever), so that you don't need to type your passwords all the time, they are just stored once and for all in keychain or more probably in the cloud.</li>
</ul>
