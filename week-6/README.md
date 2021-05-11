# Week 6: Hashing

## z) Watch [Santos et al 2017: Security Penetration Testing - The Art of Hacking Series LiveLessons: Lesson 6: Hacking User Credentials](!https://learning.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00). Take short notes.

- passwords in databases or text files, SQL for most webapps
- mimikatz dumps credentials from memory
- protecting passwords: encrypting data on the network, storing safely, intrusion detection. Long passwords,two factor auth
- you can find default passwords online for many apps and devices, people often don't change em
- sniffing and man-in-the-middle attacks
- harvesting credentials from network devices
- bruteforcing, dictionaries
- enumeration of users
- hashing: one way transformation, used to verify and validate
- cracking passwords has improved a lot. use of GPUs, dictionaries, quality of algorithms
- johntheripper on CPU, hashcat on GPU
- mangling rules for dictionaries(1337speak etc)
- LM and LMNT passwords on windows
- LM passwords having 2 separate sides make them easier to crack

## b) Make 3 hashes with different programs and break them using hashcat.

Using linux tools to create the hashes and breaking them with hashcat on my host OS Windows 10, to utilize graphics card.

### MD5

``` echo -n olli123|md5sum``` generates hash 3da45d38156340a33a6b049fdb2f008d

The flag -n is used to exlude the newline character from the input to the hashing algorithm.

Now to break it, I first save the hash as hast.txt in my hashcat folder, then run with 

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

Using OWASP: Juice Shop, a vulnerable webapp practice target.

First installed node and downloaded the git repository for juice shop. 

``` git clone https://github.com/bkimminich/juice-shop.git ```

``` npm install ```

Installing isn't working, so to try with a preinstalled version 

``` wget https://github.com/bkimminich/juice-shop/releases/download/v12.7.2/juice-shop-12.7.2_node12_linux_x64.tgz ```

``` tar zxvf juice-shop-12.7.2_node12_linux_x64.tgz ```

```cd juice-shop_12.7.2 ```

Disconnected the box from NAT.

``` npm start ```

![juiceshop is running](/week-6/juiceshop-running.png)


Ok the site is running, now to create the wordlist im using CeWL, a tool on Kali that crawls the website and gathers a wordlist.

``` cewl -d 3 -m 4 -w juicewordlist.txt http://localhost:3000/ ```

![juiceshop is running](/week-6/juiceshop-wordlist.png)

Looking at this short wordlist, CeWL has only picked up the contents of the HTML file, and hasn't looked at the javascript which makes up the bulk of the site. 

## d) Try a dictionary attack on a form on your own website.


## e) Break the password protection of a file

Made directory and a file inside it. 

![create directory](/week-6/mkdir.png)

Then zipped and password protected it with ``` zip -e protected.zip compressthis/ ``` and put "olli12" as the password.

To extract the hash ``` zip2john protected.zip > hash.tmp ``` and to remove extra information ``` cat hash.tmp | grep -E -o '(\$pkzip2\$.*\$/pkzip2\$)|(\$zip2\$.*\$/zip2\$)' > zip.hash ```

so what we have left is $pkzip2$1*2*2*0*18*c*af083b2d*47*4f*0*18*af08*0826*31ca663358e325e2a9f78ba1733d511f090b247b5d40b59b*$/pkzip2$

Moved this into the host OS windows side for hashcat. 

Looking at the options for hashcat theres several has types for PKZIP, not entirely sure which one this is. 

![pkzip](/week-6/md5-cracked.png)

Going to try the 17210 | PKZIP (Uncompressed), since it shouldn't be compressed. 

``` .\hashcat.exe -m 17210 -a 3 -o cracked.txt hash.txt ```

Worked!

![zip cracked](/week-6/hashcat-zip.png)


## Sources

https://terokarvinen.com/2021/hakkerointi-kurssi-tunkeutumistestaus-ict4tn027-3005/

https://learning.oreilly.com/videos/security-penetration-testing/9780134833989/9780134833989-sptt_00_06_00_00

https://miloserdov.org/?p=5426#11

https://github.com/bkimminich/juice-shop#from-sources

https://www.cyberciti.biz/faq/unpack-tgz-linux-command-line/

https://tools.kali.org/password-attacks/cewl


