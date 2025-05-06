---
title: Felkínált Boot Managerek
description: A jelenleg kínált boot managerek leírása és ajánlása
---

A lehető legjobb élmény nyújtására a CachyOS jelenleg a következő boot managereket kínálja: systemd-boot, rEFInd, GRUB és Limine.
Ez a wiki cikk ismerteti az egyes boot managerek funkciókészletét, és tartalmazza a kiválasztásukra vonatkozó ajánlásainkat is. A
konfigurációhoz lásd: [Boot manager konfiguráció](/configuration/boot_manager_configuration).

## systemd-boot

A systemd család részeként a systemd-bootot a lehető legegyszerűbbre tervezték, ezért csak az UEFI alapú rendszereket támogatja. Ez az egyszerű, mégis hatékony kialakítás biztosítja a megbízhatóságot és a gyorsaságot. Ez azonban a többi boot manager által támogatott fejlett funkciók rovására megy.

### Előnyök
- Nagyon egyszerű konfiguráció.
- A rendszerindító bejegyzések több fájlba vannak osztva, így könnyebben kezelhetők.

### Hátrányok
 - Nem támogatja a BIOS rendszereket.
 - Nagyon egyszerű kialakítású, és semmilyen témázási vagy testreszabási   lehetőséget nem kínál.
 - A konfig nem generálódik automatikusan, hacsak nincs erre konfigurálva. A CachyOS tartalmazza a systemd-boot-manager-t az automatikusan generált konfigurációhoz.
 - Csak EFI által támogatott fájlrendszereken (FAT, FAT16, FAT32) lévő rendszerindító képeket képes olvasni.
 - A saját partícióján kívül képtelen rendszerindító képeket találni.
 - Nem támogatja megfelelően a Btrfs snapshot-visszagörgetéseket, mivel a kernel képeket a boot partíción, és nem a root fájlrendszeren kell tárolni.

### Ajánlás

A Systemd-boot a CachyOS ajánlott és alapértelmezett rendszerindító kezelője. Ha bizonytalan, válassza ezt.

## rEFInd

A rEFIt egy forkja, a rEFInd elsősorban azért készült, hogy megkönnyítse a MacOS felhasználók számára a többrendszeres rendszerindítást. A rEFInd azonban mára hardverfüggetlenné fejlődött, így nagyszerű választássá vált a többrendszeres rendszerindításhoz bármilyen rendszeren. A rEFInd fő vonzereje, hogy képes az összes tárolóeszközt beolvasni rendszerindításkor, és ennek megfelelően megjeleníteni az egyes talált operációs rendszerek/kernelek bejegyzéseit.

### Előnyök

- Az összes operációs rendszer és kernel automatikus felismerése a tárolóeszközökön.
- A fent említett automatikus felismerésnek köszönhetően kevés vagy semmilyen konfigurációt nem igényel.
- Sokkal grafikusabb felhasználói felület, amely a MacOS rendszerindító választójára emlékeztet.
- Nagyszerű téma-támogatás
- Opcionális érintőképernyő-támogatás
- Képes rendszerindító képek olvasására EFI fájlrendszerekből (FAT, FAT16, FAT32), valamint EXT4 és BTRFS fájlokból. Más fájlrendszerek támogatása az ``efifs`` csomag EFI illesztőprogramjainak telepítésével adható hozzá.

### Hátrányok

- Nem támogatja a BIOS rendszereket.

### Ajánlás

A rEFInd az ajánlott rendszerindító menedzser több operációs rendszerrel történő indításhoz.

## GRUB

A GRUB a legrégebbi elérhető boot manager. Nagyon széles funkciókészlettel rendelkezik, szinte minden gépen működik, és ez a leggyakrabban használt Linux boot manager. Az alábbiakban felsoroljuk a főbb előnyeit és hátrányait.

### Előnyök
- Szinte az összes elérhető Linux fájlrendszerről képes beolvasni rendszerindító képeket.
- Széles körben használt és nagyon könnyen megtalálható információ online.
- Képes dekriptálni az enkriptált boot partíciókat.
- Az egyetlen olyan boot loader, amely lehetővé teszi BIOS-os gépek indítását.
- Elavult kinézetű. Azonban nagyszerű téma-támogatással rendelkezik, ami kárpótol érte.

### Hátrányok
- Felfújt, mivel sokkal régebbi hardvereket is kell támogatnia, és sok fájlrendszer-illesztőprogramra van szüksége.
- Észrevehetően lassabb a systemd-boothoz és a rEFIndhoz képest.

### Ajánlás

A GRUB az egyetlen boot manager, amely támogatja a boot partíció enkriptálását (különbözik a lemez enkripciótól)

## Limine

A Limine egy modern, fejlett és hordozható, többprotokollos rendszerbetöltő. A Limine rendszerbetöltő protokoll referencia implementációjaként szolgál, és támogatja a Linux indítását, valamint más boot loaderek láncbetöltését.

### Előnyök

- Több rendszerindítási protokollt támogat, beleértve a Multiboot2-t és a Linux rendszerindítási protokollokat.
- UEFI és BIOS rendszereken is képes indulni, így sokoldalúan használható különböző hardverkonfigurációkhoz.
- A GRUB-hoz hasonló téma-beállítási képességekkel rendelkezik.
- Közvetlen támogatás a Btrfs snapshotokhoz, amely alapértelmezetten engedélyezve van a Btrfs fájlrendszert használó telepítéseknél.

### Hátrányok

- Csak néhány fájlrendszert támogat, például FAT12, FAT16, FAT32 és ISO9660 a `/boot` partícióhoz, ami további beállítást igényelhet más fájlrendszereket használó rendszerek esetén.
- Néhány más rendszerbetöltővel ellentétben a Limine nem ad hozzá automatikusan bejegyzést az NVRAM-hoz UEFI rendszereken; ezt manuálisan kell elvégezni olyan eszközökkel, mint az `efibootmgr`, vagy a `limine-entry-tool` segítségével, amely előre telepítve van a CachyOS-re.

### Ajánlás

A Limine azoknak a felhasználóknak ajánlott, akiknek egy könnyű és sokoldalú rendszerbetöltőre van szükségük, amely mind az UEFI, mind a BIOS rendszereket támogatja. Különösen alkalmas azok számára, akik az egyszerű beállítást kedvelik, témázási lehetőségekkel és Btrfs snapshot-támogatással. Ezenkívül a Limine a GRUB modern helyettesítőjeként szolgál, amely az utóbbi időben kevesebb frissítést kapott, és számos biztonsági problémával szembesült az EFI/fájlrendszer-illesztőprogramjai miatt.

## Röviden összefoglalva
Válassza a GRUB-ot, ha a használt gép csak BIOS-t használ, a rEFInd-et, ha több operációs rendszert tervez a gépen (különösen Windowst), egyébként használja a systemd-boot-ot.
