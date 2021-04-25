# Week 3

On last weeks lecture we had visitor Riku Juurikko giving a presentation on Social Engineering.

## z) Read the material, take notes

### Riku's Slides (not public)

- Hacker Mindset: theres limitless ways to achieve an objective, trying to figure out unusual ways.
- recommended social engineering books by Christopher Hadgany
- mentioned a new trend of hackers selling access credentials online, thought of "not being responsible" when not actually using the credentials for nefarious purposes themselves
- Social engineering is: 
    - influencing/manipulating.
    - most common way to gain access
    - low risk - high reward
- Modern organizations often have very advanced technological defenses, employees end up being the weak point
- Social engineering steps: Recon(passive intelligence gathering), Scan (active), Exploit, Pivot, Covering Tracks
- Overwhelmingly most common motivation of the attacker is money
- Typical information gathered about target(for attack vectors):
    - organization structure
    - partners
    - service providers
    - job applications
    - interesting domains
    - open services
- Intelligence Collection Plan
- intel tools: osintframework, bellingcat, fonecta.fi, finder.fi
- gathering intel isnt just gathering data, also processing it

Principles of persuasion: 
    - reciprocity (be the first to give, the other feels obligated to return the favor)
    - scarcity (limited supply creates sense of urgency)
    - authority
    - commitment and consistency (once people are committed to something, it becomes harder for them to stop. Start small. Sunk cost fallacy)
    - liking (compliments, clean physical appearance, similarity, co-operation. Listening, showing interest, opening up)
    - social proof(others have liked this -> so should you)
    - unity (sense of belonging to a tribe)

Package(the exploit)
- pretexting (For walking in: creating the story of who you are, what you are doing there. Do your homework, look the part)
- phising (making target give information or download a file they shouldnt)
- smshing (spoofing the SMS-sender information to look like fe. microsoft)
- vishing (phonecalls, for example fake it support)

What to do about the social engineering exploits, defense:
- dont get too cynical, people are mostly nice, genuine
- to recognize manipulation: if you feel the need to say "yes" to something above normal - stop, evaluate
- if uncertain, ask for help from others

### [OWASP 10 2017](https://raw.githubusercontent.com/OWASP/Top10/master/2017/OWASP%20Top%2010-2017%20(en).pdf)

## A1 Injection
- works when input data isn't validated, filtered or sanitized
- attacker sends hostile data to the interpreter
- scanners and fuzzers can help attackers find injection flaws
- preventable with code reviews and testing
- results in data leaks, data corruption, access denial. Can lead to complete take over of system
## A2 Broken Authentication
- occurs when attackers is able to force an authentication
- vulnerabilities: brute forcing, weak passwords allowed, improper implementation of session ids
- attackers can use automated password lists and dictionaries
- password rotation is an ineffective defense, users use weak passwords. Instead use 2 factor auth and implement weak password checks

## A3 Sensitive Data Exposure
- a manual attack that steals sensitive information
- data is either unencrypted or the encryption method has a weakness and can be easily broken
- look at what kind of data is being transmitted: is there plain text or poor encryption?

## A7 Cross Site Scripting
- execute HTML and Javascript in the victims browser to gather sensitive information(credit card information, session IDs)
- types: Reflected(code execution in browser, fe. by link containing form), Stored(storing javascript on a site to be ran by visitors), DOM 
- there are automatic attack tools to exploit XSS
- some frameworks prevent this by design: react js, ruby on rails


### [Santos et al 2018: Hacking Web Applications The Art of Hacking Series LiveLessons (video): Security Penetration Testing for Today's DevOps and Cloud Environments: Lesson 5: Authentication and Session Management Vulnerabilities](https://learning.oreilly.com/videos/hacking-web-applications/9780135261422/9780135261422-hwap_01_05_02_00)

#### Understanding Auth schemes
- Authentication and session management as targets
- Authentication: determine that the user is who he claims to be(username + password)
- Session management: remembering how to treat a client, sessionID
- SSL/TLS: confidentiality, mutual authentication. Digital certificates.
    - Certificate is a document containing public key and the provider vouching for content. Managed by Certificate Authorities, signed.
    - 1-way: client reads servers certificate
    - 2-way: server also requests a certificate from client
- HTTP Basic Auth
    - no cookies, headers
    - browser caches credentials and transmitted in clear text
    - client needs to secure the transactions itself
    - plain text transfer is insecure
- HTTP Form-based Auth
    - common
    - specific form where credentials are taken
- DAA Digest Access Authentication
    - only a hash is sent to server
    - not secure, because of collision attacks and MD5 hashing is weak
- LTPA
    - has to use another authentication technique
    - uses a cookie
