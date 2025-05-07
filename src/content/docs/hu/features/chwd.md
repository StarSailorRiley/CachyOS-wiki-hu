---
title: CachyOS hardverfelismerő
description: Hardverészlelés és -konfigurálás a CachyOS-hez
---

A [CachyOS hardverészlelés](https://github.com/CachyOS/chwd/), vagy ismertebb nevén **`chwd`** lehetővé teszi számunkra, hogy különféle hardvereket működtessünk a futó rendszerhez szükséges csomagok és illesztőprogramok telepítésével. Ide tartoznak az NVIDIA videokártyát futtató rendszerek, a T2-es Macbookok, és a kézi eszközök mint a Steam Deckek és a ROG Ally.

## Használat

A **`chwd`** parancs jellemzően a telepítés során lefut, hogy biztosítsa a rendszer számára a szükséges csomagokat. Azonban a telepítés után is használható.

### Automatikus konfiguráció

A **`chwd`** támogatja a szükséges illesztőprogramok és csomagok telepítését és konfigurálását, hogy a rendszer optimális körülmények között működjön.

```sh
❯ sudo chwd -a
```

### Profil telepítése

A fenti módszer alternatívája az egyes profilok telepítése.

```sh title='Az összes elérhető profil listázása'
❯ chwd --list-all
╭─────────────────────────┬─────────╮
│ Name                    ┆ NonFree │
╞═════════════════════════╪═════════╡
│ nvidia-open-dkms.prime  ┆ true    │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ nvidia-dkms             ┆ true    │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ macbook-t2              ┆ false   │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ phoenix                 ┆ false   │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ steam-deck              ┆ false   │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ amd                     ┆ false   │
├╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌┼╌╌╌╌╌╌╌╌╌┤
│ intel                   ┆ false   │
╰─────────────────────────┴─────────╯
```

```sh title='chwd profil telepítése'
❯ sudo chwd -i amd
> Installing amd ...

> Successfully installed amd
```

### Más lehetőségek

A parancs szintaxisával és egyéb használati módokkal kapcsolatban tekintse meg a **`chwd`** help kimenetét.

```sh
❯ chwd --help
Usage: chwd [OPTIONS]

Options:
  -i, --install <profile>          Install profile # Profil telepítése
  -r, --remove <profile>           Remove profile # Profil eltávolítása
  -d, --detail                     Show detailed info for listings # Listázás részletes információ mutatása 
  -f, --force                      Force reinstall # Kényszerített újratelepítés
      --list-installed             List installed kernels # Telepített kernelek listázása
      --list                       List available profiles for all devices # Elérhető profilok listázása az összes eszközre
      --list-all                   List all profiles # Összes profil listázása
  -a, --autoconfigure [<classid>]  Autoconfigure # Autokonfiguráció
      --ai_sdk                     Toggle AI SDK profiles # AI SDK profilok ki/bekapcsolása
      --pmcachedir <PMCACHEDIR>    [default: /var/cache/pacman/pkg]
      --pmconfig <PMCONFIG>        [default: /etc/pacman.conf]
      --pmroot <PMROOT>            [default: /]
  -h, --help                       Print help # Súgó
  -V, --version                    Print version # Verzió
```
