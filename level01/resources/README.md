## Level01

Dans le fichier /etc/passwd, on peut observer que le compte "flag01" possède un code spécial dans ses permissions. Ce code, "42hDRfypTqqnw", est en réalité un hash de mot de passe crypté. Cela signifie que le mot de passe réel est dissimulé derrière ce hash.


```bash
cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/bin/sh
bin:x:2:2:bin:/bin:/bin/sh
sys:x:3:3:sys:/dev:/bin/sh
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/bin/sh
man:x:6:12:man:/var/cache/man:/bin/sh
lp:x:7:7:lp:/var/spool/lpd:/bin/sh
mail:x:8:8:mail:/var/mail:/bin/sh
news:x:9:9:news:/var/spool/news:/bin/sh
uucp:x:10:10:uucp:/var/spool/uucp:/bin/sh
proxy:x:13:13:proxy:/bin:/bin/sh
www-data:x:33:33:www-data:/var/www:/bin/sh
backup:x:34:34:backup:/var/backups:/bin/sh
list:x:38:38:Mailing List Manager:/var/list:/bin/sh
irc:x:39:39:ircd:/var/run/ircd:/bin/sh
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/bin/sh
nobody:x:65534:65534:nobody:/nonexistent:/bin/sh
libuuid:x:100:101::/var/lib/libuuid:/bin/sh
syslog:x:101:103::/home/syslog:/bin/false
messagebus:x:102:106::/var/run/dbus:/bin/false
whoopsie:x:103:107::/nonexistent:/bin/false
landscape:x:104:110::/var/lib/landscape:/bin/false
sshd:x:105:65534::/var/run/sshd:/usr/sbin/nologin
level00:x:2000:2000::/home/user/level00:/bin/bash
level01:x:2001:2001::/home/user/level01:/bin/bash
level02:x:2002:2002::/home/user/level02:/bin/bash
level03:x:2003:2003::/home/user/level03:/bin/bash
level04:x:2004:2004::/home/user/level04:/bin/bash
level05:x:2005:2005::/home/user/level05:/bin/bash
level06:x:2006:2006::/home/user/level06:/bin/bash
level07:x:2007:2007::/home/user/level07:/bin/bash
level08:x:2008:2008::/home/user/level08:/bin/bash
level09:x:2009:2009::/home/user/level09:/bin/bash
level10:x:2010:2010::/home/user/level10:/bin/bash
level11:x:2011:2011::/home/user/level11:/bin/bash
level12:x:2012:2012::/home/user/level12:/bin/bash
level13:x:2013:2013::/home/user/level13:/bin/bash
level14:x:2014:2014::/home/user/level14:/bin/bash
flag00:x:3000:3000::/home/flag/flag00:/bin/bash
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
flag02:x:3002:3002::/home/flag/flag02:/bin/bash
flag03:x:3003:3003::/home/flag/flag03:/bin/bash
flag04:x:3004:3004::/home/flag/flag04:/bin/bash
flag05:x:3005:3005::/home/flag/flag05:/bin/bash
flag06:x:3006:3006::/home/flag/flag06:/bin/bash
flag07:x:3007:3007::/home/flag/flag07:/bin/bash
flag08:x:3008:3008::/home/flag/flag08:/bin/bash
flag09:x:3009:3009::/home/flag/flag09:/bin/bash
flag10:x:3010:3010::/home/flag/flag10:/bin/bash
flag11:x:3011:3011::/home/flag/flag11:/bin/bash
flag12:x:3012:3012::/home/flag/flag12:/bin/bash
flag13:x:3013:3013::/home/flag/flag13:/bin/bash
flag14:x:3014:3014::/home/flag/flag14:/bin/bash
```

Le hash de mot de passe est stocké dans le fichier /etc/passwd pour des raisons de sécurité. Il est calculé en utilisant un algorithme de hachage et est généralement irréversible. Cela signifie que vous ne pouvez pas simplement extraire le mot de passe à partir du hash.

C'est là qu'intervient John the Ripper. John the Ripper est un outil de craquage de mot de passe qui peut être utilisé pour tenter de retrouver un mot de passe en essayant différentes combinaisons de caractères et en comparant les résultats au hash stocké. Il est capable de gérer différents algorithmes de hachage, y compris ceux utilisés dans le fichier /etc/passwd.

Dans ce cas, vous pouvez utiliser John the Ripper pour essayer de casser le hash du mot de passe de "flag01" et révéler le mot de passe réel. Une fois que vous avez le mot de passe, vous pourrez vous connecter au compte "flag01" et poursuivre votre progression dans le projet Snow Crash.

Pour récupérer le fichier /etc/passwd via SSH, vous pouvez utiliser
```bash
scp -P 4242 level01@<ip>:/etc/passwd
```

Ensuite, vous pouvez utiliser John the Ripper pour casser le mot de password
```bash
john passwd     
Lun 16 oct 09:51:20 2023
Created directory: /Users/wax/.john
Loaded 1 password hash (descrypt, traditional crypt(3) [DES 128/128 SSE2])
Press 'q' or Ctrl-C to abort, almost any other key for status
abcdefg          (flag01)
1g 0:00:00:00 100% 2/3 100.0g/s 150600p/s 150600c/s 150600C/s raquel..bigman
Use the "--show" option to display all of the cracked passwords reliably
Session completed
```

on trouve abcdefg

Test du flag01
Essayons à nouveau de nous connecter en tant que "flag01" avec le mot de passe déchiffré :
```bash
su flag01
N'oubliez pas de lancer la commande getflag !
```
Exécutez la commande "getflag" pour obtenir le jeton du niveau suivant :
```bash
getflag
Check flag.Here is your token : f2av5il02puano7naaf6adaaf
```