- OAuth2
    - auth is provided by a third party, there is a redirect URI from auther back to app
    - access tokens in HTTP header
    - user credentials not exposed to app, only the token with an expiry
    - if tokens do not expire, these are a vulnerability
    - uses TLS
- Open ID
    - authentication by provider(google, yahoo)
    - site that uses this method is the "relaying party"
    - Attribute Exchange Extention provides information like Name and gender to relaying party
- SAML
    - similar to Open Id

- OAuth 2 often used with SAML or Open Id

#### Session management mechanisms

- web applications use sessions to keep track of users
- can be used before and after authentication
- sessionID/token exchanged between client and server
- fingerprinting: session ID shows the used framework (fe. JSESSIONID -> java application)
- default fingerprint should be changed to something generic
- session IDs should be long(16 bytes), unpredictable and unique, include good randomness
- session ID used in the URL is unsecure, can be used in session fixation attacks
- frameworks provide session management features that should be used instead of creating your own
- attacker should study how session ids are exchanged
- web applications should use encryption
- many web apps only encrypt a part of the communication
- session cookies vs persistent cookies (expiry time)
- session IDs must be validated
- if ids are not validated, may leave the door open for SQL injection and XSS


### [Percival & Samancioglu 2020: The Complete Ethical Hacking Course (video): Chapter 21: Cross Site Scripting](https://learning.oreilly.com/videos/the-complete-ethical/9781839210495/9781839210495-video21_1)


#### Reflected XSS

- Injecting Javascript into client browser
- use ```<script> </script>``` tags in a form to run javascript
- you can create a link that includes javascript as form input, anyone clicking the link will also execute the code in browser
- proper configuration prevents running the script

#### Stored XSS

- when web app stores user inputs and shows them to other users
- attacker stores script, it gets run by the browser of the victim that visits the site

#### Real hacking with XSS

- Beef tool on Kali
- creates a hook script that allows the attacker to send javascript to the visiting victim
- character limits in forms can be bypassed by editing the page, increasing character limit

#### How to protect yourself

- stay away from untrusted websites
- turn off javascript before visiting an untrusted website
- under content settings in chrome
- you can create block and allow exceptions for javascript in browser settings


## a) Find a case of an intrusion into a company using social engineering. Analyze the case based on Riku's teachings. Were there alternative or better available avenues of attack? How could've the attacks been prevented, detected or their impacts limited?

### [Toyota 2019](https://www.cpomagazine.com/cyber-security/toyota-subsidiary-loses-37-million-due-to-bec-scam/)

This was a BEC(Business email compromise) attack. Hacker posed as a business partner and requested funds to be transferred to his account. Because of the size of the company, the 37 million US wasn't "an outlandishly large figure". Similar attacks had already been attempted against other branches of the company, and so it can be assumed that the companies infrastructure is compromised.

*Riku's teachings. **Pretexting**: The attacker had done his research about the organization and figured out the right people to target with his scam. He had constructed a believable back story for his actions. He was able to weigh the risk vs reward of requesting a large sum, in the context of the business of the company. The emails had also been spoofed as coming from a someone that the target already knew.*

***Authority as principle of persuasion**: While theres no details as to who exactly the attacker was posing as, he probably assumed a position of authority, as in either an important business partner or an executive in the company. Riku also mentioned during his lecture that the power of authority is often tied to culture, and japan has a very authoritative culture. This was the EU subsidiary of the company, so japanese people may not have actually been involved, but it wouldn't surprise me if the company had a very authoritative culture, springing from its roots in japanese culture.*

*The fact that similar attacks had already been detected against other branches of the company also suggests that there was a problem with communication in the company. Perhaps the person that fell for the scam could've been warned about the attacks, but wasn't because someone somewhere determined that "this could never work against us, were smarter than this". Or the targeted person was hesitant, but decided not to ask a superior/college in order to "save face". These are just guesses but spring from the thought of an authoritative corporate culture*

*It seems to me that the best way to prevent something like this is to have a better system around the way the company makes its payments. There should've at least been several sets of eyes to check the transaction before it went through. There could also be a requirement to connect a payment to some kind of change in inventory or another asset, like "this paid for these parts we received".*


## b) Riku's citations. Present 3-5 key lessons from one of Riku's sources.

### [Reuser’s reporterium](http://rr.reuser.biz/)

The Open Source Intelligence resource discovery toolkit

- There is an amazing number of search engines, directories, indexes etc. resources for different purposes
- many search engines take common operators (fe. AND, OR, NOT) which you can use to improve search results
- there are serial search engines that run a search in multiple engines in succession (using several engines is recommended to improve results)
- keep in mind the quality of information

