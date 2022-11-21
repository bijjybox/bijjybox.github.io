---
layout: post
title: TryHackMe
subtitle: Google Dorking
gh-repo: bijjybox/beautiful-jekyll
gh-badge: [star, fork, follow]
tags: [test]
comments: true
---
![](https://miro.medium.com/max/828/0*afBE7PqDJtS4pxga.png)
## Task One:

This section goes over a quick explanation of Google as a search engine and website indexer, that uses spiders/crawlers to gather keywords and website urls to make a dictionary on websites to suggest when using the search engine.
{: .box-warning}
*ANS 1 :* There is nothing needed to be done. Press complete and away we go!

## Task Two:

This section explains the use of crawlers and the “How” a website is indexed.
{: .box-warning}
*Hint*: Your best way of solving these questions is using ctrl+f, which will bring up a word finder, and search the webpage for keywords or clues of the question being asked.
{: .box-note}
*Question 1*: Name the key term of what a “Crawler” is used to do

The first answer can be found reading this paragraph or (ctrl+f) searching for the word “crawler” and seeing what sentences contains a word that is the answer:

![](https://miro.medium.com/max/828/0*tFL-MMbPlEZxHVP2.png)
{: .box-warning}
*ANS 1*: index
{: .box-note}
*Question 2*: What is the name of the technique that “Search Engines” use to retrieve this information about websites?

This answer is a little harder to find and requires you to crawl the paragraphs yourself looking for the quotation key word “Search Engine” hint that the question is offering.

![](https://miro.medium.com/max/828/0*3178lRvY8kvv9VPu.png)
{: .box-warning}
*ANS 2*: crawling
{: .box-note}
*Question 3*: What is an example of the type of contents that could be gathered from a website?

Searching for the word “content” will help with this answer. The question has a couple of possible answers of the type of content that can be gathered from a website. It could be urls to other websites posted on the crawled website, could be information on specific subjects, or keywords.
{: .box-warning}
*ANS 3*: keywords

## Task Three

This section goes over SEO and how websites can be ranked by the amount of keywords and hashtags that it meets in searchability for users, social media, and search engines.

These questions will require using the site: [https://seositecheckup.com/](https://seositecheckup.com/) and using tryhackme.com to find the answers.

{: .box-warning}
*Link for test*: [https://seositecheckup.com/seo-audit/tryhackme.com](https://seositecheckup.com/seo-audit/tryhackme.com)

![](https://miro.medium.com/max/828/0*ixmEeNuuQUjCtUES.png)
{: .box-note}
*Question 1*: Using the SEO Site Checkup tool on “tryhackme.com”, does TryHackMe pass the “Meta Title Test”? (Yea / Nay)

Check out the websites’ description Meta Title Test. Result has a passing green check mart
{: .box-warning}
*ANS*: Yea
{: .box-note}
*Question 2*: Does “tryhackme.com” pass the “Keywords Usage Test?” (Yea / Nay)

The website has a red x mark meaning it did not pass the Keywords Usage Test and says there are not any keywords used.4
{: .box-warning}
*ANS*: Nay
{: .box-note}
*Question 3*: Use [https://neilpatel.com/seo-analyzer/](https://neilpatel.com/seo-analyzer/) to analyse [https://blog.cmnatic.co.uk](https://blog.cmnatic.co.uk/): What “Page Score” does the Domain receive out of 100?

![](https://miro.medium.com/max/828/0*J6z0iGiAP3ewHRdD.png)

Look at the On-Page SEO Score
{: .box-warning}
*ANS*: 81/100
{: .box-note}
*Question 4*: With the same tool and domain in Question #3 (previous): How many pages use “flash”?

Don’t see anything mentioning flash, or Adobe flash, so I am going with 0.
{: .box-warning}
*ANS*: 0
{: .box-note}
*Question 5*: From a “rating score” perspective alone, what website would list first? tryhackme.com or blog.cmnatic.co.uk

The site — tryhackme had a score of 62, while blog.cmnatic.co.uk has a score of 81
{: .box-warning}
*ANS*: blog.cmnatic.co.uk

## Task Four

This section goes over Robots.txt and which file directories on a sitemap can be allowed or disallowed to be indexed by crawlers(and also can be limited to which crawlers can access these directories like if it is google or a bing crawler).
{: .box-note}
*Question 1*: Where would “robots.txt” be located on the domain “ablog.com”

When a site is first being accessed by crawlers, there will be a page where information is hidden under, and that is often in the directory/text file called “ robot.txt “ which will hold the sitemap for crawlers to get the index they want to yoink
{: .box-warning}
*ANS*: ablog.com/robots.txt
{: .box-note}
*Question 2*: If a website was to have a sitemap, where would that be located?

The sitemap is the file that will hold an xml format file with the website’s index for crawlers to use.
{: .box-warning}
*ANS*: sitemap.xml
{: .box-note}
*Question 3*: How would we only allow “Bingbot” to index the website?

This requires using the code that is mentioned in the lesson of User-Agent, and the specific crawler allowed instead of All which would be *

![](https://miro.medium.com/max/828/0*RNlLfKU-d1qfNOoW.png)
{: .box-warning}
*ANS*: User-Agent: Bingbot
{: .box-note}
*Question 4*: How would we prevent a “Crawler” from indexing the directory “/dont-index-me/”?

Using the lesson example to solve this question of limiting a index directory

![](https://miro.medium.com/max/640/0*p0XSXbsApUowyraJ.png)
{: .box-warning}
*ANS*: Disallow: /dont-index-me/
{: .box-note}
*Question 5*: What is the extension of a Unix/Linux system configuration file that we might want to hide from “Crawlers”?

The hint for this says “system files are usually 3/4 characters!” so that means the configuration file extension is slightly short than the usual abbreviation of config
{: .box-warning}
*ANS*: .conf

## Task Five

This section is pretty self explanatory after reading the first few paragraphs of this lesson.
{: .box-note}
*Question 1*: What is the typical file structure of a “Sitemap”?

As mentioned in a previous lesson, if you look at the file format for “/sitemap.xml “, it uses a xml format
{: .box-warning}
*ANS*: xml
{: .box-note}
*Question 2*: What real life example can “Sitemaps” be compared to?

The sitemap is the map to help the little crawlers to not get lost on a website.
{: .box-warning}
*ANS*: map
{: .box-note}
*Question 3*: Name the keyword for the path taken for content on a website

The first explanation of routes are in the lesson’s sentence “The blue rectangles represent the **route** to nested-content, similar to a directory I.e. “Products” for a store.”
{: .box-warning}
*ANS*: route

## Task Six

Now to the meat of the whole “Google Dorking”/Google Fu by using the index categorizations for websearches that Google has meticulously gathered. All those crawlers can now be used in reverse uno fashion for your speedy research.

![](https://miro.medium.com/max/828/0*Gn3vIDTkEHeu5LJB.png)

![](https://miro.medium.com/max/640/0*AyJDYJaY4lRu9Mrv.png)

![](https://miro.medium.com/max/828/0*Q79S149PxNh0R5De.png)

![](https://miro.medium.com/max/720/0*XD8cNFDy25V01xE0.png)
{: .box-note}
*Question 1*: What would be the format used to query the site bbc.co.uk about flood defences?

You want to use the tag [index:] the website [bbc.co.uk] and the topic [flood defences]
{: .box-warning}
*ANS*: site: bbc.co.uk flood defences
{: .box-note}
*Question 2*: What term would you use to search by file type?

Looking at the chart, filetype: would be the option
{: .box-warning}
*ANS*: filetype:
{: .box-note}
*Question 3*: What term can we use to look for login pages?

The Hint in this says “term: query” so if you think about an intitle on a web address and web page title, it may say website.com/login, or the page title itself may say “login”
{: .box-warning}
*ANS: intitle: login

And, we completed this room as well 🙌
