### 🌞 Créer un répertoire de travail dans votre répertoire personnel

```powershell
me@Debian11:~$ ls
Desktop  Documents  Downloads  Music  Pictures  Public  Templates  Videos
me@Debian11:~$ cd ./Desktop/
me@Debian11:~/Desktop$ mkdir work
```

### 🌞 Retrouvez à l'aide de readelf l'architecture pour laquelle le programme est compilé

```powershell
e@Debian11:~/Desktop/work$ readelf -h hello1 | grep 32
  Class:                             ELF32
  Size of program headers:           32 (bytes)
```

### 🌞 Afficher la liste des sections contenues dans le programme

```powershell
me@Debian11:~/Desktop/work$ readelf -S hello1 | grep text
  [ 6] .text             PROGBITS        00001000 001000 000060 00  AX  0   0  1
  ```

  ### 🌞 Affichez le code assembleur de la section .text à l'aide d'objdump

  ```powershell
  me@Debian11:~/Desktop/work$ objdump -M intel -j .text -d hello1

hello1:     file format elf32-i386


Disassembly of section .text:

00001000 <_start>:
    1000:       55                      push   ebp
    1001:       89 e5                   mov    ebp,esp
    1003:       57                      push   edi
    1004:       56                      push   esi
    1005:       53                      push   ebx
    1006:       83 ec 10                sub    esp,0x10
    1009:       e8 4e 00 00 00          call   105c <__x86.get_pc_thunk.ax>
    100e:       05 f2 2f 00 00          add    eax,0x2ff2
    1013:       c7 45 e5 48 65 6c 6c    mov    DWORD PTR [ebp-0x1b],0x6c6c6548
    101a:       c7 45 e9 6f 2c 20 57    mov    DWORD PTR [ebp-0x17],0x57202c6f
    1021:       c7 45 ed 6f 72 6c 64    mov    DWORD PTR [ebp-0x13],0x646c726f
    1028:       66 c7 45 f1 21 0a       mov    WORD PTR [ebp-0xf],0xa21
    102e:       c6 45 f3 00             mov    BYTE PTR [ebp-0xd],0x0
    1032:       8d 75 e5                lea    esi,[ebp-0x1b]
    1035:       bf 0e 00 00 00          mov    edi,0xe
    103a:       b8 04 00 00 00          mov    eax,0x4
    103f:       bb 01 00 00 00          mov    ebx,0x1
    1044:       89 f1                   mov    ecx,esi
    1046:       89 fa                   mov    edx,edi
    1048:       cd 80                   int    0x80
    104a:       b8 01 00 00 00          mov    eax,0x1
    104f:       31 db                   xor    ebx,ebx
    1051:       cd 80                   int    0x80
    1053:       90                      nop
    1054:       83 c4 10                add    esp,0x10
    1057:       5b                      pop    ebx
    1058:       5e                      pop    esi
    1059:       5f                      pop    edi
    105a:       5d                      pop    ebp
    105b:       c3                      ret

0000105c <__x86.get_pc_thunk.ax>:
    105c:       8b 04 24                mov    eax,DWORD PTR [esp]
    105f:       c3                      ret
    
    
```
### 🌞 Tracez à l'aide de la commande ldd les librairies appelées par...

```powershell

me@Debian11:~/Desktop/work$ ldd hello2
        linux-gate.so.1 (0xf7f4b000)
        libc.so.6 => /lib32/libc.so.6 (0xf7d45000)
        /lib/ld-linux.so.2 (0xf7f4d000)
me@Debian11:~/Desktop/work$ ldd hello1
        statically linked
me@Debian11:/$ file /usr/bin/ls
/usr/bin/ls: ELF 64-bit
me@Debian11:/$ file /usr/bin/firefox
/usr/bin/firefox: POSIX shell script, ASCII text executable
```

### 🌞 Parmi les librairies appelées par hello2, déterminer le type du fichier nommé libc.so.X

