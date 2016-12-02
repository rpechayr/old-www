---
layout: post
title: The single sign out problem -  Google Accounts analysis
category: blog
---
**TL;DR** -  SSO technologies are well documented when it comes to signing-in, but handling sign-out can be challenging.

[Single sign on](https://en.wikipedia.org/wiki/Single_sign-on) is now everywhere. It is used in major tech companies like Facebook and Google, who allow their users to be logged on once across all their products, but also generally allow third party service providers to authenticate their users using an OAuth 2.0 interface. More traditional large organizations often have a SSO for their internal websites using different technologies (most of the time, this SSO is based on Microsoft [Active Directory](https://fr.wikipedia.org/wiki/Active_Directory) or [LDAP](https://fr.wikipedia.org/wiki/Lightweight_Directory_Access_Protocol), or both, and exposes a [SAML](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) Interface). The advantage of OAuth 2 is that it also includes complete authorization features to enable third party access to APIs (using OAuth scopes).

## Signing-in, the *easy* part

When implementing SSO features, be it for creating a client or a provider, almost all information you are going to find about OAuth, OpenId, or SAML refers to how sign-in works. For OAuth, you'll learn that it is [not really an authentication solution, whereas OpenID is](http://stackoverflow.com/a/1087071). The basic idea is that you request information about your user to the identity provider, using Web browser redirects. Once you have that information your are going to use it to sign-in this user, even if he has not set a password. Most of the time, when Google / Facebook / Twitter are used for authentication, there is also a possibility to create a standard account by creating a password. Apart from exempting the user from creating a password, being connected to Facebook API for example can allow your application to fetch information about your user directly from your server, like his friends (if the right permission has been granted by the user).

## Signing out, the tricky part

As described above, when a user is signed-in though a SSO, he has 2 sessions. One on the identity provider, and one on the service provider. The process of signing out can mean different things. Do you want to close your session on the service provider only, on the identity provider, or both ?

### 1/ As a third party service provider

When you are a third party service provider, you generally use Facebook login or Google Accounts to make it easier for your users to create an account, or be able to access some APIs like Facebook Graph API. Signing out only means leaving destroying your local session. Be careful though, because this process does not sign users out from the identity provider, so if your product redirects all unauthenticated user to the service provider, signing out will have no effect, because people will be signed out, then redirected to the identity provider (generally already signed-in) and redirected back to your service with a new session. So you must have a landing page asking the user to click on a link to sign-in using Facebook or Google for example.

### 2/ As an application from a family of products

When your company grows and your services expand, there is one point where you will need to have your own SSO. This will allow you to have separated platforms with a consistent user experience. So setting up a SSO is a great pattern. If you are using [Ruby On Rails](http://rubyonrails.org/), [Doorkeeper](https://github.com/doorkeeper-gem/doorkeeper) is a great gem to do that. If you don't want to have your users bother about signing-in on every service, you can automatically redirect them when they are not signed-in. However, you'll face the same issue as described above. Your users will constantly come back signed-in your application. Your need is to not only sign user out from your application, but also sign out from the SSO, and all other services like yours. I recently took a look at how it works with Google Accounts.

### Sign-out process in Google Accounts

Before taking a look at it, I thought Google was handling authentication across their products using [a single cookie](http://stackoverflow.com/questions/18492576/share-cookie-between-subdomain-and-domain) as all applications are subdomains of google.com domains. This is absolutely not the case. A very good reason for that is because youtube.com, google.com, google.fr and blogger.com are different domains and [can't share cookies](http://stackoverflow.com/a/4781366/508080).

When signing out from gmail a few days ago, I noticed my browser visited blogger.com for 0.5 second. I went back to blogger.com and realized I was logged out. Same on youtube.com. I then tried again and checked all http requests (using the [preserve logs](http://stackoverflow.com/a/12282621/508080) option on chrome inspector). Here is a list of request my browser performs when signing out from gmail :

1. Log out from youtube.com:
 `https://accounts.youtube.com/accounts/Logout2?hl=en...&continue=https%3A%2F%2Fmail.google.com%2Fmail&...`

1. Log out from google.fr:
 `http://www.google.fr/accounts/Logout2?hl=en&...&continue=https%3A%2F%2Fmail.google.com%2Fmail...`

1. Log out from google.com:
 `http://www.google.com/accounts/Logout2?hl=en&...&continue=https%3A%2F%2Fmail.google.com%2Fmail&...`

1. Come back to Google log in page:
 `https://accounts.google.com/ServiceLogin?service=mail&...&continue=https://mail.google.com/mail/&...`

The main idea is that the browser actually visits all website from Google on which I have the session and closes the session on all of them. Here are a few interesting remarks about this:

- **There is no logout phase from gmail**. My guess is that the cookie is destroyed in javascript before initiating the whole sequence. The session is probably destroyed on the server as well by google accounts on a next phase.
- **The list and order of service to visit is not in the URLs**. This is probably because the list of services has to be stored on the server for a given user, so there is probably a backend call from every service to know what's the next step.
- **Every request has a 200 status code, not 302 or 301**. My first idea would have been to implement that using redirects. Instead google implemented this using javascript. Every page runs a script that changes your location. I think this is to avoid a [*too many redirects* error](https://support.apple.com/en-us/HT203370) when visiting a dozen of services.
- We don't see it here, but **every service implements the sequence differently**. For example if you run it from Youtube, you'll see that everything runs from an iframe, and ends up by posting a message to the parent window on youtube to sign out the user.

The main reason why signing out from several websites in one click is not well documented is because it is not a very common situation. When this problem arises it is probably fixed using a custom, in-house implementation. The fact the OAuth 2 is not an actual authentication system (authentication must be handled within the identity provider) does not help for that. Google used a quite nice implementation to solve this and I think it can inspire anyone implementing a SSO provider to have users connecting between several products.
