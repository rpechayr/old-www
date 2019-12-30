---
layout: post
title: Use openpivot to make quick flat file processing
description: I am pleased to announce the release of my first open source project .Openpivot was designed to efficiently reproduce the functionality of MS excel pivot tables
category: blog
---

I am pleased to announce the release of my first open source project . <a href="http://code.google.com/p/openpivot/" target="_blank">Openpivot</a> is designed to efficiently reproduce the functionality of MS excel pivot tables. The problem of MS excel is that old versions (prior to 2007) cannot deal with files larger than 65 000 rows. At work a lot of people are always writing scripts to pre-aggregate the data they have to then be able to play with it in excel.

Openpivot was written in C++ an I have been focusing on performance so that it can been used in large amount of data processing.

The idea is that you have a [large] csv file with headers and you would like to have an aggregated view of this file. You may want to aggregate a certain column or see the sum of a certain columns by entry of another column.
Check it out on Google Code :

<a href="http://code.google.com/p/openpivot/" target="_blank"> http://code.google.com/p/openpivot/</a>

I will post a tutorial in a few days to show how to use it, and you will see how simple it is. So far it is a command line tool and only works on Mac OS and Linux. I will try to have it compatible with Windows when I get to a stable point.
