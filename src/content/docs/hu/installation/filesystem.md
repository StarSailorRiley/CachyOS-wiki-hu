---
title: Fájlrendszerek
description: Az elérhető fájlrendszerek leírása és ajánlásai (ext4, f2fs, btrfs, xfs, zfs, bcachefs)
---

A CachyOS 5 fájlrendszert kínál, hogy a felhasználó kiválaszthassa az igényeinek leginkább megfelelőt. Az alábbiakban az egyes fájlrendszerek előnyeit, hátrányait és ajánlásait ismertetjük. Minden fájlrendszerhez előre telepítve vannak a CachyOS-en a követelmények/segédprogramok.

:::note[Megjegyzés]
A BTRFS a CachyOS alapértelmezett és ajánlott fájlrendszere. Ha bizonytalan, válassza ezt.
:::

## XFS
Az XFS egy journaling fájlrendszer, amelyet a Silicon Graphics, Inc. készített és fejlesztett. 1993-ban hozták létre, 2001-ben portolták Linuxra, és ma már a legtöbb Linux disztribúció széles körben támogatja.
### Előnyök
- Gyors, az XFS-t eredetileg a sebesség és a rendkívüli skálázhatóság szem előtt tartásával tervezték.
- Megbízható, az XFS számos technológiát használ az adatvesztés megakadályozására.
- Ellenáll a töredezésnek a kiterjedésalapú jellege és a késleltetett allokációs stratégiája miatt.
### Hátrányok
- Nem lehet zsugorítani.

### Felhasználói tér segédprogram
Az XFS fájlrendszerek kezelésére szolgáló felhasználói térbeli eszközöket tartalmazó csomag az `xfsprogs`.

### Ajánlás:
Az XFS az ajánlott fájlrendszer azoknak a felhasználóknak, akiknek nincs szükségük haladó funkciókra, és egyszerűen csak egy gyors és megbízható fájlrendszert szeretnének.


## BTRFS
A BTRFS egy modern, 2007-ben létrehozott, copy-on-write (másolás-írás) (COW) fájlrendszer, amelyet 2013-ban stabilnak nyilvánítottak a Linux kernelben. Széles körben támogatott, és főként fejlett funkciókészletéről ismert.
### Előnyök
- Átlátszó tömörítés. A BTRFS támogatja a fájlok átlátszó tömörítését, így jelentős helymegtakarítást érhet el felhasználói beavatkozás nélkül. A CachyOS alapértelmezés szerint 3-as ZSTD tömörítési szintet használ.
- Snapshot funkció. A BTRFS a COW természetét kihasználva lehetővé teszi olyan alkötetek (subvolumeok) pillanatképeinek készítését, amelyek nagyon kevés helyet foglalnak el.
- Alkötet (subvolume) funkcionalitás, amely lehetővé teszi a fájlrendszer nagyobb irányítását.
- Képes kiterjedni vagy zsugorodni.
- Nagyon gyors fejlesztés.
### Hátrányok
- Néha töredezettségmentesítést vagy kiegyensúlyozást igényel.
- Forgó meghajtókon rosszabb a fent említett töredezettség miatt.
### Felhasználói tér segédprogram
A Btrfs felhasználói tér segédprogramcsomagja a `btrfs-progs`.

### Alkötet (subvolume) elrendezés
A CachyOS alapértelmezés szerint egy alkötet elrendezést biztosít az egyszerű pillanatkép-készítési funkciókhoz.
- Subvol @ = /
- Subvol @home = /home
- Subvol @root = /root
- Subvol @srv = /srv
- Subvol @cache = /var/cache
- Subvol @tmp = /var/tmp
- Subvol @log = /var/log

### Ajánlás:
A BTRFS-t azoknak a felhasználóknak ajánljuk, akik snapshot/mentés funkciót és transzparens tömörítést szeretnének.


## EXT4
Az EXT4 (fourth extended filesystem) a leggyakrabban használt Linux fájlrendszer. Az EXT4-et 2008-ban tették stabillá a Linux kernelben.
### Előnyök
- Nagyon elterjedt, könnyű hozzáférést biztosít számos erőforráshoz.
- Megbízható. Az EXT4 bizonyítottan nagyon megbízható.
- Képes kiterjedni vagy zsugorodni.
### Hátrányok
- Régi kódalapra épül.
- Hiányzik belőle sok olyan fejlett funkció, amit más fájlrendszerek kínálnak.

