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
>Disallow: /deep-blue-sea/
>Disallow: /deep-blue-sea/team/
>//Yes, these are alphabet puns :)
>Disallow: /deep-blue-sea/team/characters
>Disallow: /deep-blue-sea/team/paragraphs
>Disallow: /deep-blue-sea/team/lines
>Disallow: /deep-blue-sea/team/runes
>Disallow: /deep-blue-sea/team/vendors

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

Let's test it out with script in the justification box:

>script???

To perform espionage against this site, which is our main goal, it would be pretty destructive if we were able to get a hold of the admin's session data from the cookie then try to use the session to impersonate and login as the admin


##✓ Ernst Echidna
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
md5-hash is the name of our cookie

There's a hashed form of md5-hash given to us too under Content

We can try to crack this md5 hash, [md5cracker.org](http://md5cracker.org) works fine

Let's try to trick the webpage into thinking we are the administrator.

We can create our own md5hash of the username: admin

[This site works](http://md5cracker.org/create-md5)
Edit our current cookie for the username we registered and change it to our new hashed form of admin.
To do this we need to download a quick Cookie editor extension. I used EditThisCookie which is a free Chrome extension. We can remove it when we are done with this CTF challenge, or not. Then, change the value of our cookie to the hashed admin value.

Now when we go to /admin the webpage says:
>The authentication server says..
>Congratulations, your token is 'CTF{renaming-a-bunch-of-levels-sure-is-annoying}'

##Dancing Dingoes
>We're interested in finding out what information is stored on [this](https://dancing-dingoes.ctfcompetition.com/) website. We've already obtained the username "proff" and the password "strobe.c", but can't work out how to access the "admin" user. Any ideas?

We're given a basic login screen to begin with. We can always start by looking at the source code.
A css Style Sheet is accessible, might not be too useful, but its there.

Even if we try to visit other webpages on the site (robots.txt) we are blocked by the same username/password login screen.

Let's use the given username and password to login and look around.
>username: proff
>password: strobe.c

Documents Available

Below is the documents available to you!
* International Subversives
* Worms Against Nuclear Killers
* Underground

Once we're logged in, we can see the site stores two cookies during one session
1. APISESSIONID
2. SSOSESSIONID

The hashed APISESSIONID: dHlwZT1wcm9mZiZpZHg9MSZjPTU2JnVpZD04OTVjdHg9MjE3NyZoYXNoPTM1NmY3ODk5Y2M1MmRmNjAxNjY3YzdiOTUxNzE2ZDRjMGE3MzhmNjEwYTRlMzQxNWEyYzdlZDU3ZmMwNGUyNjQ=

We can try to manipulate this to become the hashed admin value again like we did in a previous challenge
