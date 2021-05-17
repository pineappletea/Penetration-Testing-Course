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


## d) Install Kali Linux into a virtual machine. Try an included tool on a localhost address.

Installed with Oracle VM virtual box on Windows 10 host.

![kali screenshot](/week-1/kali.png)

## Added 17.05.2021

### c) Solve Webgoat: A1 Injection (intro)

After struggling to remember that you must use single quotes with SQL, managed to form the query.

![sql query](/week-1/sql-1.png)

Second page was similar

![sql query](/week-1/sql-2.png)

For the third one were adding a column with:

![sql query](/week-1/sql-3.png)

Fourth part: 

![sql query](/week-1/sql-4.png)

The next few pages teach how to inject queries into a form.

![sql query](/week-1/sql-5.png)

![sql query](/week-1/sql-6.png)

With these queries the trick really seems to be just being very careful with quotations, in a real scenario I expect it would be a lot of trial and error to figure out how exactly the code in the background was written.

![sql query](/week-1/sql-7.png)

So far we've altered values, next up is Query chaining. We end the current query and write a malicious one after it. For the first query we must make sure its valid. For the Authentication TAN field we fill

``` 3SL99A'; UPDATE employees SET salary=3000000 WHERE auth_tan='3SL99A ```

![sql query](/week-1/sql-8.png)

On this last on the instruction is a little bit confusing but the hints help. We're using a field meant for searching access_log and we need to achieve 2 things: delete the table and then make a query to confirm that its deleted. Here its important to use the -- character at the end to comment out what wouldve been the end of the LIKE comparison.

``` ikea"'; DROP TABLE access_log; SELECT * from access_log; -- ```

![sql query](/week-1/sql-9.png)

## Sources

https://terokarvinen.com/2020/install-webgoat-web-pentest-practice-target/

https://tools.kali.org/web-applications/zaproxy

https://askubuntu.com/questions/434660/how-can-i-disable-my-internet-connection-from-terminal

https://www.w3schools.com/sql/sql_update.asp

https://www.w3schools.com/sql/sql_alter.asp


