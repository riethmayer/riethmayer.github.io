---
layout: post
title:  "Use command line tools more often!"
date:   2014-04-04 18:40:52
categories: automation
---

My team already makes fun of me about my little shell-script-crusade.

## 7 command-line tools for data science (7cltfds)

When coming across
[7 command-line tools for data science](http://jeroenjanssens.com/2013/09/19/seven-command-line-tools-for-data-science.html)
I liked the article. Installing all the different tools was bit of a
pain as it required python and nodejs to be setup. I hesitated to
install the tools and start playing as I didn't have a cool idea or
problem to solve.

## Leaving the comfort zone

One day, when turning to lead-qualification for our sales-team in Brazil, I
looked at a website, which offered a lot of content about online-shops.

My first approach on scraping the site for leads was building a ruby
script. It's been a javascript paginated site, so the straight forward
solution was to utilize a webdriver to scrape and click. Building this with
[mechanize](https://github.com/sparklemotion/mechanize) was pretty
straight forward.

Then I got lost in refactoring of throw-away code because it was so
ugly. I wasn't happy with maintaining another project to do future
lead-qualification of other sources, this smelled to me.
So I thought about how to shell-script it, now I had a reason
to come back to installing the tools mentioned in 7cltfds.

Instead of using the pagination of the site, I utilized the search as
an api to search for all shops (eventually not so nice).

{% highlight bash %}
curl http://example.com/site-with-relative-urls -s > results.txt
cat results.txt | scrape -be "li a" | grep "</a>" | \
scrape -be "a" > links.html
cat links.html | xml2json | jq -c ".html.body.a[]" | \
json2csv -k href,'$t' > relative_links.csv
mkdir results
cat relative_links.csv | cut -d "," -f 1 | while read relative; do \
`curl -L "http://www.ebit.com.br$relative" > ./results$relative.html`; \
done
ls -1 results | while read page; do \
(scrape -be 'table tr td:first-child a' $page |  xml2json | \
jq '.html.body.a.href' > $page.link); done
find . -iname "*.link" | while read "link"; do \
echo "${link/.html.link/}"; done | while read "file"; do \
echo "${file/.\//},`cat $file.html.link`"; done > shops.csv
{% endhighlight %}

Although the shell-script looks like a mess, creating the shell script
line by line with temporary results in text files (a very nice
feedback-loop), it's been straight forward to create.

## Knowledge transfer of shell scripts

The best side-effect is, I've been able to go show the shell-script to
our business intelligence. Besides scraping future
lead-qualification sites, BI had a huge learning curve to solve
recurring tasks when generating reports from other apis learnign more
shell commands.

## Learn more shell commands

So what are the next steps to shell-script mastery?

Get familiar with the main operators like the pipe `|`, redirecting in
and out (`<`, `>`), firing off subshells `()` and use
control-constructs like `while`, `if` and `for`.

Learn using basic commands like `echo`, `cat`, `find`, `cut`, `grep`, `ls`,
`xargs`, `wc`, `sort`, `sed` and `awk`. Unleash extra utility by
installing `curl`, `xml2json`,
`scrape`, `jq` and `json2csv`.

Once you think you're done, look at the few 100 unix commands
installed on your machine :)

    ls /usr/bin | less

Cheers!
Jan
