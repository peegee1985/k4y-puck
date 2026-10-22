# K4Y Puck – changelog

## 0.4.16 (build 250) — 22. 10. 2026

- Automatická rotace rozlišuje oba směry i všechny čtyři polohy 0/90/180/270. Původní kód zahazoval znaménko gravitace a směr gyra, takže levé otočení dostalo stejnou 90° korekci jako pravé.
- LCD překreslování nyní podporuje 180° a 270° včetně částečných oken, zarovnání SPD2010 a inverzního mapování dotyku přes LVGL. Ruční režimy 0/90 a uložené nastavení jsou kompatibilní.
- Nadpis OTA se už nezalamuje přes upozornění NEVYPÍNEJ ZAŘÍZENÍ. Všechny řádky mají pevné samostatné místo a kratší texty; nepodporovaná tečka ve zprávě o restartu byla nahrazena pomlčkou.
- Znovuvytvořená OTA obrazovka ihned přebere aktuální stav a procenta.
- Nastavení Pucku → JAS SPOŘIČE: samostatných 10–100 %. Klepnutí snižuje jas o 10 procentních bodů, při 10 % další klepnutí přejde na 100 %. Výchozí jas zůstává 30 %. Běžný jas se po probuzení i při zahájení OTA obnoví.
- Nová hodnota přežije restart a změnu běžného jasu vzdáleným příkazem. Samostatný NVS klíč zachovává původní 8bajtový blob nastavení i kompatibilitu s rollbackem. Menu při změnách udrží odscrollovanou pozici.

### Audit baterie

ADC používá kalibraci ESP32 a stejný přepočet 3 / 0,990476 jako oficiální příklad Waveshare; průměruje 16 vzorků. Výpočet i zobrazení všech 0–100 % na dashboardu a spořiči byly ověřeny. Křivka ani odhad nabíjení se v tomto vydání nemění.