### Felhasználói tér segédprogram
Az ext4 kezelésére szolgáló csomag az `e2fsprogs`.

### Ajánlás:
Az EXT4 azoknak a felhasználóknak ajánlott, akik a legegyszerűbb és leggyakrabban használt fájlrendszert szeretnék.

## ZFS

A ZFS egy fejlett fájlrendszer, amelyet eredetileg a Sun Microsystems fejlesztett ki 2005-ben. A ZFS számos funkcióval rendelkezik, azonban CDDL licenc alatt van, ami azt jelenti, hogy nem integrálható a Linux kernelbe, és külön modul telepítését igényli.

:::caution[Figyelem]
Ne használj valós idejű kernelt ZFS-sel együtt, mert licencelési problémák miatt nem kompatibilisek.
:::

### Előnyök
- Pooled storage (zpool)
- Pillanatképek COW használatával
- Tömörítés
- Raid-Z támogatás
- Az ARC gyorsítótár hihetetlenül gyors olvasási időt biztosít a gyakran használt fájlokon.
### Hátrányok
- Nagyon bonyolult a használata és megértése olyan funkciók miatt, mint a zpool és az ARC.
- Az ARC sok RAM-ot igényel a hatékony működéshez.
- Nem része a Linux kernelnek, ezért egy harmadik féltől származó kernel modultól (OpenZFS) függ.
- Nem kompatibilis a valós idejű preempcióval.

### Szükséges eszközök
'ZFS-Module' A CachyOS minden kernel verzióhoz előre kompilált zfs modult biztosít.
A felhasználói térben található segédprogramokhoz a `zfs-utils` modult használjuk.

### Ajánlás:
A ZFS-t csak haladó felhasználók képesek használni, akik a ZFS speciális funkcióit, például a közös tárhelyet vagy az ARC gyorsítótárat szeretnék használni.


## F2FS
Az F2FS, vagyis a Flash-Friendly File System egy flash fájlrendszer, amelyet a Samsung eredetileg a Linux kernelhez készített és fejlesztett ki. Az F2FS-t kifejezetten a modern tárolókban használt NAND flash memóriák kiszolgálására hozták létre.
### Előnyök
- Flash-barát kialakítással tervezve.
- Átlátszó tömörítés a lemezírások számának csökkentésére (a felhasználó jelenleg nem használhatja fel a helymegtakarítást).
- Gyorsabb, mint más fájlrendszerek, például az EXT4.
- Jobb kopáskiegyenlítés, ami tovább növeli a NAND flash élettartamát.
### Hátrányok
- Nem zsugorítható.
- A tömörítésből származó helymegtakarítást a felhasználó jelenleg nem használhatja fel. Ez a jövőben lehet hozzá lesz adva.
- Viszonylag gyenge fsck. (fájlrendszer-ellenőrzés)
- A fájlrendszert létrehozó verziónál régebbi kernelre való visszalépés problémákat okozhat.

### Felhasználói tér segédprogram
Az f2fs fő segédprogramja az `f2fs-tools`.

### Ajánlás:
Az F2FS csak azoknak a felhasználóknak ajánlott, akik maximalizálni szeretnék NAND flash memóriájuk élettartamát.

## BcacheFS
A Bcachefs egy fejlett új fájlrendszer Linuxra, amely a megbízhatóságra és a robusztusságra helyezi a hangsúlyt, és a modern fájlrendszerektől elvárható funkciók teljes skáláját kínálja.

:::caution[FIGYELEM]
A Bcachefs még kísérleti jellegűnek számít, és problémákat okozhat.
:::

### Előnyök
- Copy on write (CoW) - mint a BTRFS vagy a ZFS
- Tömörítés
- Gyorsítótárazás, adatelhelyezés
- Replikáció
- Skálázható
### Hátrányok
- Kísérleti
- A beállítás bonyolult lehet

## Röviden összefoglalva
Használja az alapértelmezett **BTRFS** fájlrendszert, mivel stabilnak tekinthető, és sok hasznos funkcióval rendelkezik (pillanatképek, tömörítés stb.). Használjon **XFS** vagy **EXT4** formátumot egy egyszerű és gyors fájlrendszerhez.

