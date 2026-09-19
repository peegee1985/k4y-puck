# K4Y Puck – changelog

## 0.4.11 (build 245) — 19. 9. 2026

- Správa Pucků rozlišuje fyzické Pucky od historických tokenů chytrých hodinek podle identity zařízení a telemetrie.
- Opakované párování stejného fyzického Pucku se v přehledu sloučí podle stabilního hardwarového `deviceId`.
- Dispečer může bezpečně zařadit jednorázový vzdálený restart nebo okamžitou kontrolu OTA.
- Webová správa umožňuje na dálku nastavit velikost textu, orientaci, zvuk, hlasitost, běžný jas a čas spořiče.
- Puck přebírá příkazy při pravidelné synchronizaci; příkaz restartu je vydán pouze jednou, takže nemůže vytvořit restartovací smyčku.
- Nastavení se ukládá do NVS a přežije restart. Staré nastavení verze 1 se beze ztráty migruje s výchozím jasem 70 %.
- Telemetrie doplňuje hlasitost a jas; backend zůstává kompatibilní s buildem 244 během pořadí nasazení.
- Zachovány zprávy probouzející displej, blokace spořiče do potvrzení, OTA SHA-256, rollback, `-O2`, PSRAM a SD diagnostika.

## 0.4.10 (build 244) — 19. 9. 2026

- Nová zpráva nebo announcement z dispečinku okamžitě probudí displej a otevře obrazovku zprávy.
- Dokud řidič zprávu nepotvrdí tlačítkem `PŘEČTENO`, firmware nepovolí přechod do spořiče obrazovky.
- Pokud se potvrzení nepodaří odeslat, zpráva zůstane aktivní a displej se neuspí; nedojde tedy k tichému ztracení upozornění.
- Po úspěšném potvrzení se běžný časovač spořiče rozběhne znovu od okamžiku akce řidiče.
- Diagnostika SD karty zobrazuje rychlost čtení a zápisu na desetiny MiB/s, takže hodnoty pod 1 MiB/s už nejsou chybně zobrazeny jako `R0 W0`.
- Verze na stavové obrazovce se skládá přímo z verze sestavení a není už zadaná natvrdo.
- Nové chování displeje i formátování SD rychlosti mají samostatné regresní testy.
- Zachovány optimalizace `-O2`, PSRAM, OTA SHA-256 kontrola, rollback, dotyk, otáčení, Wi-Fi profily a uživatelská data.

## 0.4.9 (build 243) — 19. 9. 2026

- Běžný dashboard refresh používá po nasazení odpovídajícího backendu jeden agregovaný HTTPS požadavek místo tří samostatných TLS spojení.
- Firmware zachovává úzký fallback na staré endpointy pouze při HTTP 404/405, takže OTA lze bezpečně instalovat před backendem.
- Produkční obraz je kompilován s optimalizací `-O2`; při přechodu byla opravena a regresně otestována omezená kopie UI textů.
- OTA partial download používá 4 KiB HTTP požadavky, což snižuje přechodnou spotřebu interní RAM.
- Synchronizace odesílá verzi, uptime, Wi-Fi RSSI, heap/PSRAM, důvod restartu, rezervy stacků a stav SD do webové správy Pucků.
- Stavová obrazovka zobrazuje aktuální RAM, největší blok, historické minimum, PSRAM a rychlost SD.
- SD karta má oddělené adresáře `/K4Y/cache`, `maps`, `logs`, `offline` a `system`.
- Neinvazivní 512 KiB benchmark používá dočasný unikátní soubor, kontroluje přečtený vzor a soubor po měření odstraní.
- Velký benchmarkovací buffer je v PSRAM; SD karta se nepoužívá jako RAM.
- Zachovány OTA SHA-256 kontrola, rollback, dotyk, otáčení, Wi-Fi profily, párování a uživatelská data.

## 0.4.8 (build 242) — 19. 9. 2026

- OTA manifest nově vyžaduje platný 64znakový SHA-256 a přesnou velikost obrazu.
- Puck po stažení znovu spočítá SHA-256 přímo z neaktivní OTA partition; nesoulad ukončí aktualizaci dříve, než se obraz označí jako bootovatelný.
- Po zjištění aktuální verze se OTA kontrola opakuje každých šest hodin, takže budoucí staged rollout nevyžaduje ruční restart.
- Přidána runtime diagnostika: aktuální a minimální volná interní RAM, největší blok, PSRAM, uptime a důvod restartu.
- Obrazovka `STAV ZAŘÍZENÍ` zobrazuje stav SD karty a volnou/minimální interní RAM.
- microSD se inicializuje na nativní jedné datové lince podle zapojení Waveshare (GPIO14/17/16, EXIO3).
- Firmware pouze připojí existující FAT32; nikdy kartu automaticky neformátuje. exFAT nebo neplatný filesystem zobrazí jako `SD VYŽADUJE FAT32`.
- SD probe proběhne ještě před potvrzením nového OTA obrazu, aby fatální regrese ovladače zůstala krytá rollbackem.
- Zachovány funkční otáčení, dotyk, Wi-Fi profily, párování, zvuk, progress bar a NVS data.

