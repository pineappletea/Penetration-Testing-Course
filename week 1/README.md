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

## c) Solve Webgoat tasks "HTTP Basics", "Developer tools", "CIA Triad" and "A1 Injection (intro)"

## d) Install Kali Linux into a virtual machine. Try an included tool on a localhost adress.

![kali screenshot](/kali.png)
