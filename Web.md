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
Robots.txt is a file sites usually have to give instructions to web robots
Disallows web robots from visiting certain pages of the site
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
An XSS vector could work here to steal a user's session cookie, such as the admin
The best way to perform an XSS attack here is to trick the page into outputting data we enter
The webpage displays an error message when we don't enter information that meets its rules
The error message contains exact text we enter which is what generally leads to XSS in any situation

To perform espionage against this site, which is our main goal, it would be pretty destructive if we were able to get a hold of the admin's session data from the cookie then try to use the session to impersonate and login as the admin


##Ernst Echidna
>Can you hack [this](http://ernst-echidna.ctfcompetition.com "this") website? The robots.txt sure looks interesting.

Let's begin by taking up the hint and taking a look at the robots.txt page
>`Disallow: /admin`
Let's check out the admin page since it seems to be the only one the site doesn't want robots to see
>You are not logged in!
True...
Our best option is to try registering for an account back on the home page.
We can take a look at the source code, but there's not much there to see. Just a standard username/password screen, nothing hidden in the HTML.
After registering, a page appears that says:
>Welcome
>Thank you for registering!
>Unfortunately, there has been no content posted on this site
What if we revisit the admin page?
>Sorry, this interface is restricted to administrators only
Revisit the home page...The site is keeping track that we are now a registered member. This is a sign that it is holding our cookie. There is a possibility that the site also holds the admin's cookie too.
Go to the Pad-Lock in the URL bar, and view the cookies for this site.
![](https://github.com/lsp7856/Google-CTF/blob/master/Screen%20Shot%202016-05-25%20at%201.45.50%20PM.png?raw=true)
