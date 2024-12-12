# Sus Emails and How to Extract Them
It's been awhile since I checked my blog; to be honest I am not active to begin with. My blog email is littered with tons of messages like this. They are comments on my blog written in the russian language which I don't understand; I'm only familiar with cyrillic. 

`Author: Robertfaw (IP address: 109.194.243.115, 109x194x243x115.dynamic.irkutsk.ertelecom.ru)
 Email: bikon777@rambler.ru
 URL: http://nashestroitelstvo.ru/
 Comment:
 Вперед к новым приключениям!

 <a href="http://nashestroitelstvo.ru/arenda-katera-sochi-tsena17/post-169.html" rel="nofollow ugc">аренда катера
 евпатория</a>
 - отличный способ провести время с семьей или друзьями. ==>>> Эаказать:
 <a href="http://nashestroitelstvo.ru/arenda-katera-ryazan6/post-202.html" rel="nofollow ugc">аренда яхты катера
 москва метро</a>
 Исследуйте живописные места, устраивайте вечерние посиделки или веселые гонки.
 Начните планировать свой день на воде! Обращайтесь, чтобы забронировать!
 variant4
`

Of course they are just spam mails that are not worth my time. Are they? I notice this as an oppurtunity to practice my commandline skill. Extract Username, IP, Email, and URL.

## Extracting IP
Extracting IP is quite straightforward because it is included on top of the mail's part. I ran `grep -F IP` which revealed the IP address.

Output : 
`Author: Robertfaw (IP address: 109.194.243.115, 109x194x243x115.dynamic.irkutsk.ertelecom.ru)`

But there is one problem, it includes other text. I only want the IP so I ran `grep -F IP | awk '{print $5}'` which prints fifth column of the mail where the IP address resides. 

## Extracting Email
## Extracting Username
### Putting Them Together
## Extracting URL

# Epilogue
