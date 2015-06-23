---
layout: post
title: Google accounts architecture - high level reverse engineering
category: blog
---

#Google account architecture 

- Intro : 
  1. Central authentication system, distributed sessions
  2. How to sign out from all services
  3. How I found out about how google handles it
  
## General workflow

1. The user issues a sign out request from a service (say Blogger)
2. The user is disconnected from the service and redirected to a sign out endpoint to google accounts
3. Google accounts disonnects the user and redirects the user to all other services (without discosing the list of services)
4. On every service the user is disconnected and redirected to the next service to be disconnected 
5. Finally the user is redirected back to the initial service 

## Notes 
- Not handled though `301/302` response : this is because there can be dozens of redirects (but typically 2 or 3 for a standard user) and the browers limits the number of redirections to avoid infinite loops
- The next service is probably resolved by a backend call to the authentication service
- Upon sign in on a specific service, the user is probably marked as signed in on this specific service on the authentication service side. 
- No delete requests are possible 