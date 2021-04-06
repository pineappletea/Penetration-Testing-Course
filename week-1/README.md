# Week 1

## a) Get an Invite to HackTheBox

The [Starting page](https://www.hackthebox.eu/invite) gave a tip to check the console.
The console gave the next instructions "This page loads an interesting javascript file. See if you can find it :)".

The debugger displayed the file with the comment "This javascript code looks strange...is it obfuscated???"

Pasting the code into a [deobfuscator](https://lelinhtinh.github.io/de4js/) lead to this readable code

```
function makeInviteCode() {
    $.ajax({
        type: "POST",
        dataType: "json",
        url: '/api/invite/how/to/generate',
        success: function (a) {
            console.log(a)
        },
        error: function (a) {
            console.log(a)
        }
    })
}
```
I used Postman to send the request and received the response

```
{
    "success": 1,
    "data": {
        "data": "SW4gb3JkZXIgdG8gZ2VuZXJhdGUgdGhlIGludml0ZSBjb2RlLCBtYWtlIGEgUE9TVCByZXF1ZXN0IHRvIC9hcGkvaW52aXRlL2dlbmVyYXRl",
        "enctype": "BASE64"
    },
    "hint": "Data is encrypted … We should probably check the encryption type in order to decrypt it…",
    "0": 200
}
```

Decoding with BASE64 resulted in "In order to generate the invite code, make a POST request to /api/invite/generate". I sent the POST request to the new address and received response:

```
{
    "success": 1,
    "data": {
        "code": "<40 character string im leaving out>",
        "format": "encoded"
    },
    "0": 200
}
```

Decoding again with BASE64 gave me the invite code to create an account!

## b) Install Webgoat

Installed into the Kali virtual machine.

![webgoat screenshot](/week-1/webgoat-ss.png)

## c) Solve Webgoat tasks "HTTP Basics", "Developer tools", "CIA Triad" and "A1 Injection (intro)"

WebGoat recommends that it only be used disconnected from the internet. To bring the Kali virtual box offline I used the terminal command `$ nmcli networking off`

### HTTP Basics

This task gave the tip to use OWASP ZAP, which comes installed with Kali. Looking at the tool I ran into its HUD function. Wasn't sure I quite needed it yet but it seemed cool so I completed the tutorial.

![ZAP-HUD](/week-1/ZAP-HUD.png)

I used the history page of the ZAP HUD to look up the post request contents, which gave me the "magic number"...

![zap-post](/week-1/zap-post.png)

... and allowed me to complete the assignment!

![http-completed](/week-1/http-completed.png)

### Developer tools

In this introduction to developer tools completion required me to look up the contents of an HTTP request and fill it in.

![developer-tools-completed](/week-1/devtools-completed.png)

### CIA Triad

This assignment included learning about the CIA Triad model for information security and taking a quiz.

![quiz-done](/week-1/CIA-quiz-done.png)

### A1 Injection (intro)

## d) Install Kali Linux into a virtual machine. Try an included tool on a localhost address.

Installed with Oracle VM virtual box on Windows 10 host.

![kali screenshot](/week-1/kali.png)


## Sources

https://terokarvinen.com/2020/install-webgoat-web-pentest-practice-target/

https://tools.kali.org/web-applications/zaproxy

https://askubuntu.com/questions/434660/how-can-i-disable-my-internet-connection-from-terminal

