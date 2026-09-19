# K4Y Puck – changelog

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
