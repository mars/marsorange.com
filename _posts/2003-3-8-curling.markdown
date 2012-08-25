---
layout: post
title: !binary |-
  Y1VSTGluZw==
---
<p>So, my first step to making my MT entries postable by eMail is now done. I figured out the simple <a href="http://curl.haxx.se/">cURL</a> command to post a new MT entry without a Web browser.</p>
<!--more-->
<p>I will plug this cURL command into the PHP script I wrote last year (to post eMail messages using a primative XHTML-only blogging system):</p>

<blockquote><code><strong>TheShell &gt;&gt;&gt;</strong>  curl \<br /> 
--cookie "user=<em>[TheUserCookieValue]</em>" \<br /> 
--data "author_id=1& \<br />
blog_id=1& \<br >
__mode=save_entry& \<br />
title=<em>[TheEntryTitle]</em>& \<br />
category_id=1& \<br />
text=<em>[TheEntryText]</em>& \<br />
text_more=<em>[TheExtendedText]</em>& \<br />
excerpt=<em>[TheExerpText]</em>& \<br />
status=1& \<br />
allow_comments=0& \<br />
allow_pings=0& \<br />
convert_breaks=0& \<br />
to_ping_urls=" \<br /> 
http://www.theDomain.com/path/to/mt.cgi</code></blockquote>

<p>Soon, this PHP script will be doing a rudimentary (as far at MT features go) inject to MT! Yeah!!!</p>
