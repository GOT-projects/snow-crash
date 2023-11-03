```nasm
08048946 <main>:
[...]
 8048afd:       e8 ae f9 ff ff          call   80484b0 <getuid@plt>
 8048b02:       89 44 24 18             mov    DWORD PTR [esp+0x18],eax
 8048b06:       8b 44 24 18             mov    eax,DWORD PTR [esp+0x18]
 8048b0a:       3d be 0b 00 00          cmp    eax,0xbbe
 8048b0f:       0f 84 b6 01 00 00       je     8048ccb <main+0x385>
 8048b15:       3d be 0b 00 00          cmp    eax,0xbbe
 8048b1a:       77 4c                   ja     8048b68 <main+0x222>
 8048b1c:       3d ba 0b 00 00          cmp    eax,0xbba
 8048b21:       0f 84 14 01 00 00       je     8048c3b <main+0x2f5>
 8048b27:       3d ba 0b 00 00          cmp    eax,0xbba
[...]
```

Problem le programme check s'il est debug, si oui il exit

```bash
level14@SnowCrash:~$ gdb /bin/getflag 
(gdb) b *0x8048b06
Breakpoint 1 at 0x8048b06
(gdb) r
Starting program: /bin/getflag 
You should not reverse this
[Inferior 1 (process 2722) exited with code 01]
```

```nasm
08048946 <main>:
[...]
 8048989:       e8 b2 fb ff ff          call   8048540 <ptrace@plt>
 804898e:       85 c0                   test   eax,eax
 8048990:       79 16                   jns    80489a8 <main+0x62>
 8048992:       c7 04 24 a8 8f 04 08    mov    DWORD PTR [esp],0x8048fa8
 8048999:       e8 42 fb ff ff          call   80484e0 <puts@plt>
 804899e:       b8 01 00 00 00          mov    eax,0x1
 80489a3:       e9 0a 05 00 00          jmp    8048eb2 <main+0x56c>
[...]
 8048eb2:       8b 94 24 1c 01 00 00    mov    edx,DWORD PTR [esp+0x11c]
 8048eb9:       65 33 15 14 00 00 00    xor    edx,DWORD PTR gs:0x14
 8048ec0:       74 05                   je     8048ec7 <main+0x581>
 8048ec2:       e8 d9 f5 ff ff          call   80484a0 <__stack_chk_fail@plt>
 8048ec7:       8b 5d fc                mov    ebx,DWORD PTR [ebp-0x4]
 8048eca:       c9                      leave  
 8048ecb:       c3                      ret    
 8048ecc:       90                      nop
 8048ecd:       90                      nop
 8048ece:       90                      nop
 8048ecf:       90                      nop
```

La premiere partie concerne le check de si le programme est reverse.
La deuxieme partie celle est la fin du main a partir de l'address jump si l'appel de `ptrace` est signe.

```bash
level14@SnowCrash:~$ gdb /bin/getflag 
(gdb) b *0x804898e
Breakpoint 1 at 0x804898e
(gdb) b *0x8048b02
Breakpoint 2 at 0x8048b02
(gdb) r
Starting program: /bin/getflag 

Breakpoint 1, 0x0804898e in main ()
(gdb) i r eax
eax            0xffffffff	-1
(gdb) set $eax = 0
(gdb) i r eax
eax            0x0	0
(gdb) n
Single stepping until exit from function main,
which has no line number information.

Breakpoint 2, 0x08048b02 in main ()
(gdb) i r eax
eax            0x7de	2014
(gdb) set $eax = 3014
(gdb) i r eax
eax            0xbc6	3014
(gdb) n
Single stepping until exit from function main,
which has no line number information.
Check flag.Here is your token : 7QiHafiNa3HVozsaXkawuYrTstxbpABHD8CPnHJ
```
