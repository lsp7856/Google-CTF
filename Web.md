#Web

##Wallowing Wallabies - Part One
>Wallowing Wallabies provides enterprise contract management - we'd like to find out how easy it is to perform corporate espionage against them. Visit them here.

>*Please note* Please do not run automated scanners against the target - that's not the intended solution. Instead, perhaps look up "xss cookie catching", "xss cookie stealing" and other documents along those lines. Thanks!

We're brought to a home page.
![](https://github.com/lsp7856/Google-CTF/blob/master/CTF1.png)
Viewing the Source Code of the page could be helpful to start out.
Nothing too interesting.

https://wallowing-wallabies.ctfcompetition.com/robots.txt
brings us to a page of interesting links
>User-agent: *
Disallow: /deep-blue-sea/
Disallow: /deep-blue-sea/team/
//Yes, these are alphabet puns :)
Disallow: /deep-blue-sea/team/characters
Disallow: /deep-blue-sea/team/paragraphs
Disallow: /deep-blue-sea/team/lines
Disallow: /deep-blue-sea/team/runes
Disallow: /deep-blue-sea/team/vendors

We can submit a message on the `Disallow: /deep-blue-sea/team/vendors` page. It is vulnerable to [XSS](http://www.golemtechnologies.com/articles/prevent-xss "XSS") (Cross-Site Scripting)
><em>XSS</em> - enables attackers to inject client-side scripts into web pages viewed by other users

Once a message is submitted we're redirected a page with this text:
>Thank you, your request has been received
Your message to the admins is as follows:
!!! Expecting to find '<script src=.' in your input -- please re-read the level challenge.

This could be a hint... `<script src=.`
An XSS vector could work here to steal the admin's cookie


##Ernst Echidna
>
