---
title: Hibajelentés
---

# Írja le a problémáját

- *Mi nem működik?*
- *Az X csomag korábbi verzióra váltása megoldja a problémát?*
- *Használja a keresési funkciót az azonos problémákhoz*
- *Végzett már saját módosításokat?*
  - Példa: `További jelző hozzáadása egy modprobe fájlban`

# Logok biztosítása

A CachyOS egy nagyszerű eszközt biztosít a logok rendszerből történő gyűjtésére, a `cachyos-bugreport.sh`-t.
Ez az eszköz a következő helyekről gyűjti a logokat:
- dmesg
- journalctl
- inxi `(Hardverinformációk gyűjtése)`

A logok összegyűjtése után a felhasználónak döntenie kell, hogy feltölti-e azokat a beillesztési webhelyünkre.

**Futtassa a következő parancsot a terminálban, és tegye közzé a hibákat tartalmazó linket a témában:**
```sh
sudo cachyos-bugreport.sh
```

# Linkek a jelentés benyújtásához

- Github: <https://github.com/CachyOS/distribution>
- Fórum: <https://discuss.cachyos.org/c/feedback/bugreports/10>
- Discord: [Támogatási Csatorna](https://discord.com/channels/862292009423470592/862294383470051348)
