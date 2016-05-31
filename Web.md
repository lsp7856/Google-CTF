#Web

##✓ Wallowing Wallabies - Part One
>Wallowing Wallabies provides enterprise contract management - we'd like to find out how easy it is to perform corporate espionage against them. Visit them here.

>*Please note* Please do not run automated scanners against the target - that's not the intended solution. Instead, perhaps look up "xss cookie catching", "xss cookie stealing" and other documents along those lines. Thanks!

We're brought to a home page.
![](https://github.com/lsp7856/Google-CTF/blob/master/CTF1.png)
Viewing the Source Code of the page could be helpful to start out.
Nothing too interesting.

https://wallowing-wallabies.ctfcompetition.com/robots.txt brings us to a page of interesting links

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
><em>XSS</em> enables attackers to inject client-side scripts into web pages viewed by other users

Once a message is submitted we're redirected to a page with this text:
>Thank you, your request has been received

>Your message to the admins is as follows:

>!!! Expecting to find '<script src=.' in your input -- please re-read the level challenge.

This could be a hint... `<script src=.`

An XSS vector could work here to steal a user's session cookie, such as the admin's

The best way to perform an XSS attack here is to trick the page into outputting data we enter (using JavaScript to get the admin's cookie)

The webpage displays an error message when we don't enter information that meets its rules

The error message contains exact text we enter which is what generally leads to XSS in any situation

Let's test it out with script in the justification box. We know the script has to start with `<script src= `.

Google Chrome comes with a Developer Tool called Javascript Console that can be used to figure out what script got through to the site.

To figure out what got through the filter, we can use [this site's xss script](http://www.smeegesec.com/2012/06/collection-of-cross-site-scripting-xss.html) as test script then look for lines where Chrome's built-in XSS audtiory blocked the execution.

For a better understanding of how XSS works, [this site](http://www.go4expert.com/articles/stealing-cookie-xss-t17066/) was also helpful and included PHP script to steal the admin's cookie.

A free-hosting web server would be necessary in this situation to upload the cookier stealer PHP file and log file to.

The payload in the script gets sent to the admin as a message. The stolen cookie will be saved on the hosting web server using the PHP script.
In a few minutes, the log on the web server will contain the stolen admin cookie after the site's admin 'reads' the message.

>`green-mountains=eyJub25jZSI6ImUxNjgwMjcyYTcxNDE3MjMiLCJhbGxvd2VkIjoiXi9kZWVwLWJsdWUtc2VhL3RlYW0vdmVuZG9ycy4qJCIsImV4cGlyeSI6MTQ2MjAzMTg2OH0=|1462031865|d985a99f12846cd73da3b9b01b3b921fd15512e3`

Viewing the site's cookies after using the script, we see our cookie has the same value of the green-mountains one.

Refresh the page with the stolen cookie.

The site is redirected to the flag.

>part 1: CTF{feeling_robbed_of_your_cookies}

>Vendor management roles available:

>All vendors have been deprecated!


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

We can try to manipulate this hashed APISESSIONID cookie to become the hashed admin value again like we did in a previous challenge.

When we check to see what requests are actually sent to the site when logging in, 



The site is working with a Plain-text Protocol which can be messed up with binary.
If the URL had binary in it, the protocol could be messed up if a linebreak was used for instance.
> 0x0A (\n)

Base64 represents binary data in ascii. We can encode binary and transmit it over something expecting ascii.

It will be decoded on the other end to get it back to the raw binary

You can encode binary, transmit it over something expecting ascii, and the other end can decode it to get it back to the raw binary

Base64 is alphanumeric - it has 'padding' at the end, somewhere between 0-3 '='

We can decode the hashed APISESSIONID using a [Base64 decoder/encoder](https://www.base64decode.org/) (or we could write the program in Python)

Decoding it to UTF-8 we get:

>type=proff&idx=1&c=56&uid=895ctx=2177&hash=356f7899cc52df601667c7b951716d4c0a738f610a4e3415a2c7ed57fc04e264

To display it all more clearly, we have:
* type=proff
* idx=1
* c=56
* uid=895ctx=2177
* hash=356f7899cc52df601667c7b951716d4c0a738f610a4e3415a2c7ed57fc04e264

SSOSESSIONID
* type=session&idx=6&c=136&uid=350ctx=2213&hash=d2260600d4076a31fdd7514b08706741b7cc054faebec4cc3cbb930c9381d238
* type=session
* idx=6
* c=136
* uid=350ctx=2213
* hash=d2260600d4076a31fdd7514b08706741b7cc054faebec4cc3cbb930c9381d238

Another thing to note, when logging in, we can see the site is communicating with the API

>/api/users/login?domain=

##✓ Spotted Quoll
>[This](https://spotted-quoll.ctfcompetition.com/) blog on Zombie research looks like it might be interesting - can you break into the /admin section?

We're brought to a blog page "My Zombie Research Project"

A body of text says "I don't have much content yet, except for my admin page..."

Clicking the Admin button at the top of the page redirects us nowhere but the url says "#err_user_not_found"

Simply going to `/admin` does not work either.

Looking at the page's source code, we see there is a page `/getCookie`
>`<iframe style="border: 0;" src="/getCookie"></iframe>`

When going to this page though, it is blank. Even its source code is empty. It should be noted though as it might come in use later.

If we look at the cookies being stored on this blog's homepage we see that there is one titled `obsoletePickle`
>`Content: KGRwMQpTJ3B5dGhvbicKcDIKUydwaWNrbGVzJwpwMwpzUydzdWJ0bGUnCnA0ClMnaGludCcKcDUKc1MndXNlcicKcDYKTnMu`

When we decode this hashed value using an online decoder such as [this one](https://www.base64decode.org/) we get this:
>(dp1

>S'python'

>p2

>S'pickles'

>p3

>sS'subtle'

>p4

>S'hint'

>p5

>sS'user'

>p6

>Ns.


So we're dealing with Python pickling here, which is essentially the process where a Python object hierarchy is converted into a byte/character stream. Its used for serializing/deserializing a Python object structure. The object is serialized first before being written to a file. For more info on pickling: [this link](https://pythontips.com/2013/08/02/what-is-pickle-in-python/) and [this link](https://docs.python.org/2/library/pickle.html#)

What I did was visit the `/admin` webpage again with BurpSuite open this time.

Under Target > Site map looking at the GET request to /admin displays the hashed cookie value named obsoletePickle again.

BurpSuite also has a built-in Decoder option that gets us the same value as using the web decoder.

The next step was to decode the String in Python.

Running Python in the terminal, decoding the String told us the user was set to 'None'
> `$ python`

> `>>> import pickle`

> `>>> import base64`

> `>>> pickle.loads('KGRwMQpTJ3B5dGhvbicKcDIKUydwaWNrbGVzJwpwMwpzUydzdWJ0bGUnCnA0ClMnaGludCcKcDUKc1MndXNlcicKcDYKTnMu'.decode('base64'))`

> `{'python': 'pickles', 'subtle': 'hint', 'user': None}`

If the user were to be set to 'admin' we could submit the modified cookie to the website. To do this we need to call the pickler's `dumps()` method and pass through the params we got but set the user to 'admin' like so:

>`pickle.dumps({'python': 'pickles', 'subtle': 'hint', 'user': 'admin'})`

which returns the String:
> `"(dp0\nS'python'\np1\nS'pickles'\np2\nsS'subtle'\np3\nS'hint'\np4\nsS'user'\np5\nS'admin'\np6\ns."`

We can assign the String to the variable `encoded`
> `>>>> encoded=_`

Then encode the modified String back to base64
> `>>> base64.b64encode(encoded)`

Lastly, modify the obsoletePickle cookie on the site to this hashed value then load the `/admin` page.
> `'KGRwMApTJ3B5dGhvbicKcDEKUydwaWNrbGVzJwpwMgpzUydzdWJ0bGUnCnAzClMnaGludCcKcDQKc1MndXNlcicKcDUKUydhZG1pbicKcDYKcy4='`

>Your flag is CTF{but_wait,theres_more.if_you_call} ... but is there more(1)? or less(1)?

##In Recorded Conversation
>Can you find the flag?

We are given a file to download called irc.pcap
