
# Level 00

### Connexion SSH au niveau 00
Pour accéder au niveau 00, utilisez la connexion SSH avec les informations suivantes :
```bash
ssh level00@<ip> -p 4242
passwd: level00
```

### Première approche
Commencez par rechercher les fichiers binaires dans le système :
```bash
ls -la /usr/*/*
```

Pour affiner la recherche en se concentrant sur le groupe "level00" :
```bash
ls -la /usr/*/* | grep level00
```

Aucun résultat n'est obtenu en utilisant "level00", essayons avec "flag00" :
```bash
ls -la /usr/*/* | grep flag00

level00@SnowCrash:~$ ls -la /usr/*/* | grep flag00
----r--r--    1 flag00  flag00        15 Mar  5  2016 /usr/sbin/john
cat /usr/sbin/john
cdiiddwpgswtgt 
```

Alternative :

```bash
find / -user flag00 -group flag00 2>/dev/null
```

### Test du flag00
Tentons de nous connecter en tant que "flag00" :
```bash
su flag00
su: Authentication failure
```

## Chiffre de César
Pour résoudre ce niveau, nous allons utiliser le chiffre de César. Vous pouvez trouver des outils en ligne pour cela, comme 
https://www.xarg.org/tools/caesar-cipher/

### Test des différentes clés de 1 à 13
Testez les différentes clés possibles, de 1 à 13, pour le chiffre de César. La clé 11 semble être la bonne :

```code
nottoohardhere
```

Test du flag00
Essayons à nouveau de nous connecter en tant que "flag00" avec le mot de passe déchiffré :
```bash
su flag00
Don't forget to launch getflag !
```

Exécutez la commande "getflag" pour obtenir le jeton du niveau suivant :
```bash
getflag
Check flag.Here is your token : x24ti5gi3x0ol2eh4esiuxias
```
Le niveau 00 est terminé avec succès.