- 4,10 V → 93 %, 4,16 V → 97 %, 4,18 V → 99 %, 4,20 V → 100 %.
- Zaokrouhlení na 100 % začíná při filtrovaných 4,193 V.
- Obnova po 15 s a filtr mohou dosažení 100 % zpozdit; při modelovém skoku 4,10 → 4,20 V je prodleva 135 s. Při startu s 4,20 V je 100 % ihned.
- Jde o orientační odhad z napětí, nikoli měření skutečné kapacity v mAh nebo přímé potvrzení dokončení nabíjení. Při nabíjení může 100 % přijít dříve, při nižším koncovém naměřeném napětí nemusí přijít vůbec. Přesnost konkrétního článku vyžaduje fyzický nabíjecí/vybíjecí test a srovnání napětí; nebyla potvrzena.
- Kalibrační konstanta: [oficiální Waveshare BAT_Driver.h](https://github.com/waveshareteam/ESP32-S3-Touch-LCD-1.46/blob/main/example/ESP-IDF-5.3.2/ESP32-S3-Touch-LCD-1.46-Test/main/BAT_Driver/BAT_Driver.h).

### Ověření

Nové testy skutečného k4y_settings.c s NVS mockem: samostatné hodnoty jasu, cyklování až na 10 %, uložení/obnovení, kompatibilita v1/v2, chybějící a neplatný nový klíč a nezměněná velikost původního blobu. LVGL menu: dva stisky přes produkční event handler nastaví 10 %, text se vejde a odscrollovaná pozice se uchová.

MSVC /W4 /WX: podepsané polohy IMU, oba směry gyra, návrat, čtyři samostatné čtvrtotáčky, blokace dvojí rotace stejného pohybu, pixelové transformace, částečná okna, zarovnání a dotyková inverze. Skutečný LVGL 8.3.11 s produkčními fonty: bez překryvů OTA, text uvnitř kruhu, 200 průběžných překreslení shodných s plným snímkem, všechny hodnoty baterie na obou obrazovkách. Produkční LCD flush/rounder v simulaci: shoda každého pixelu s očekávaným snímkem a správné kliknutí přes skutečný LVGL pointer pro všechny čtyři polohy.

Při instalaci 250 zobrazuje OTA obrazovku dosavadní firmware 249; opravené rozložení se použije pro aktualizace spouštěné už z 250. Fyzické potvrzení nové rotace na konkrétním Pucku čeká po OTA. Firmware zachovává párování, Wi-Fi, nastavení, SHA-256 a rollback; nevyžaduje backend ani webový deploy.

### Ověřený firmware

- APP: 1733776 B
- SHA-256: `1546f730e0fcc845313a5d3f8e58fea7fe05a9a867c01b64920db4cb45fd8a48`
- Zdroj: `901b94a1e01221555ff4121d8e870d010fa46d57`
- [Zdrojové CI](https://github.com/peegee1985/macaly-kuryr4-you-dispecink-qxs5v8/actions/runs/39856267374) — všechny tři joby prošly.
- [PR #206](https://github.com/peegee1985/macaly-kuryr4-you-dispecink-qxs5v8/pull/206) — sloučen do firmware větve.
- GitHub CI ZIP SHA-256: `b53b6c997b38eba3d2a1afd98038672f3807bb4834c15264fde2335695843f12` — stažený archiv odpovídá digestu.
- ESP APP descriptor: 0.4.16 / k4y_puck; APP je shodná s obrazem pro offset 0x10000 z flash balíčku.


## 0.4.15 (build 249) — 22. 10. 2026

- Procenta na spořiči jsou nyní těsně u baterie místo zarovnání k pravému okraji širokého pole.
- Ikona baterie, případný nabíjecí blesk a procenta mají mezi sousedními prvky 6 px.
- Když blesk není zobrazený, nezabírá prázdné místo.
- Čas a celý bateriový blok se automaticky vycentrují společně, s mezerou 24 px.
- Detekce nabíjení zůstává z buildu 248; uživatel potvrdil, že se blesk po prodlevě objeví.
- Provozní obrazovka, měření baterie, napájení, párování, Wi-Fi a OTA rollback zůstávají stejné.

Ověřeno skutečným LVGL 8.3.11 renderem: přesný rozestup, vycentrování, kruhový
výřez, šířka textu, barvy, skrytý/zobrazený blesk a 100 přepnutí stránek.
Fyzické ověření rozestupu následuje po OTA instalaci 248 → 249.

## Ověřený firmware
- APP: 1733136 B
- SHA-256: `36ad710efa5472adb9231a588acf11ca24a953448725e156a61cf52cae666d6e`
- Zdroj: `fc1742ff45df382dacffeccb575c0fe216f40cf3`
- [Zdrojové CI](https://github.com/peegee1985/macaly-kuryr4-you-dispecink-qxs5v8/actions/runs/39850528352) — všechny tři joby prošly.
- [PR #205](https://github.com/peegee1985/macaly-kuryr4-you-dispecink-qxs5v8/pull/205) — sloučen.
- Stažený CI ZIP odpovídá GitHub digestu; APP descriptor potvrzuje 0.4.15 / k4y_puck.


## 0.4.14 (build 248) — 22. 10. 2026

- OTA navazuje na build 247 používaný v novém Pucku.
- Provozní obrazovka: pod Nastavením je barevná ikona baterie a procenta.
- Spořič: čas a baterie jsou vedle sebe ve vycentrované skupině.
- Zelená 50–100 %, oranžová 20–49 %, červená pod 20 %.
- Malý blesk na obou obrazovkách indikuje odhad nabíjení podle soustavného růstu napětí.
- Měření se obnovuje po 15 sekundách a je filtrované; zastaralé či neplatné hodnoty ukazují -- %.
- Blesk zhasne při poklesu napětí, chybě měření, plné baterii nebo vypršení trendu.
- Zapnutý symbolový font, sjednocená verze firmware i ESP obrazu 0.4.14 / 248.
- Zachováno napájení GPIO7, kalibrované měření GPIO8, párování, Wi-Fi, nastavení a OTA rollback.

Procenta jsou odhadem podle napětí. ETA6098 STAT je na této desce připojen pouze
k fyzické nabíjecí LED; blesk proto není přímým potvrzením připojeného USB či
nabíjecího proudu. Přesný okamžitý stav nabíjení bez změny hardware nelze číst.

Ověření: bateriové C testy s MSVC /W4 /WX a GCC, plné firmware kontrakty,
web/TypeScript, ESP-IDF 5.5.2 pro ESP32-S3, skutečný LVGL 8.3.11 render a
100 přepnutí stránek. Kontrola kruhového výřezu, vycentrování, šířky procent,
barvy a viditelnosti blesku prošla.

Fyzický test instalace 247 → 248 a chování na novém Pucku čeká na uživatele.
Párování ani data se kvůli aktualizaci nemažou.

## Ověřený obraz
- Velikost: 1732768 B
- SHA-256: `beca9de91bdf6d37b39b53b05ebcd1be2be23bcc15b54b9499cce7cdf045210a`
- Zdroj: `14b676bf3c476b49e1d3a55d084a3f90405b9140`
- [CI sestavení](https://github.com/peegee1985/macaly-kuryr4-you-dispecink-qxs5v8/actions/runs/39768848337)
- [PR #204](https://github.com/peegee1985/macaly-kuryr4-you-dispecink-qxs5v8/pull/204)
- [Oficiální schéma Waveshare](https://files.waveshare.com/wiki/ESP32-S3-Touch-LCD-1.46/ESP32-S3-Touch-LCD-1.46.pdf)


## 0.4.12 (build 246) — 19. 9. 2026

- Dlouhé hlášky v pravém horním rohu už nemohou přetéct do názvu řidiče ani stavu zakázky; text nahradil pevný 16px stavový bod.
- Zelený bod znamená online, oranžový synchronizaci nebo přechodnou chybu a červený potvrzený výpadek připojení.
- Stavové texty čekající akce, připojování a OTA mají pevnou jednořádkovou výšku s výpustkou, takže se nepřekrývají s dalšími prvky.
- Běžná 20sekundová synchronizace už při nezměněných datech nezničí a znovu nevytvoří celý dashboard; tím se snižuje práce LVGL a rušivé překreslování.
- Překreslení zůstává vynucené po akci řidiče a při změně zakázky, zprávy nebo dat dashboardu.
- Zachovány párování, dotyk, orientace, Wi-Fi profily, NVS nastavení, zprávy blokující spořič, OTA SHA-256 a rollback.

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
