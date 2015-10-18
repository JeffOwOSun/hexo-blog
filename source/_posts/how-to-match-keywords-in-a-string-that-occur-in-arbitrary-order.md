title: How to match keywords in a string that occur in arbitrary order?
id: 105
categories:
  - 'Coding &amp; Geek Stuff'
date: 2015-03-17 19:50:54
tags:
---

dmhy.org sometimes produces ill-formed item names:

Saenai Heroine 720P MP4 GB (95% of the time)
Saenai Heroine GB MP4 720P (sometimes)
Saenai Heroine GB 720P MP4 (another possible situation I made up)

I used to use .*Saenai.*Heroine.*720P.*MP4.*GB.* to do the match but it obviously fails for the other two cases.

Now I want to ignore the occurring order of the keywords.

After looking at this post here:
[http://stackoverflow.com/questions/4389644/regex-to-match-string-containing-two-names-in-any-order](http://stackoverflow.com/questions/4389644/regex-to-match-string-containing-two-names-in-any-order "Regex to Match String Containing Two Names in any Order")

"lookaheads" (正向零宽断言) seems to work for my case. For example:
<span class="lang:default decode:true crayon-inline">^(?=.*\bjack\b)(?=.*\bjames\b).*$</span> matches a line with jack AND james occurring, in any order.

[Example here](http://www.rubular.com/r/XzVf1NQtjX)