## 0.4.7 (build 241) — 19. 9. 2026

- Automatická rotace má nový stavový automat: správné mapování os QMI8658, známou výchozí polohu a jedinou změnu orientace na jeden fyzický pohyb.
- Přechod mezi akcelerometrem a gyroskopem je uzamčený až do souvislého klidu, takže stejný pohyb už nemůže obraz otočit dvakrát.
- Dotyk rozlišuje skutečný tap od pohybu prstu a scrollu; akce se provede až po uvolnění bez překročení pohybového limitu.
- Prázdný interval eventového SPD2010 se už nepovažuje za puštění prstu a ztracený release se bezpečně ukončí časovým limitem.
- Opakované překreslování už nehromadí callbacky obrazovky a jeden swipe proto nemůže spustit navigaci vícekrát.
- LVGL smyčka vždy uvolní alespoň jeden FreeRTOS tick; odstraněn busy-spin, který zpomaloval dotyk, síť i animace.
- Zápis nastavení do NVS probíhá mimo UI smyčku, takže přepínání voleb nezadrhává obraz.
- OTA aktualizace zobrazuje samostatnou obrazovku, skutečný průběh 0–100 %, fázi ověření a restartu.
- Čekající síťové akce a připojování používají plynulý průběhový indikátor.
- Ve spořiči se podsvícení sníží na 30 % a po probuzení se vrátí na 70 %.
- Zachovány uložené Wi-Fi profily, párování, nastavení, rollback a stávající partition table.

## 0.4.6 (build 240) — 19. 9. 2026

- Puck si nyní bezpečně pamatuje až pět ověřených Wi‑Fi sítí v odděleném NVS profilu.
- Existující Wi‑Fi z buildu 239 se při prvním startu automaticky migruje, bez ztráty připojení nebo párovacího tokenu.
- Po ztrátě sítě Puck nejdříve zkusí naposledy úspěšný profil a potom automaticky prochází ostatní známé sítě.
- Nová síť se uloží až po skutečném získání IP adresy; chybné heslo tedy nepřepíše funkční profil.
- Telefonní nastavení Wi‑Fi už nemaže dříve uložené sítě a automaticky se nabídne, pokud není dosažitelná žádná známá síť.
- Tlačítka se aktivují až dokončeným klepnutím, takže swipe v nastavení už nespouští položku pod prstem.
- Přechod mezi obrazovkami čeká na uvolnění dotyku a blokuje opakovaný tap, který dříve propadl do nově otevřeného menu.
- SPD2010 touch polling nyní filtruje jeden prázdný vzorek během pohybu a běží po 10 ms; LVGL plánování bylo zrychleno.
- Automatická rotace má citlivější, ale stabilizovaný práh akcelerometru i gyroskopu; manuální režimy 0° a 90° zůstávají zachované.
- Zachovány OTA rollback, přesné logo, přímé zprávy a announcements, zvuk, nastavení a všechna NVS data.

## 0.4.5 (build 239) — 19. 9. 2026

- Běžná synchronizace po 20 sekundách už nevkládá do pracovní obrazovky stav `SYNC...`.
- Jednotlivý krátký výpadek API už okamžitě nepřepne Puck do stavu OFFLINE; poslední platný stav se drží přes tři pokusy.
- Po neúspěchu se první dva pokusy opakují po 5 sekundách a teprve třetí souvislá chyba se zobrazí uživateli.
- OTA boot self-test již nevrací jinak funkční firmware jen kvůli přechodně nedostupnému touch řadiči; stav dotyku zůstává v diagnostice.
- Po selhání OTA se kontrola po pěti minutách automaticky zopakuje bez nutnosti restartu zařízení.
- Veřejný obraz buildu 238 byl obnoven z ověřeného CI artefaktu poté, co jej následný chybný upload zkrátil.
- Zachovány přesné K4Y logo, přímé zprávy i announcements, nastavení, zvuk, automatická rotace, Wi‑Fi údaje, párování a rollback na build 237.

## 0.4.4 (build 238) — 19. 9. 2026

- Screensaver používá skutečný grafický znak K4Y se správnou kresbou písmen a šipky.
- Přímé zprávy dispečera jsou zapojené do stejné fronty jako provozní announcements.
- Po potvrzení se jako přečtená označí přesně zobrazená chatová zpráva nebo announcement.
- Zachována zpětná kompatibilita s announcement ID ze starších buildů.
- Zachovány OTA rollback, Wi‑Fi konfigurace, zvuk, dotykové ovládání, nastavení a automatická rotace.
