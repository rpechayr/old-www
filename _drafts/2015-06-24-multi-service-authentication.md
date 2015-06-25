---
layout: post
title: Authentication users accross multiple web applications
category: blog
---

# Authenticating users accross multiple web applications 

This post discusses different approaches you can have to perform multi-apps authentication. This is interesting for people willing to build a service oriented infrastructure, where users need to access multiple apps with a single authentication system, beyond the monolithic web app approach many startups begin with.

When your codebase grows and your development team expands, putting all features in one single web applications and having all developers working on the same project can create some frictions. Any relation between to objects can be created, and if you are using an MVC framework like rails, cascading callbacks can become a real mess for large applications. A common and good approach to solve that is to split your codebase into smaller, clearly identified components. Sometimes you even have several products corresponding to multiple web applications. For example, google authenticates the users on https://google.com/accounts, and the corresponding session persists accross all google products, like youtube, gmail, blogger, etc.

Another example of shared authentication is when an independent application uses a popular platform for authentication. This is what happens when some websites asks you to sign in using Facebook, Google, or Githu for example. The most common technology for this is OAuth 2.0, which was not specifically designed for authentication, but still commonly used for that. Oauth 2 has many other benefits than allowing third party authentication, which is the reason why it has become the most used standard. 

##How authentication works on simple web applications

Authenticated web apps usually work by first asking the user for a username and a password, and then issuing a session cookie, enabling the user to be authenticated on every request without re-typing his credentials. On every request, the session cookie is generally used to match a user against a database ([Devise](https://github.com/plataformatec/devise) gem does that pretty well for Ruby on Rails). 

##Authenticate once for many services

The idea behind OAuth 2, is that a remote service has authenticated a user for you (the application), enabling you to query the remote service on this user's behalf. A particular case of that is to fetch the users profile. Therefore combining OAuth 2 with a simple profile API allows you to authenticate and identity a user on a remote platform, without asking him to set a password, type his name, or email, etc. Still, the user is signed in on your local application as a regular user, and once his session is open, there is no difference between a OAuth user and a regular one. For example, the identity of the user is not checked against the remote service on every request. If the user is signed out from the remote service (or his account is blocked), he will still be authenticated on your platform.

In other words, with OAuth2, user profiles are generally replicated between the central authentication system and the applications, which means they can be out of sync. This is also an advantage because the central authentication system won't be called on every request and therefore be a small application in terms of infrastructure. With such a system you can scale you infrastructure independently for every service, including the authentication component. 

##Problems when having multiple sessions 

If you want to provide a consistent user experience such as people sign in and out once for every application, this can be tricky. 

The main problems you can have are : 

- User info synchronization : copying information about the user on multiple databases will make them out of sync, especially if you allow local profile modifications (like when you sign in using facebook and the change your avatar on the application) 
- Single sign **out** : signing out from an application does not sign you aout from other applications. It can be even worse. If you application redirects any user to the 

Signing in once is not an issue because the session on every application will be lazily created, which means you will be authenticated on a given service only if you visit it. The problem is when you want to sign out once for all platform. Here is how google performs it :

1. The user issues a sign out request from a service (say Blogger)
2. The user is disconnected from the service and redirected to a sign out endpoint to google accounts
3. Google accounts disconnects the user and redirects him to all other services (without disclosing the list of services)
4. On every service the user is disconnected and redirected to the next service to be disconnected 
5. Finally the user is redirected back to the initial service 

You can observe this behavior in google Chrome by logging in to Youtube and gmail for example. Then, if you are using Chrome, open developer tools and go to the network tab. Then check the "preserve logs" option. You can then disconnect from Youtube for example. You'll observe a sequence of requests to all google services in which you had a session. The way google does it is to redirect the user using javascript. The reason why they don't perform http redirects is because the sequence can be quite long (up to 20 in some extreme cases), and some users could rich a "too many redirects" error. 

From the outside, the is no way to see what s the sequence of applications to visit for siging out. My guess is that the sequence is stored on the backend and every service queries it to find out what the next service is. The way the backend maintains a list of active services is quite straightforward. It is just a matter storing the information on every sign in, which is possible because in OAuth for example, the user agent visits the authentication server. 

- Intro : 
  1. Central authentication system, distributed sessions
  2. How to sign out from all services
  3. How I found out about how google handles it
  
## General workflow



## Notes 
- Not handled though `301/302` response : this is because there can be dozens of redirects (but typically 2 or 3 for a standard user) and the browers limits the number of redirections to avoid infinite loops
- The next service is probably resolved by a backend call to the authentication service
- Upon sign in on a specific service, the user is probably marked as signed in on this specific service on the authentication service side. 
- No delete requests are possible 

##TODO
- Specific terms and impl reference for
  - direct-auth : process where users are authenticated on every http request on a remote server ()
  - replicated-auth : process where users are authenticated "once" on a central server, and re-authenticated on every services. This is how OAuth applications work. The user actually exists in the databse of every application