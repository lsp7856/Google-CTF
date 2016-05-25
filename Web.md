#Web

##Wallowing Wallabies - Part One
>Wallowing Wallabies provides enterprise contract management - we'd like to find out how easy it is to perform corporate espionage against them. Visit them here.

>*Please note* Please do not run automated scanners against the target - that's not the intended solution. Instead, perhaps look up "xss cookie catching", "xss cookie stealing" and other documents along those lines. Thanks!

We're brought to a home page.
Viewing the Source Code of the page could be helpful to start out.
Nothing too interesting.

https://wallowing-wallabies.ctfcompetition.com/robots.txt
brings us to a page of interesting links
`User-agent: *
Disallow: /deep-blue-sea/
Disallow: /deep-blue-sea/team/
//Yes, these are alphabet puns :)
Disallow: /deep-blue-sea/team/characters
Disallow: /deep-blue-sea/team/paragraphs
Disallow: /deep-blue-sea/team/lines
Disallow: /deep-blue-sea/team/runes
Disallow: /deep-blue-sea/team/vendors`

We can submit a message on this page. It is vulnerable to XSS (Cross-Site Scripting)
><em>XSS</em> - enables attackers to inject client-side scripts into web pages viewed by other users

##Ernst Echidna
>