```powershell
me@Debian11:~/Desktop/work$ ldd hello2
        linux-gate.so.1 (0xf7f42000)
        libc.so.6 => /lib32/libc.so.6 (0xf7d3c000)
        /lib/ld-linux.so.2 (0xf7f44000)
me@Debian11:/lib32$ file libc.so.6
libc.so.6: symbolic link to libc-2.31.so
me@Debian11:/lib32$ file libc-2.31.so
libc-2.31.so: ELF 32-bit
```

### 🌞 Affichez le type des fichiers hello2 et hello3

```powershell
me@Debian11:~/Desktop/work$ ldd hello2
        linux-gate.so.1 (0xf7f37000)
        libc.so.6 => /lib32/libc.so.6 (0xf7d31000)
        /lib/ld-linux.so.2 (0xf7f39000)
me@Debian11:~/Desktop/work$ ldd hello3
        not a dynamic executable
```

### 🌞 Affichez leurs tailles

```powershell
me@Debian11:~/Desktop/work$ du -h  hello2
20K     hello2
me@Debian11:~/Desktop/work$ du -h  hello3
684K    hello3
```

### 🌞 Affichez l'architecture de votre CPU

```powershell
me@Debian11:~/Desktop/work$ cat /proc/cpuinfo
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 94
model name      : Intel(R) Core(TM) i7-6820HQ CPU @ 2.70GHz
stepping        : 3
cpu MHz         : 2712.000
cache size      : 8192 KB
physical id     : 0
siblings        : 1
core id         : 0
cpu cores       : 1
apicid          : 0
initial apicid  : 0
fpu             : yes
fpu_exception   : yes
cpuid level     : 22
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush mmx fxsr sse sse2 ht syscall nx rdtscp lm constant_tsc rep_good nopl xtopology nonstop_tsc cpuid tsc_known_freq pni pclmulqdq monitor ssse3 fma cx16 pcid sse4_1 sse4_2 x2apic movbe popcnt aes xsave avx f16c rdrand hypervisor lahf_lm abm 3dnowprefetch invpcid_single pti fsgsbase bmi1 avx2 bmi2 invpcid rdseed adx clflushopt arat md_clear flush_l1d
bugs            : cpu_meltdown spectre_v1 spectre_v2 spec_store_bypass l1tf mds swapgs itlb_multihit srbds mmio_stale_data retbleed gds
bogomips        : 5424.00
clflush size    : 64
cache_alignment : 64
address sizes   : 39 bits physical, 48 bits virtual
power management:
```
### 🌞 Vérifiez que vous avez la commande x86_64-linux-gnu-gcc
```powershell
me@Debian11:~/Desktop/work$ which x86_64-linux-gnu-gcc
/usr/bin/x86_64-linux-gnu-gcc
```

### 🌞 Compilez votre fichier hello3.c dans un fichier cible nommé hello4 vers une autre architecture que la vôtre

```powershell

me@Debian11:~/Desktop/work$ aarch64-linux-gnu-gcc -static -fno-stack-protector -g -o hello4 hello3.c
me@Debian11:~/Desktop/work$ ls
hello1  hello1.c  hello2  hello2.c  hello3  hello3.c  hello4

```
### 🌞 Désassemblez hello3 et hello4 à l'aide d'objdump

