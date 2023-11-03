```bash
level13@SnowCrash:~$ ll
[...]
-rwsr-sr-x 1 flag13  level13 7303 Aug 30  2015 level13*
[...]

level13@SnowCrash:~$ ./level13 
UID 2013 started us but we we expect 4242

level13@SnowCrash:~$ objdump -M intel -D level13 | less
```

Le binaire level13 en asm (main)

```nasm
0804858c <main>:
 804858c:       55                      push   ebp
 804858d:       89 e5                   mov    ebp,esp
 804858f:       83 e4 f0                and    esp,0xfffffff0
 8048592:       83 ec 10                sub    esp,0x10
 8048595:       e8 e6 fd ff ff          call   8048380 <getuid@plt>
 804859a:       3d 92 10 00 00          cmp    eax,0x1092
 804859f:       74 2a                   je     80485cb <main+0x3f>
 80485a1:       e8 da fd ff ff          call   8048380 <getuid@plt>
 80485a6:       ba c8 86 04 08          mov    edx,0x80486c8
 80485ab:       c7 44 24 08 92 10 00    mov    DWORD PTR [esp+0x8],0x1092
 80485b2:       00 
 80485b3:       89 44 24 04             mov    DWORD PTR [esp+0x4],eax
 80485b7:       89 14 24                mov    DWORD PTR [esp],edx
 80485ba:       e8 a1 fd ff ff          call   8048360 <printf@plt>
 80485bf:       c7 04 24 01 00 00 00    mov    DWORD PTR [esp],0x1
 80485c6:       e8 d5 fd ff ff          call   80483a0 <exit@plt>
 80485cb:       c7 04 24 ef 86 04 08    mov    DWORD PTR [esp],0x80486ef
 80485d2:       e8 9d fe ff ff          call   8048474 <ft_des>
 80485d7:       ba 09 87 04 08          mov    edx,0x8048709
 80485dc:       89 44 24 04             mov    DWORD PTR [esp+0x4],eax
 80485e0:       89 14 24                mov    DWORD PTR [esp],edx
 80485e3:       e8 78 fd ff ff          call   8048360 <printf@plt>
 80485e8:       c9                      leave
```

A l'address `0x8048595` la fonction getuid est appele, ensuite a l'address `0x804859a` l'uid est compare a 4242 (0x1092).
Si les 2 uid sont les meme alors le programme jump a 0x80485cb, puis dans la suite du programme le flag sera affiche.

Le resultat du call getuid a l'address est stocke dans eax, qui est comparer a l'instruction suivante.

Pour avoir le flag il faut donc soit avoir le bon uid soit modifier la valeur de eax entre l'appel et la comparaison.

```bash
level13@SnowCrash:~$ gdb ./level13
(gdb) b *0x804859a
(gdb) r
(gdb) p $eax
$1 = 2013
(gdb) set $eax = 4242
(gdb) p $eax
$2 = 4242
(gdb) c
Continuing.
your token is 2A31L79asukciNyi8uppkEuSx
```
