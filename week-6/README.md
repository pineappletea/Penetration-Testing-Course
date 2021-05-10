# Week 6: Hashing

## z) Watch [Santos et al 2017: Security Penetration Testing - The Art of Hacking Series LiveLessons: Lesson 6: Hacking User Credentials](!https://learning.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00). Take short notes.

## b) Make 3 hashes with different programs and break them using hashcat.

Using linux tools to create the hashes and breaking them with hashcat on my host OS Windows 10, to utilize graphics card.

### MD5

``` echo -n olli123|md5sum``` generates hash 3da45d38156340a33a6b049fdb2f008d

The flag -n is used to exlude the newline character from the input to the hashing algorithm.

Now to break with I first save the hash as hast.txt in my hashcat folder, then run with 

``` .\hashcat.exe -m 0 -a 3 -o cracked.txt hash.txt ```

![md5 cracked](/week-6/md5-cracked.png)

### SHA1

``` echo -n olli123|sha1sum``` to create hash 6cafe4b90d9b471ff43670c0f68de5473360dcc9

and to crack ``` .\hashcat.exe -m 100 -a 3 -o cracked.txt hash.txt ```

![sha1 cracked](/week-6/sha1-cracked.png)

### SHA256

``` echo -n olli1|sha256sum``` generates hash 936b3750747167031cb6023f747dd169b490fe761212e8fb9c9122e769ea2c6b

and after transfering it to hash.txt on the host OS, cracking with

``` .\hashcat.exe -m 1400 -a 3 -o cracked.txt hash.txt ```

![sha256 cracked](/week-6/sha256-cracked.png)

And finally heres the results hashcat printed into cracked.txt 

![cracked.txt](/week-6/cracked-txt.png)

## c) Make a wordlist for password cracking from a website you made.

## d) Try a dictionary attack on a form on your own website.

## e) Break the password protection of a file

Made directory and a file inside it. 

![create directory](/week-6/mkdir.png)

Then zipped and password protected it with ``` zip -e protected.zip compressthis/ ``` and put "olli12" as the password. 



## Sources

https://terokarvinen.com/2021/hakkerointi-kurssi-tunkeutumistestaus-ict4tn027-3005/

https://learning.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00
