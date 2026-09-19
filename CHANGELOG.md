# K4Y Puck – changelog

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