```powershell
me@Debian11:~/Desktop/work$ objdump -M intel -j .text -d hello3

hello3:     file format elf32-i386


Disassembly of section .text:

08049090 <__assert_fail_base.cold>:
 8049090:       83 ec 0c               sub    esp,0xc
 8049093:       ff 74 24 20            push   DWORD PTR [esp+0x20]
 8049097:       e8 54 59 01 00         call   805e9f0 <__free>
 804909c:       83 c4 10               add    esp,0x10
 804909f:       e8 0c 00 00 00         call   80490b0 <abort>

080490a4 <plural_eval.cold>:
 80490a4:       31 ed                  xor    ebp,ebp
 80490a6:       e9 f9 1e 00 00         jmp    804afa4 <plural_eval+0x44>

080490ab <_nl_load_domain.cold>:
 80490ab:       e8 00 00 00 00         call   80490b0 <abort>

080490b0 <abort>:
 80490b0:       55                     push   ebp
 80490b1:       e8 20 69 00 00         call   804f9d6 <__x86.get_pc_thunk.bp>
 80490b6:       81 c5 4a 9f 09 00      add    ebp,0x99f4a
 80490bc:       57                     push   edi
 80490bd:       56                     push   esi
 80490be:       53                     push   ebx
 80490bf:       65 8b 1d 08 00 00 00   mov    ebx,DWORD PTR gs:0x8
 80490c6:       81 ec 1c 01 00 00      sub    esp,0x11c
 80490cc:       65 a1 14 00 00 00      mov    eax,gs:0x14
 80490d2:       89 84 24 0c 01 00 00   mov    DWORD PTR [esp+0x10c],eax
 80490d9:       31 c0                  xor    eax,eax
 80490db:       39 9d f8 17 00 00      cmp    DWORD PTR [ebp+0x17f8],ebx
 80490e1:       74 41                  je     8049124 <abort+0x74>
 80490e3:       65 a1 0c 00 00 00      mov    eax,gs:0xc
 80490e9:       85 c0                  test   eax,eax
 80490eb:       75 0e                  jne    80490fb <abort+0x4b>
 80490ed:       ba 01 00 00 00         mov    edx,0x1
 80490f2:       0f b1 95 f0 17 00 00   cmpxchg DWORD PTR [ebp+0x17f0],edx
 80490f9:       eb 23                  jmp    804911e <abort+0x6e>
 80490fb:       31 c0                  xor    eax,eax
 80490fd:       ba 01 00 00 00         mov    edx,0x1
 8049102:       f0 0f b1 95 f0 17 00   lock cmpxchg DWORD PTR [ebp+0x17f0],edx

me@Debian11:~/Desktop/work$ objdump -M intel -j .text -d hello4

hello4:     file format elf64-littleaarch64


Disassembly of section .text:

0000000000400340 <abort>:
  400340:       objdump: unrecognised disassembler option: intel
a9ab7bfd        stp     x29, x30, [sp, #-336]!
  400344:       b0000440        adrp   x0, 489000 <__sys_errlist_internal+0x348>
  400348:       910003fd        mov    x29, sp
  40034c:       a90153f3        stp    x19, x20, [sp, #16]
  400350:       90000473        adrp   x19, 48c000 <object.0+0x28>
  400354:       d53bd054        mrs    x20, tpidr_el0
  400358:       a9025bf5        stp    x21, x22, [sp, #32]
  40035c:       91268275        add    x21, x19, #0x9a0
  400360:       d11c0294        sub    x20, x20, #0x700
  400364:       f945dc00        ldr    x0, [x0, #3000]
  400368:       f94006a1        ldr    x1, [x21, #8]
  40036c:       f9400002        ldr    x2, [x0]
  400370:       f900a7e2        str    x2, [sp, #328]
  400374:       d2800002        mov    x2, #0x0                         // #0
  400378:       eb14003f        cmp    x1, x20
  40037c:       54000140        b.eq   4003a4 <abort+0x64>  // b.none
  400380:       aa1503e2        mov    x2, x21
  400384:       52800021        mov    w1, #0x1                         // #1
  400388:       52800000        mov    w0, #0x0                         // #0
  40038c:       94013325        bl     44d020 <__aarch64_cas4_acq>
  400390:       34000060        cbz    w0, 40039c <abort+0x5c>
  400394:       aa1503e0        mov    x0, x21
  400398:       94003922        bl     40e820 <__lll_lock_wait_private>
  40039c:       91268260        add    x0, x19, #0x9a0
  4003a0:       f9000414        str    x20, [x0, #8]
  4003a4:       91268263        add    x3, x19, #0x9a0
  4003a8:       b9400460        ldr    w0, [x3, #4]
  4003ac:       b9401061        ldr    w1, [x3, #16]
  4003b0:       11000400        add    w0, w0, #0x1
  4003b4:       b9000460        str    w0, [x3, #4]
  4003b8:       350001a1        cbnz   w1, 4003ec <abort+0xac>
  4003bc:       52800036        mov    w22, #0x1                        // #1
  4003c0:       d2800f02        mov    x2, #0x78                        // #120
  4003c4:       9100e3e0        add    x0, sp, #0x38
  4003c8:       b9001076        str    w22, [x3, #16]
  4003cc:       97ffffc1        bl     4002d0 <.plt+0x30>
  4003d0:       9100c3f5        add    x21, sp, #0x30
  4003d4:       d2800403        mov    x3, #0x20                        // #32
  4003d8:       2a1603e0        mov    w0, w22
  4003dc:       aa1503e1        mov    x1, x21
  4003e0:       d2800002        mov    x2, #0x0                         // #0
  4003e4:       f9001be3        str    x3, [sp, #48]
  4003e8:       9400144e        bl     405520 <__sigprocmask>
  4003ec:       91268275        add    x21, x19, #0x9a0
  4003f0:       b94012a0        ldr    w0, [x21, #16]
  4003f4:       7100041f        cmp    w0, #0x1
  4003f8:       540004a1        b.ne   40048c <abort+0x14c>  // b.any
  4003fc:       b94006a0        ldr    w0, [x21, #4]
  400400:       b90012bf        str    wzr, [x21, #16]
  400404:       51000400        sub    w0, w0, #0x1
  400408:       b90006a0        str    w0, [x21, #4]
  40040c:       35000180        cbnz   w0, 40043c <abort+0xfc>
  400410:       f90006bf        str    xzr, [x21, #8]
  400414:       aa1503e1        mov    x1, x21
  400418:       94013362        bl     44d1a0 <__aarch64_swp4_rel>
  40041c:       7100041f        cmp    w0, #0x1
  400420:       540000ed        b.le   40043c <abort+0xfc>
  400424:       aa1503e0        mov    x0, x21
  400428:       d2801021        mov    x1, #0x81                        // #129
  40042c:       d2800022        mov    x2, #0x1                         // #1
  400430:       d2800003        mov    x3, #0x0                         // #0
  400434:       d2800c48        mov    x8, #0x62                        // #98
  400438:       d4000001        svc    #0x0
  40043c:       91268275        add    x21, x19, #0x9a0
  400440:       528000c0        mov    w0, #0x6                         // #6
  400444:       940013eb        bl     4053f0 <raise>
  400448:       f94006a0        ldr    x0, [x21, #8]
  40044c:       eb14001f        cmp    x0, x20
  400450:       54000140        b.eq   400478 <abort+0x138>  // b.none
  400454:       aa1503e2        mov    x2, x21
  400458:       52800021        mov    w1, #0x1                         // #1
  40045c:       52800000        mov    w0, #0x0                         // #0
  400460:       940132f0        bl     44d020 <__aarch64_cas4_acq>
  400464:       34000060        cbz    w0, 400470 <abort+0x130>
  400468:       aa1503e0        mov    x0, x21
  40046c:       940038ed        bl     40e820 <__lll_lock_wait_private>
  400470:       91268260        add    x0, x19, #0x9a0
  400474:       f9000414        str    x20, [x0, #8]
  400478:       91268261        add    x1, x19, #0x9a0
  40047c:       b9400420        ldr    w0, [x1, #4]
  400480:       11000400        add    w0, w0, #0x1
  400484:       b9000420        str    w0, [x1, #4]
  400488:       14000003        b      400494 <abort+0x154>
  40048c:       7100081f        cmp    w0, #0x2
  400490:       54000221        b.ne   4004d4 <abort+0x194>  // b.any
  400494:       91268263        add    x3, x19, #0x9a0
  400498:       52800064        mov    w4, #0x3                         // #3
  40049c:       9102c3f4        add    x20, sp, #0xb0
  4004a0:       d2801302        mov    x2, #0x98                        // #152
  4004a4:       52800001        mov    w1, #0x0                         // #0
  4004a8:       aa1403e0        mov    x0, x20
 ```

 ```
 bah ca a pas la meme geule et y a pas les memes instructions au meme endroit ;-;
 ```

 ### 🌞 Essayez d'exécuter le programme hello4

 ```
 uKR
3F`_%q~Xa60hP}AYܫ|x{Z˫:k(7CkOI)SsVZt\cẐnIRĿ+=[<m=%)s%{^%+~Ej6șbxc\tʢ\a%P-{7
gKSFBsoxgvQfE_3
5QB"UUh(dB,G7N4}[L|Z12Іv[kGXsՖaQ@p6iV        }st?mŊt<LdN8=^
>#a7qd4NA@U)~4\k4USj68{[P`xJ3]GmaG]us&`cmOf.L3F.     幱!=&uHiWzI
u4]=JcdrGk_P6ՅPuu48瞀x"R\z{DQdI#g~HH]1c鐂XdM       ~3 Y2p'8=pZ|<<AK}倔acI+bͭF:,OeKS͒0"t8GVr"To<zV{`F4nHoɝۈH����z0HH7I}Gg=b*
/#`?V[;IV'PvTA3V a#AuVvtN!oCb6'@MW&f*5U9$i8hLddKR X^LdXnk4ݙNx(/Wn)n81ܘT^E6g?!d~UzR!r)hVB̖{I
k1d5LJ
֟okiB;)!Ӌk'|UE<,Ӡ:7pyM댢M4q>vOy;+3S]l*Cʑ0_dISoy}hgVlw!H˘FYD/>?Y1Y4FV2ѷjّ[@$><9*qx]     HzJ|7?s"h7Ӝ%CρnVBH(5Yϋ~خ
```
### 🌞 Avec une commande apt search, déterminez si le paquet ghidra est disponible
### 🌞 Ajouter l'URL des dépôts Kali à vos dépôts existants
### 🌞 Ajoutez la clé publique des gars de chez Kali
### 🌞 Mettez à jour la liste des dépôts que votre OS connaît actuellement
### 🌞 Avec une commande apt search, déterminez si le paquet ghidra est disponible
### 🌞  Installez le paquet ghidra


``` powershell
┌─[matyspg@parrot]─[~]
└──╼ $apt search ghidra
Sorting... Done
Full Text Search... Done
ghidra/parrot6,now 10.3.2-0parrot1 amd64 [installed,automatic]
  Software Reverse Engineering Framework

ghidra-data/parrot6,now 10.4-0parrot1 all [installed,automatic]
  FID databases for Ghidra

rizin-plugin-ghidra/parrot6 0.6.0-1parrot2 amd64
  A fork of Ghidra's decompiler for Rizin and Cutter

rizin-plugin-ghidra-dbgsym/parrot6 0.6.0-1parrot2 amd64
  debug symbols for rizin-plugin-ghidra
```
vive parrot lol 

### 🌞 Récupérez le code de password_2.c sur la machine Linux et compilez-le
impossible a compiler xd

###

```powershell

┌─[matyspg@parrot]─[~/work]
└──╼ $wget https://gitlab.com/it4lik/b1-os/-/blob/main/tp/5/kaddate_challenge
--2024-11-28 02:16:06--  https://gitlab.com/it4lik/b1-os/-/blob/main/tp/5/kaddate_challenge
Resolving gitlab.com (gitlab.com)... 172.65.251.78, 2606:4700:90:0:f22e:fbec:5bed:a9b9
Connecting to gitlab.com (gitlab.com)|172.65.251.78|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 36369 (36K) [text/html]
Saving to: ‘kaddate_challenge’

kaddate_challenge   100%[===================>]  35,52K  --.-KB/s    in 0,002s  

2024-11-28 02:16:07 (20,3 MB/s) - ‘kaddate_challenge’ saved [36369/36369]
┌─[✗]─[matyspg@parrot]─[~/work]
└──╼ $./kaddate_challenge
bash: ./kaddate_challenge: Permission denied
┌─[✗]─[matyspg@parrot]─[~/work]
└──╼ $sudo ./kaddate_challenge
[sudo] password for matyspg: 
sudo: ./kaddate_challenge: command not found


┌─[✗]─[matyspg@parrot]─[~/work]
└──╼ $echo "c3VwZXJzZWNyZXRmbGFnCg==" | base64 -d
supersecretflag
```
xd