## b2) Webgoat assigments.

### A2 Broken authentication, Authentication bypasses: 2 2FA Password Reset

I filled in answers test and test, then pressed F12 to open the developer tools and looked up the request.

![developer tools](/week-3/wg-a2.png)

I copied the destination url for the request, http://localhost:8080/WebGoat/auth-bypass/verify-account, and the request payload 
secQuestion0=test&secQuestion1=test&jsEnabled=1&verifyMethod=SEC_QUESTIONS&userId=12309746.

Tried to send the request with curl ``` curl -X POST http://localhost:8080/WebGoat/auth-bypass/verify-account -d "jsEnabled=1&verifyMethod=SEC_QUESTIONS&userId=12309746" -v ```

but got connection refused. Makes sense since we had to log in to webgoat to start the excercise. Tried adding the cookie with ``` -cookie "JSESSIONID: cGt3rr_J1Ph8JbWN0G_XE2RiNZSgeD6lElq7ExI7"``` Still connection refused.

Figured out that you can edit and resend the request in the developer tools by right clicking on it. Tried sending the the request without mention of the security questions, so as ``` jsEnabled=1&verifyMethod=SEC_QUESTIONS&userId=12309746 ```. I get response 400 "bad request" with message: "Required String parameter 'userId' is not present".

Tried without the verifyMethod, response 400: "Required String parameter 'verifyMethod' is not present". So it looks like we do have to include the security questions. The questions are numbered as "secQuestion0" and "secQuestion1". I wonder if the numbers can just be changed, and we would trick the application to look for entries in a database that have no answer, returning an empty string, comparing it to my empty and thinking its correct. So I edit the request payload to ``` secQuestion10=&secQuestion11=&jsEnabled=1&verifyMethod=SEC_QUESTIONS&userId=12309746 ```

Success!

![developer tools](/week-3/wg-a2-done.png)



### A2 Broken authentication, Secure Passwords: 4 How long could it take to brute force your password?

The NIST password standard is a guideline for good password practices. Among them one jumps out at me. "No composition rules -
Do not request the user to e.g. use at least one upper case letter and a special character on their password. Give them the opportunity to, but do not force them!". I still see this all the time when creating accounts.

I use the tool on page 4 to evaluate some of the old passwords I used to use and know to be weak, and they all fail as expected.
I try crafting a memorable but strong password (so not use a random symbols or a password manager/generator) and come up with "1Goat4StrengthTra1n1ng"

![password success](/week-3/pw-done.png)

### A3 Sensitive data exposure, Insecure Login: 2 Let's try

I again look at the networking tab of Developer tools after clicking the "Log in" button on the second page. Among the POST-request I find this. 

![plaintext password in request](/week-3/insecure-pw.png)

I fill them in, and it works!

### A7 Cross Site Scripting (XSS): Cross site scripting, 2 What is XSS?

The instructions are to type ``` javascript: alert(document.cookie); ``` into the address bar, but nothing happens. I imagine this is some firefox safety feature, so I open up the console and type that in. 

![pop-up of cookie](/week-3/cookie-popup.png)

I repeat this for another window, the cookie is the same, so I input "yes".

### A7 Cross Site Scripting (XSS): Cross site scripting, 7 Try It! Reflected XSS

Into the console I type ``` console.log(document) ``` and start scrolling through it.
At the bottom theres xss-5a. 

![xss-5a](/week-3/xss-1.png)

After some trial and error and looking up, i figure out how to refer to a specific value: ``` console.log(document.forms["xss-5a"][5].value) ``` prints the credit card number, and with 6 instead of 5 it prints the access code.

![print of fields](/week-3/xss-fields.png)

There isn't really a field to input the answers to this, and it isn't made very clear what is required to pass this. But the tracker box has turned green so Im calling it done.

## Sources

https://terokarvinen.com/2021/hakkerointi-kurssi-tunkeutumistestaus-ict4tn027-3005/

https://learning.oreilly.com/videos/hacking-web-applications/9780135261422/9780135261422-hwap_01_05_00_00

https://raw.githubusercontent.com/OWASP/Top10/master/2017/OWASP%20Top%2010-2017%20(en).pdf

https://learning.oreilly.com/videos/the-complete-ethical/9781839210495/9781839210495-video21_1

https://linuxize.com/post/curl-post-request/

https://gist.github.com/subfuzion/08c5d85437d5d4f00e58

https://developer.mozilla.org/en-US/docs/Web/API/Document/forms

https://www.cpomagazine.com/cyber-security/toyota-subsidiary-loses-37-million-due-to-bec-scam/

https://en.wikipedia.org/wiki/Business_email_compromise
