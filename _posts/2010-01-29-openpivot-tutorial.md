---
layout: post
title: Openpivot tutorial
---


A few weeks ago I introduced [openpivot]({% post_url 2009-12-04-use-openpivot-to-make-quick-flat-file-processing %}) <a href="http://rpechayr.wordpress.com/2009/12/04/use-openpivot-to-make-quick-flat-file-processing/">openpivot</a> on this blog. It is a powerful command line utility to build pivot tables. Assume you have the following simple file :

[code]
company Name;year;NbEmployee;profit;sector
Microsoft;2001;30000;1.5;IT
Microsoft;2003;33001;1.5;IT
Apple;2001;23001;1.5;IT
Apple;2003;23001;1.5;IT
Google;2001;43001;1.5;IT
Google;2003;43001;1.5;IT
[/code]

What a complex data set ! <a href="http://en.wikipedia.org/wiki/Pivot_table">Pivot tables</a> are very powerful tools allowing you to make summary of your data from the point of view of your choice.  Suppose you want to have per year and per sector, the average number of employees accross all entries. Here it is implicit that these entries will be only different companies as here there is 1 line per company/year. Note also that there is only 1 sector.  So as there are 2 different years represented, and only 1 sector, you will get 2 statistics. Doing this with openpivot is simple. You only need to describe what pivot table you want in an xml file configuration and it will look like this:

[sourcecode language="xml"]
&lt;?xml version=&quot;1.0&quot; encoding=&quot;UTF-8&quot;?&gt;
&lt;problem&gt;
  &lt;defaultaccumulation value=&quot;sum&quot;/&gt;
  &lt;columnlist&gt;
    &lt;col id=&quot;NbEmployee&quot; accumulation&quot;average&quot;/&gt;
    &lt;col id=&quot;profit&quot;/&gt; &lt;!-- will use default accumulation --&gt;
  &lt;/columnlist&gt;
  &lt;rowsequence&gt;
    &lt;row id=&quot;year&quot;/&gt;
    &lt;row id=&quot;sector&quot;/&gt;
  &lt;/rowsequence&gt;
&lt;/problem&gt;
[/sourcecode]

This configuration is almost the pivot table we want for our example, except that there is an additional column we want to have a statistics for. Note that what we call row here the variable sequence from which you want to pivot. There are taken into account in the order specified. Just like in excel, changing the order of variables will offer a diferent view on your data.  You are now ready to have your pivot table, all you need is to call openpivot to compute this from your sample data :
<pre><span style="color:#ff0000;"><strong>% openpivot -p configfile.xml -o outputfile.csv inputfile.csv</strong></span></pre>
Et voila ! This will write your pivot table in the outputfile.csv.  Please drop me a line if you have problem to build openpivot. I have tested it on mac os 10.6 and various linux distribution, and it seemed to work