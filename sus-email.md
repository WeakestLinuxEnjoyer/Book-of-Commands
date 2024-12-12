# Sus Emails and How to Extract Them
It's been awhile since I checked my blog; to be honest I am not active to begin with. My blog email is littered with tons of messages like these  : 

`Author: Robertfaw (IP address: 109.194.243.115, 109x194x243x115.dynamic.irkutsk.ertelecom.ru)
Email: bikon777@rambler.ru
URL: http://nashestroitelstvo.ru/
Comment:Вперед к новым приключениям! <a href="http://nashestroitelstvo.ru/arenda-katera-sochi-tsena17/post-169.html" rel="nofollow ugc">аренда катера
 евпатория</a> - отличный способ провести время с семьей или друзьями. ==>>> Эаказать:
 <a href="http://nashestroitelstvo.ru/arenda-katera-ryazan6/post-202.html" rel="nofollow ugc">аренда яхты катера
 москва метро</a>
 Исследуйте живописные места, устраивайте вечерние посиделки или веселые гонки.
 Начните планировать свой день на воде! Обращайтесь, чтобы забронировать!
 variant4
`

I can read in russian but only understand few words from using Yandex, `новым = new`, `яхты = yacht`, `москва метро = Moscow Metro`, it's not enough to decipher this email myself. 

Of course they are just spam mails that are not worth my time. Are they? I notice this as an oppurtunity to practice my commandline skill. Extract Username, IP, Email, and URL.

## Disclaimer
IP, Email, and Username are sensitive data that can reveal a lot more private information. You should ***NOT*** reveal them in most situation. However, I decided to reveal them because they are spam emails sent by bot via disposable email, so the chances of being traced back is near impossible; they are likely criminals if anything. 

## Converting Email(s)
These emails are sent to my Gmail address, which unfortunately isn't so straightforward to download. I had to manually click `print to pdf` on the client, then I convert those .pdf files to text with `pdftotext`. There were 10 mails total. 

## Extracting IP
Extracting IP is quite straightforward because it is included on top of the mail's part. I ran `grep -F IP` which revealed the IP address.

Output : 

`Author: Robertfaw (IP address: 109.194.243.115, 109x194x243x115.dynamic.irkutsk.ertelecom.ru)`

But there is one problem, it includes other text. I only want the IP so I ran `grep -F IP | awk '{print $5}'` which prints fifth column of the mail where the IP address resides. 

Output : 

`109.194.243.115,`

Oof I took the `,`. I could delete this using `tr -d ","` but that's ok. 

## Extracting Email
Email is on the header alone so we can simply extract it using `grep -F Email`

Output : 

`Email: bikon777@rambler.ru`

## Extracting Username
Username was located in the first header alongside IP as `Author`. I rerun the command for IP `grep -F IP`, then filter it to the second column using AWK `grep -F IP | awk '{print $3}'` 

Output : 

`Robertfaw`

### Extract More
I have been playing with only one mail, but there are nine more to extract. How do I extract them all? At first I thought of using `while` loops, but I remember `xargs`. All I did was add `ls | xargs (original command)`, for example like this : 

`ls | xargs grep -F IP sus-email1.txt | awk '{print $3}`

This gives me output of each username, and since I want to save it, I pipe it to a file using `>`, then name it as `username`. I did this procedure email and IP too. 

But what if I want to line them up together (Username, Email, IP)? This is where `paste` comes handy. You can simply line the files like this `paste username email IP`

Output : 

`Sazruff kdqsopywemn@rudiplomust.com 94.103.88.177 
Robertfaw bikon777@rambler.ru 109.194.243.115 
Oariorjcj ddzxknngfmn@rudiplomust.com 93.183.93.32 
Sazrqaf nhuqelldmmn@rudiplomust.com 178.20.46.206 
Trefvlu ynuehsncfmn@rudiplomust.com 195.2.75.64 
Mazrxqb pownivhfbmn@rudiplomust.com 93.183.94.206 
snyatie yksecjmucOr@myagkie-paneli11.ru domy_fmOr
snyatie fzqyulcmtKa@myagkie-paneli11.ru domy_fsKa
snyatie ohakbrfqgkl@myagkie-paneli11.ru domy_ibkl
Lazribx kyhenqjjjmn@rudiplomust.com 93.183.94.206 
`
As you can see here, I missed the IP address for row 7-9. Perhaps regex would do the job better, but I haven't mastered it well enough. 

## Extracting URL
URL is trickier, because it is embedded in the text so simple `grep -F` and 
`awk '{print $x}'` won't help. I decided to look up how to extract website from text using regex from [this article](https://www.baeldung.com/linux/shell-get-url-from-string) . 

`grep -o 'http[s]\?://[^ ]\+'` works well, perhaps too well because it also brings button interaction links like this : 

`http://nashestroitelstvo.ru/arenda-katera-ryazan6/post-202.html"
https://linuxenjoyer.com/wp-admin/comment.php?action=approve&c=3346#wpbody-content
`
Then I noticed one thing, the link is preceded by `href` tag, so I can just `grep -F href`. Here's the output : 
`<a href="http://nashestroitelstvo.ru/arenda-katera-sochi-tsena17/post-169.html" rel="nofollow ugc">аренда катера`

Looks good, but not quite. I just want the link. To do so, I replaced the `=` with `space` using `tr "=" " "`. Then I ran `awk '{print $3}'` to get the url.

Final command : 
`grep -F href sus-email1.txt | tr "=" " " | awk '{print $3}'`

Output
`"http://nashestroitelstvo.ru/arenda-katera-sochi-tsena17/post-169.html"`

Then I ran it to the rest of the files using `ls | xargs (command)` and pipe it to a file. 

## Epilogue
I'd like to find out where will those data lead me to, but it won't fit into this repository as I will be using web browser. That'll be on my blog. 
Since there are thousands more, I'd like to develop a script to extract them more efficiently. 
