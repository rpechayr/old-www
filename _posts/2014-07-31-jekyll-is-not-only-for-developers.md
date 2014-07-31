---
layout: post
title: No, Jekyll isn't only for developers
category: blog
---
**TL;DR** -  Anyone can use [Jekyll](http://jekyllrb.com/) on [Github pages](https://pages.github.com/) just like anyone can post content on Tumblr. You just need to know about the [add](https://help.github.com/articles/creating-new-files) and [edit](https://help.github.com/articles/editing-files-in-your-repository) button on Github.

## _Forever_ blogging platforms

It has become quite difficult to find a reliable and sustainable blogging platform. Posterous has been [shut down by Twitter](http://www.posterous.com/) in April 2013 and Google reader has been [discontinued by Google](http://www.google.com/reader/about/) in July 2013 (which in my opinion let people think that you can only read content on social networks), not to mention Yahoo!’s habit to shut down [everything they buy](http://bgr.com/2014/03/07/yahoo-shutting-down-startups/) (which might threaten Tumblr in the long run). 

Online blogging platforms have suffered from these recent events, and people are now trying to find alternate solutions that can’t be shut down by FYGT (Facebook/Yahoo/Google/Twitter). This is why the word _forever_ is now the number one keyword for blogging platform marketing. _Forever_ means either [install it yourself](https://ghost.org/pricing/) like in the old days, or [pay 5 dollars per month](https://posthaven.com/). 

## Jekyll on Github pages

A powerful solution that fits people requirements are [Github pages](https://pages.github.com/) powered by [Jekyll](http://jekyllrb.com/). Jekyll is an amazing CMS because it is simple in its conception and does not have a back-office as you just create static pages. Not relying on a back-office is an excellent benefit against time erosion. It will never make you feel like you are using a CMS from the previous decade (tumblr interface is ugly). However, [Jekyll](http://jekyllrb.com/) does have powerfull posting features. You just need to create a new [markdown](http://daringfireball.net/projects/markdown/) file in the `_posts` directory and once deployed it is going to generate a new article, available in your blog and in the generated RSS feed.

In terms of **performance**, github pages are just static pages so you can't do better (Jekyll builds your website when you push it to Github) and these pages are eventually [served by a CDN](https://github.com/blog/1715-faster-more-awesome-github-pages) to minimize latency.

If you are concerned about the **sustainability** issue, github won't be purchased and shut down by FYGT anytime soon, since they [raised $100 million back in 2012](http://peter.a16z.com/2012/07/09/software-eats-software-development/) and have been profitable for years.

With Jekyll on github pages, you have a long term free online solution (*forever* available) powered by on open source technology (*forever* available even if Github pages are shut down).

## Sure, Jekyll has been made for developers

The main _disadvantage_ of [Jekyll](http://jekyllrb.com/) is that it relies on you creating new files in the file system to create new posts. Part of the buzz around Jekyll amongst developers is that they can `git push` a new article and write it using their favorite text editor without a browser. Just visit [Jekyll’s website](http://jekyllrb.com/) that shows a terminal screenshot or some [tutorial](http://learn.andrewmunsell.com/learn/jekyll-by-example/introduction) as a non developer and you will directly think this is not for you:

>Jekyll, while an extremely promising platform, is mainly targeted at tech-savvy bloggers and web masters.

I disagree. Jekyll is not only for developers.

## Using Jekyll with _zero_ programming tools

It turns out you can use [Jekyll](http://jekyllrb.com/) without using a terminal, git, amazon S3 or anything like that. Everthing happens on github.com. The only things you need are:

- A (free) [Github account](https://github.com/join)
- A Jekyll website running on Github pages. Technically this can be done by forking someone else’s Jekyll site on Github, which is as simple as choosing a theme in Tumblr (to fork a repo follow step 1 of [this tutorial](https://help.github.com/articles/fork-a-repo))

*Of course, having some knowledge html/css/javascript can help, but it is not mandatory. Anyway, many blogs are actually bootstrapped by developers but content is managed by non technical people.*

Once you have your [Jekyll](http://jekyllrb.com/) website hosted on Github pages, you can create articles, write content (as of July 2014, you can’t upload pictures in Github without a terminal or other external tools) : 

-   **To create an article**: navigate to the `_posts` directory and create a new file using the *discrete* + button as explained [here](https://github.com/blog/1327-creating-files-on-github).
You need to format the name of this file using the current date, like `2014-07-30-my-new-article.md` to make it work, which is not a big deal. 

- **To change the content of any file**: click the *discrete* edit button as [explained here](https://help.github.com/articles/editing-files-in-your-repository). you'll notice that you can add a comment when saving your modifications. This comment will actually be the git commit message, but you don't really to care about it, except that the whole history of your site will be available in the commits tab of your github project. 

##Limitations

Jekyll on github pages is great, but don't expect it to have all possible CMS features you could think of. First of all, you can see it as a limitation or not, but you need to learn the basics of the Markdown language. Markdown is the contrary of html. It does not look like code, because it does not have any markup, and you write it naturally like you write a simple text email. You only need to know how to format text as titles, bold, italic, and links, and you can be up and running. 
Also github does not allow uploading non text files directly from github.com website. So if you want to add files like photos you can't do that with this. A simple way around is to use [imgur](http://imgur.com/), which allows you to upload images and get the markdown version to paste it to your Github page. 


