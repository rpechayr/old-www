---
layout: post
title: No, Jekyll isn't for only for developers
category: blog
---
**TL;DR** -  Anyone can use jekyll on github pages just like anyone can post content on tumblr. You just need to know about the add and edit button on github

## _Forever_ blogging platforms

It has become quite difficult to find a reliable and sustainable blogging platform. Posterous has been [shut down by Twitter](http://www.posterous.com/) in April 2013 and Google reader has been [discontinued by Google](http://www.google.com/reader/about/) in July 2013 (which in my opinion let people think that you can only read content on social networks), not to mention Yahoo!’s habit to shut down [everything they buy](http://bgr.com/2014/03/07/yahoo-shutting-down-startups/) (which threatens Tumblr in the long run). 

Online blogging platform have suffered from these recent events, and people are now trying to find alternate solutions that can’t be shut down by FYGT (Facebook/Yahoo/Google/Twitter). This is why the word _forever_ is now the number one keyword for blogging platform marketing. _Forever_ means either [install it yourself](https://ghost.org/pricing/) like in the old days, or [pay 5 dollars per month](https://posthaven.com/). 

## Jekyll

A very powerful remaining alternatives are [Github pages](https://pages.github.com/) powered by [Jekyll](http://jekyllrb.com/). Jekyll is an amazing CMS because it is simple in its conception and does not have a back-office as you just create static pages. Not relying on a back-office is an excellent benefit against time erosion. It will never make you feel like you are using a 5 year old CMS (tumblr interface is ugly). However, [Jekyll](http://jekyllrb.com/) does have powerfull posting features. Just just need to create a new [markdown](http://daringfireball.net/projects/markdown/) file in the `_posts` directory and once deployed it is going to make a new article, available in your blog and in the generated RSS feed.
In terms of performance, github pages allow you to have the most powerful site ever since it is based on static pages (jekyll builds your website when you push it to github) and these pages are eventually [served by a CDN](https://github.com/blog/1715-faster-more-awesome-github-pages).

## Sure, Jekyll has been made for developers

The main _disadvantage_ of [Jekyll](http://jekyllrb.com/) is that it relies on you creating new files in the file system to create new posts. Part of the buzz around Jekyll amongst developers is that they can `git push` a new article and write it using their favorite text editor without a browser. Just visit [Jekyll’s website](http://jekyllrb.com/) that shows a terminal screenshot or some [tutorial](http://learn.andrewmunsell.com/learn/jekyll-by-example/introduction) as a non developer and you will directly think this is not for you :
&gt;Jekyll, while an extremely promising platform, is mainly targeted at tech-savvy bloggers and web masters.

I disagree. Jekyll is not only for developers

## Using Jekyll with _zero_ programming tools

It turns out you can use [Jekyll](http://jekyllrb.com/) without using a terminal, without git, without amazon S3 or anything like that. The only thing you need are:
- A (free) [Github account](https://github.com/join)
- A Jekyll website running on github pages. Technically this can be done by forking someone else’s Jekyll site on github, which is as simple as choosing a theme in tumblr (to fork a repo follow step 1 of [this tutorial](https://help.github.com/articles/fork-a-repo))

Of course, having some knowledge html/css/javascript could help, but it is not fully mandatory. Anyway, many blogs are actually bootstraped by developers but content is managed by non technical people.

Once you have your Jekyll website hosted on github pages, you can create articles, write content (as of July 2014, you can’t upload pictures ni github without a terminal) : 

-   **To create an article**: navigate to the `_posts` directory and create a new file using the *discrete* + button![](https://camo.githubusercontent.com/8fdc501d6746c307ada3d168e5db7b8d1b12cd75/687474703a2f2f636c2e6c792f4c4c65302f6e65772d66696c652e6a7067) as explained [here](https://github.com/blog/1327-creating-files-on-github).
You need to format the name of this file using the current date, like `2014-07-30-my-new-article.md` to make it work, which is not a big deal. 

- **To change the content of any file**: click the *discrete* edit button as [explained here](https://help.github.com/articles/editing-files-in-your-repository). you'll notice that you can add a comment when saving your modifications. This comment will actually be the git commit message, but you don't really to care about it, except that the whole history of your site will be available in the commits tab of your github project. 




