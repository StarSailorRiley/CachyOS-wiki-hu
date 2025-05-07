---
title: CachyOS Kernel
description: A CachyOS kernel funkciói és változásai
---

A CachyOS Kernel egy testreszabott kernel, amely fejlesztéseket, konfigurációkat és javításokat használ upstream forgalomból.

## Funkciók

- Választhat 3 kernel ütemező és különféle [sched-ext](/configuration/sched-ext) ütemezők közül a jobb válaszidő érdekében
- AMD P-State fejlesztések
- Legújabb BBRv3 a Google-tól
- le9uo a jelentősen jobb válaszidő érdekében nagy memóriaterhelés esetén
- Naprakész NTSYNC patchkészlet, kompatibilis wine/proton builddel használható
- Kompatibilitás a T2 MacOS eszközökkel a [t2linux](https://github.com/t2linux/linux-t2-patches/) javításaival
- Lehetővé teszi a magonkénti CPU energiafelhasználás olvasását AMD felhasználók számára
- ACS Override és v412loopback
- VHBA modul CD/DVD-ROM eszközök emulálásához
- Legújabb ZSTD patchkészlet
- Különböző egyéb javítások, amelyek a teljesítmény javítására összpontosítanak (optimalizált kompilálócímkék, kriptográfiai fejlesztések, memóriakezelési finomhangolások)

A CachyOS által kínált javítások átfogóbb listájáért tekintse meg a teljesebb
[funkciólistát](https://github.com/CachyOS/linux-cachyos/?tab=readme-ov-file#features), a [kernel-patches repository](https://github.com/CachyOS/kernel-patches)-t
és a [CachyOS Linux forrásfáját](https://github.com/CachyOS/linux).

## Változatok

A CachyOS változatos kernel opciókat kínál. Az általunk biztosított összes kernel tartalmazza a [CachyOS Alap Patchkészlet](https://github.com/CachyOS/kernel-patches)-et.
Minden kernelhez tartozik egy [megfelelő `-lto` variáns](#package-naming-convention), amely [clang](https://clang.llvm.org/) kapcsolóval készült [GCC](https://gcc.gnu.org/) helyett. Mind az alapértelmezett, mind a `-rc` kernel kivétel ez alól, mivel alapértelmezés szerint [ThinLTO](https://blog.llvm.org/2016/06/thinlto-scalable-and-incremental-lto.html) kapcsolóval készültek, ezért ehelyett `-gcc` kernel variánsokkal rendelkeznek.

- **linux-cachyos**
    - Alapértelmezett kernel. Ez az ajánlott kernel, ha nem biztos benne, hogy melyik kernelt kell használni.
    - A [BORE](https://github.com/firelzrd/bore-scheduler) ütemezőt használja.
    - Alapértelmezés szerint clang és ThinLTO segítségével készült az optimalizáltabb bináris fájlok létrehozása érdekében.
    - Saját [AutoFDO](https://cachyos.org/blog/2411-kernel-autofdo/) profilunkkal profilizálva a jobb teljesítmény érdekében. A kernel profilozásához a [Script](https://github.com/CachyOS/cachyos-benchmarker/blob/master/kernel-autofdo.sh) szolgál.
- **linux-cachyos-bore**
    - A BORE ütemezőt használja.
- **linux-cachyos-bmq**
    - Az Alfred Chen által készített [Project C](https://gitlab.com/alfredchen/projectc/) BMQ ütemezőjét használja.
        - **Nem támogatja a sched-ext-et**.
- **linux-cachyos-deckify**
    - Alapértelmezett kernel kézi számítógépekhez. **Nem ajánlott** és **nem támogatott** más kernel használata kézi számítógépeken ezen kívül.
    - A BORE ütemezőt használja.
    - Kézi eszközökre specifikus javítások az alap patchkészleten kívül a kézi eszközök kompatibilitásának és az általános felhasználói élmény javítása érdekében.
- **linux-cachyos-eevdf** 
    - Módosítja az alapértelmezett kernel ütemezőt a jobb válaszidő érdekében.
- **linux-cachyos-lts** 
    - A legújabb Long Term Support kernel alapján.
    - A BORE ütemezőt használja.
    - Minimálisan javított a többi kernelhez képest a maximális stabilitás biztosítása érdekében.
- **linux-cachyos-hardened**
    - A BORE ütemezőt használja.
    - Tartalmazza a [linux-hardened](https://github.com/anthraxx/linux-hardened) patchkészletet.
    - Kernel konfig a [linux-hardened konfig](https://gitlab.archlinux.org/archlinux/packaging/packages/linux-hardened/-/blob/main/config) alapján.
        - Nagyon agresszív hardening-et tartalmaz, amely jelentősen rontja a teljesítményt és a felhasználói élményt.
        - **Nem támogatja a sched-ext-et**.
- **linux-cachyos-rc**
    - A [Linus fájáról] (https://github.com/torvalds/linux/) származó legújabb mainline kernel alapján.
    - A BORE ütemezőt használja.
    - A fő kernel a patchkészletünkbe való új javítások bevezetésére.
- **linux-cachyos-server**
    - Szerver terhelésekhez hangolva az asztali használathoz képest.
        - 300Hz tickráta.
        - Nincs preempció.
        - Gyári EEVDF.
- **linux-cachyos-rt-bore**
    - Valós idejű preempció.
    - A BORE ütemezőt használja.

Javaslatokért és fejlesztésekért, amelyek hozzáadhatók az alapértelmezett kernelhez, kérjük, nyisson meg egy problémát a [linux-cachyos GitHub](https://github.com/CachyOS/linux-cachyos) oldalon.

## Előreépített kernelmodulok

A nagyobb felhasználói bázis kiszolgálása érdekében a CachyOS néhány jól ismert és széles körben használt kernel modult is mellékel a kernelhez. Ez azt jelenti, hogy a felhasználóknak többé nem kell újrakompilálniuk ezeket a modulokat minden kernelfrissítés vagy minden új kernel telepítésekor, hanem csak a tárolóból kell telepíteniük őket, mivel az már előre le van kompilálva. Ez gyakorlatilag elavulttá teszi azokat a `-dkms` csomagokat, amelyekkel a felhasználó rendelkezhet, és amelyek ugyanazt a modult biztosítják, mint az előre lekompilált verzió.

### ZFS

A [ZFS](https://openzfs.org/wiki/Main_Page) egyike a CachyOS által támogatott számos fájlrendszernek. Mivel a [CDDL](https://opensource.org/license/cddl-1-0) licenc alatt van, nem kompatibilis a Linux kernel licencével, ezért nem egyesíthető a fában. A szállított modul tartalmazza a legújabb upstream funkciókat és javításokat a legújabb kernellel való kompatibilitás biztosítása érdekében.

### NVIDIA

A CachyOS mind a zárt- és [nyílt-forráskódú](https://github.com/NVIDIA/open-gpu-kernel-modules/) előrekompilált kernel moduljait szállítja. Mivel az NVIDIA kernel moduljának fejlesztése fán kívüli, és így nem követi a kernel kiadási ütemét, a gyári konfiguráció néha inkompatibilis lehet a legújabb
kernellel. Megoldásként a CachyOS közösség által létrehozott vagy az NVIDIA által közvetlenül megosztott javításokkal javítja a modulokat.

## Más modulok

A CachyOS kernelnek vannak más figyelemre méltó, apró, mégis felhasználói élményt javító funkciói is.

- Tartalmazza a kernel egy debug változatát, amely egy csupaszítatlan kernel bináris fájlt biztosít debugoló célokra. Ez a csomag szükséges a kernel AutoFDO-val történő profilozásához.
- A [Binder](https://developer.android.com/reference/android/os/Binder), a [Waydroid](https://waydro.id/)-hez szükséges modul alapértelmezés szerint engedélyezve van a kernel konfigurációjában,
és már [be is van állítva](https://github.com/CachyOS/linux-cachyos/blob/master/linux-cachyos/config#L10559).

## Csomagnevezési konvenció

```sh
linux-cachyos # Alap kernel csomag az alapértelmezett kernelhez. clang-el kompilálva
linux-cachyos-gcc # GCC-vel kompilált megfelelője a linux-cachyos-nek
linux-cachyos-{,gcc-}headers # Kernel fejlécek építéshez
linux-cachyos-{,gcc-}nvidia # Előkompilált zárt forráskodú NVIDIA modulok a linux-cachyos kernelhez
linux-cachyos-{,gcc-}nvidia-open
linux-cachyos-{,gcc-}zfs # Előkompilált ZFS modulok a linux-cachyos kernelhez
linux-cachyos-{,gcc-}dbg # Csupaszítatlan linux bináris fájl hibakereséshez

linux-cachyos-hardened # Alap kernel csomag a hardened kernelhez. GCC-vel kompilálva
linux-cachyos-hardened-lto # clang-el kompilált megfelelője a linux-cachyos-hardened-nek
linux-cachyos-hardened-{,lto-}headers
linux-cachyos-hardened-{,lto-}nvidia
linux-cachyos-hardened-{,lto-}nvidia-open
linux-cachyos-hardened-{,lto-}zfs
linux-cachyos-hardened-{,lto-}dbg
```

## Gyakik

### Miért nem használják az AutoFDO-t az összes többi kernelváltozathoz?

Mert költséges felépíteni, mivel alapvetően kétszer kell felépíteni a kernelt, ezért több erőforrást és időt igényel a kompilálás. Az AutoFDO-val rendelkező kernel építési folyamata a következő lépésekből áll:

1) A kernel felépítése az AutoFDO és a hibakeresési képességek engedélyezésével.
2) Profil létrehozása, amely a munkaterhelések végrehajtását jelenti, hogy profilozási adatokat gyűjtsön a lehetséges optimalizáláshoz.
3) A kernel újraépítése az AutoFDO profillal.

Ezért egyelőre csak a [linux-cachyos](/features/kernel#variants) változatban van jelen.

Az AutoFDO-ról további információért kattintson [ide.](https://cachyos.org/blog/2411-kernel-autofdo/)

### Javítja a valós idejű kernel a játékok teljesítményét?

Nem. Egy valós idejű kernel sokkal preemptívabbá teszi a kódot, mint egy átlag teljesen preemptív kernel. Ez azt jelenti, hogy sokkal több feladat (játékfolyamatok ideértve) van gyakran preemptálva, és kényszerítetten rendszererőforrási elsőbbséget ad, ami rosszabb teljesítményt eredményez.