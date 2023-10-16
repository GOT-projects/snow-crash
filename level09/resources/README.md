# Level09

 implique un binaire nommé level09 et un fichier nommé token. L'objectif est d'exploiter level09 pour décoder le contenu du fichier token et obtenir le jeton du niveau suivant.


1) Tout d'abord, en listant les fichiers, vous pouvez voir que level09 a le bit SUID activé, ce qui signifie qu'il s'exécutera avec les droits de l'utilisateur propriétaire, "flag09" dans ce cas. Le contenu du fichier "token" est également affiché.

```bash
ls -l
total 12
-rwsr-sr-x 1 flag09 level09 7640 Mar  5  2016 level09
----r--r-- 1 flag09 level09   26 Mar  5  2016 token
```

```bash
cat token
f4kmm6p|=pnDBDu{}
```

2) Lorsque vous exécutez level09 sans argument ou avec un nombre incorrect d'arguments, il affiche un message indiquant qu'un seul argument doit être fourni.
```bash
./level09
You need to provied only one arg.
```

```bash
./level09 1234567890
13579;=?A9
```
3) Si vous exécutez level09 avec un argument composé de chiffres (par exemple, "1234567890"), il renverra une chaîne de caractères transformée, où chaque caractère a été décalé de sa position dans la chaîne d'origine (décalage alphabétique).


```bash
./level09 aaaaa
abcde
```

4) Vous pouvez déterminer le décalage alphabétique en consultant la table des caractères ASCII pour les lettres minuscules.

```bash
Position  | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
          ---------------------------------
Char      | a | b | c | d | e | f | g | h |
          ---------------------------------
Char code |97 |98 |99 |100|101|102|103|104|

    Befor   |  After
---------------------
( 97) a + 0 : a ( 97)
( 98) b + 1 : c ( 99)
( 99) c + 2 : e (101)
(100) d + 3 : g (103)
(101) e + 4 : i (105)
(102) f + 5 : k (107)
(103) g + 6 : m (109)
(104) h + 7 : o (111)

```
5) Pour décoder le contenu du fichier "token", vous pouvez créer un script Python qui prend en charge le décalage et affiche la chaîne décodée. Le script lit chaque caractère de la chaîne d'origine, soustrait son index dans la chaîne (0 pour le premier caractère, 1 pour le deuxième, etc.) et affiche le caractère décodé.

create script dans /tmp
```python
import sys

for i, val in enumerate(sys.argv[1]):
	sys.stdout.write(chr(ord(val)-i))
	sys.stdout.flush()
print
```

Ensuite, vous pouvez exécuter le script Python en lui passant le contenu du fichier "token" comme argument.
```bash
python /tmp/decode.py `cat token`
f3iji1ju5yuevaus41q1afiuq
```

Une fois que vous avez obtenu le contenu du fichier "token", vous pouvez l'utiliser pour accéder au compte "flag09" et récupérer le jeton du niveau suivant. Vous pouvez le faire en lançant une session en tant que "flag09" :
```bash
su flag09
```

Enfin, exécutez la commande "getflag" pour obtenir le jeton du niveau suivant :
```bash
getflag
Check flag.Here is your token : s5cAJpM8ev6XHw998pRWG728z
```
