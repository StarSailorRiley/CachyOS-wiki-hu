---
title: Miért a CachyOS?
description: Miért lenne jobb önnek a CachyOS
---

A CachyOS (ejtsd: kesi-ó-essz) kifinomult Arch élményt kínál, felhasználóbarát telepítővel, előre beállított desktop környezetekkel és teljesítményoptimalizálással, anélkül, hogy a felhasználói élményt és a rendszer biztonságát gyengítené. Az alábbiakban bemutatjuk néhány kiemelkedő funkcióját, melyeket a CachyOS a lenyűgöző asztali élmény érdekében biztosít.

## Optimalizált csomagok és repository-k

A CachyOS optimalizált csomagokat kínál különféle hardverkonfigurációkhoz, beleértve az x86-64-v3, x86-64-v4 és Zen4+ rendszereket, a rendszerteljesítmény javításának céljából. Ezenkívül a CachyOS gyakran kért [AUR](https://aur.archlinux.org/) csomagokat is szállít a felhasználók QoL-ának javítása érdekében.

A CachyOS által optimalizált különféle csomagokról bővebben az [Optimalizált Repository-k](/features/optimized_repos) oldalon olvashat.

## Teljesítményre és stabilitásra hangolt egyedi Kernel

A CachyOS alap kerneljavító készletén kívül, amely a kernel különböző paramétereit hangolja az asztal válaszidejének javítása érdekében, a CachyOS ígéretes javításkészleteket is kiválaszt, 
melyek nem szerepelnek a kernel stabil verziójában. Ezek a javítások belső tesztelésen esnek át, mielőtt a felhasználókhoz kerülnének,
hogy biztosítsák stabilitásukat. A CachyOS által biztosított javítások teljes listáját lásd a [Kernel](/features/kernel) oldalon.

## Egyéni CPU-ütemező támogatása

A CPU-ütemezés a kernel fontos része annak biztosítására, hogy minden feladat igazságosan jusson processzoridőhöz. A Linux kernel különféle ütemezési osztályokat valósít meg,
hogy minden egyes feladat megfelelően legyen ütemezve. A fair scheduling class, ismertebb nevén egyszerűen "alapértelmezett ütemező", az [EEVDF (Earliest Eligible Virtual Deadline First)](https://lwn.net/Articles/925371/) algoritmuson alapul.

Alapértelmezetten az EEVDF úgy van beállítva, hogy igazságosan ossza el a rendelkezésre álló processzoridőt az összes feladat között, és főként az átviteli sebességre orientált munkaterhelésekhez van igazítva. A CachyOS kernel
[konfigurál néhány EEVDF tunable-t](https://github.com/CachyOS/linux/blob/6.12/cachy/kernel/sched/fair.c#L76-L79), hogy az asztal válaszidejét helyezze előtérbe a puszta átviteli sebességgel szemben.

Az EEVDF-et azonban eredetileg nem asztali hozzáférésre szánták. Ezt szem előtt tartva, a CachyOS a [BORE (Burst-Oriented Response Enhancer)](https://github.com/firelzrd/bore-scheduler) ütemezővel javított kerneleket szállít, amely egy további tulajdonságot vezet be, mellyel a gyors válaszidőt igénylő feladatoknak több processzoridőt rendel azok helyett, amelyek a "burstiness"-ük alapján nem igénylik ezt.

A 6.12-es verzióban a Linux kernel lehetővé teszi a BPF ütemezők gyors telepítését és a fair scheduling class egy másik ütemezővel való helyettesítését. Ennek megkönnyítése érdekében
a CachyOS elsőosztályú támogatást nyújt a [sched-ext ütemezőkhöz](https://github.com/sched-ext/scx)

A CachyOS által kínált kernelekről és a sched-ext ütemezőkről további információt a [Kernel](/features/kernel) és a [sched-ext](/configuration/sched-ext/) oldalon talál.

## Hardverfelismerés

A CachyOS saját [hardverfelismerő eszközt](https://github.com/CachyOS/chwd) kínál, amely helyesen telepíti a szükséges csomagokat és illesztőprogramokat minden rendszerhez, hogy megkönnyítse a felhasználók számára a telepítés utáni setupot.

## Testreszabható telepítési folyamat

A CachyOS telepítője garantálja, hogy a felhasználók szabadon választhatják ki a kívánt rendszerüket. Ez a testreszabhatóság többek közt a következőket foglalja magában:
- [Desktop környezetek](/installation/desktop_environments/)
- [Boot managerek](/installation/boot_managers/)
- [Kernel-variánsok](/features/kernel#variants)
- [Fájlrendszerek](/installation/filesystem)
- Egyéni csomagok telepítése a telepítési folyamat során

## CachyOS alkalmazások

Alapértelmezés szerint a CachyOS biztosít felhasználót segítő alkalmazásokat, mint például a CachyOS Hello és a CachyOS Package Installer, hogy egyszerűsítse és javítsa a Linux élményt.

A CachyOS Hello például lehetőséget biztosít a rendszer frissítésére, a szolgáltatások engedélyezésére és a letöltési tükrök rangsorolására. Emellett egy kattintással elérhető finomhangolásokat és javításokat is tartalmaz néhány
gyakori problémára. A Package Installer segít a csomagok telepítésében.

A CachyOS által fejlesztett és karbantartott alkalmazások listája:

- **Cachy Browser**: Firefox-alapú böngésző, biztonságosabb konfigurációval és Gentoo-javításokkal + Teljesítményoptimalizálással.
- **CachyOS Kernel Manager**: Könnyen telepíthet kerneleket a repository-ból, vagy konfigurálhatja saját kernelét, és beépítheti saját javításait, sőt, akár a sched-ext keretrendszert is kezelheti az [scx_loader](<https://github.com/sched-ext/scx/tree/main/rust/scx_loader>) segítségével.
- **CachyOS Hello**: Alkalmazás a CachyOS-sel kapcsolatos módosítások vezérléséhez, javítások alkalmazásához, csomagok telepítéséhez és további információkhoz.
- **CachyOS Package Installer**: Grafikus felhasználói felület a gyakran használt alkalmazások egyszerű telepítéséhez.
- **cachyos-rate-mirrors**: Automatikusan rangsorolja az Arch és a CachyOS letöltési tükreit az optimális letöltési sebesség érdekében.
- **systemd-boot-manager**: Automatikusan új bejegyzéseket generál a systemd-boot-manager számára, és könnyen konfigurálható az `/etc/sdboot-manage.conf` fájlban.

## Barátságos és aktív közösség

A legfontosabb tényező a CachyOS folyamatosan növekvő közössége. A közösségünk nélkül a CachyOS soha nem érhetett volna el oda, ahol most van.
A közösség tagjai segítik egymást, tippeket és trükköket osztanak meg egymással a jobb Linux-élmény érdekében. Csatlakozzon a
[CachyOS Discordon](https://discord.com/invite/cachyos-862292009423470592) vagy a [CachyOS fórumon](https://discuss.cachyos.org/).