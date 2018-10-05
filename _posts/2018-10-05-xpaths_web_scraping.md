---
title: Useful xpaths and tips for web scraping
key: 20181005
tags: xpath web-scraping
---

The internet is chaotic, the structure of websites follow no rules, have no reservations 
and will make sure that they are different from that other site that looks the same but it's made using the latest x javascript technology  
because it's cooler and the frontend developer wanted to learn something new, <i> end of rage <i>.

But even in this chaos, there are some basic principles that most websites follow.  
In web scraping, `most websites` is your favorite keyword by the way :smile:
Diving into specifics now:

## [Xpath](https://www.w3schools.com/xml/xml_xpath.asp) expressions to get title, logo and/or video
1. ##### Title extraction
* `//meta[@property='og:title']/@content` the best if it exists
* `//meta[@name='description']/@content` depending desired length this tag also contains a good summary text
* `(//*[contains(@*,'content')]//h1 | //*[contains(@*,'content')]//h2)[1]` stay with me! lot of websites use an element 
named `content` to wrap text, it's first header must be a good title
* `//title` game over

1. ##### Logo extraction
* `//meta[@property='og:image']//@content` surprise surprise!
* `(//*[contains(@*,'content')]//img/@src[contains(.,'jpg')])[1]` the same as above only this time, the first jpg will be returned.
Alternatevily use `not(contains(., 'gif'))` to get all non gif images and then decide based on size or other factors

2. ##### Video extraction
* `//meta[@property='og:video']//@content` doesn't exist often
* `//iframe[contains(@src,'youtube.com')]/@src` youtube embedded videos
* `//iframe[contains(@src,'player.vimeo.com')]/@src` vimeo embedded videos
* `/div[@id='main']//embed[@type='application/x-shockwave-flash']/@src` for us 90s boys

## General tips
* Respect [robots.txt](http://www.robotstxt.org/) and don't spam people, 
* [sitemaps](https://en.wikipedia.org/wiki/Site_map) are your friends, even if they don't follow [Google's](https://support.google.com/webmasters/answer/156184?hl=en) format, they are still full of internal links,
* `xpath` won't work on react, angular generally any heavy javascript website, use [PhantomJS](http://phantomjs.org/) or [Headless Chrome](https://developers.google.com/web/updates/2017/04/headless-chrome) (new kid on the block), [Selenium](https://www.seleniumhq.org/) or something equivalent that can
first convert javascript to html and then use xpath. 
