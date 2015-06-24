---
layout: post
title: Beyond the Oauth 2 client - Anthentication users accross multiple web applications
category: blog
---

# Beyond the Oauth client - Anthentication users accross multiple web applications 

This post discusses different approaches you can have to perform multi-apps authentication. This post tries to explain how google account authentication works. This is interesting for people willing to build a service oriented infrastructure, where users need to access multiple apps with a single authentication system, beyond the monolithic web app approach most startup begin with.

##How authentication works on web applications

Authenticated web apps usually work by first asking the user for a username and a password, and then issuing a session cookie, enabling the user to be authenticated on every request without re-typing his credentials. On every request, the session cookie is used to match a user against a database ([Devise](https://github.com/plataformatec/devise) gem does that really well for Ruby on Rails apps). 

##Authenticate once for many services

There are situations where you want the users to have the same identity accross different services. This can be for 2 reasons : 
- You need your user to be authenticated once accross all your products. This is what google does for example. youtube.com and gmail are different services that share the same authentication service (google accounts).
- You way also want, as an independent application developer, to have your user authenticated to Google (or Facebook, Twitter, yahoo, github, etc.). Protocols


- Intro : 
  1. Central authentication system, distributed sessions
  2. How to sign out from all services
  3. How I found out about how google handles it
  
## General workflow

1. The user issues a sign out request from a service (say Blogger)
2. The user is disconnected from the service and redirected to a sign out endpoint to google accounts
3. Google accounts disconnects the user and redirects the user to all other services (without discosing the list of services)
4. On every service the user is disconnected and redirected to the next service to be disconnected 
5. Finally the user is redirected back to the initial service 

## Notes 
- Not handled though `301/302` response : this is because there can be dozens of redirects (but typically 2 or 3 for a standard user) and the browers limits the number of redirections to avoid infinite loops
- The next service is probably resolved by a backend call to the authentication service
- Upon sign in on a specific service, the user is probably marked as signed in on this specific service on the authentication service side. 
- No delete requests are possible 

##TODO
- Specific terms and impl reference for
  - direct-auth : process where users are authenticated on every http request on a remote server ()
  - replicated-auth : process where users are authenticated "once" on a central server, and re-authenticated on every services. This is how OAuth applications work. The user actually exists in the databse of every application