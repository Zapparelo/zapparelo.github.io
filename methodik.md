---
layout: page
title: Methodik und Quellen
description: "So werden die MaStR-Zahlen auf dieser Seite gezählt: Datenstand, Förderende, Kreisabgrenzung über den Amtlichen Gemeindeschlüssel und Lizenz."
permalink: /methodik/
image: /assets/og-default.png
---

Diese Seiten enthalten eigene Auswertungen öffentlicher Registerdaten zu Photovoltaik in Deutschland. Sie sind keine Rechts-, Steuer- oder Anlageberatung.

## Datenquelle

Grundlage ist das [Marktstammdatenregister (MaStR)](https://www.marktstammdatenregister.de) der Bundesnetzagentur, abgerufen am **17.11.2026**. Lizenz: [Datenlizenz Deutschland – Namensnennung – Version 2.0](https://www.govdata.de/dl-de/by-2-0). Namensnennung: Bundesnetzagentur, Marktstammdatenregister; eigene Auswertung durch pegton, aggregiert.

Erfasst sind Solar-Einheiten mit Status „In Betrieb“ und Inbetriebnahmejahr 2000 bis 2020 (je nach Auswertung eingeschränkt, im Text angegeben), dedupliziert nach MaStR-Nummer. Im Register stehen 417.003 solcher Einheiten mit Inbetriebnahme 2000–2008 (die Kohorten, deren Förderung bis 2028 endet oder bereits geendet hat).

## Wie das Förderende bestimmt wird

Nach dem EEG läuft die feste Vergütung 20 Kalenderjahre plus das restliche Inbetriebnahmejahr. Eine Anlage aus 2006 verliert die feste Vergütung damit am **31.12.2026**. In den Tabellen ist „Förderende 2026“ also der Jahrgang mit Inbetriebnahme 2006.

Ob eine konkrete Anlage tatsächlich EEG-Vergütung bezieht, ist im Register nicht erkennbar. Anlagen, die früher stillgelegt wurden, fehlen in den Zahlen; die frühen Jahrgänge unterzeichnen deshalb, wie viele Anlagen ursprünglich gebaut wurden. „Anlage“ heißt hier eine registrierte Einheit. Manche Betreiber registrieren mehrere Einheiten.

## Landkreise: Gruppierung über den Gemeindeschlüssel

Das MaStR-Feld „Landkreis“ verwendet für eine kreisfreie Stadt und den gleichnamigen Landkreis denselben Namen. Das betrifft 22 Paare, zum Beispiel Karlsruhe, München, Regensburg, Heilbronn und Passau. Wer nach diesem Namen gruppiert, addiert Stadt und Landkreis unbemerkt zusammen.

Auf diesen Seiten werden Kreise über die **ersten fünf Stellen des Amtlichen Gemeindeschlüssels (AGS)** getrennt. Bei Namensgleichheit lautet die Bezeichnung „Landkreis …“ bzw. „… (Stadt)“. Zwei Einheiten mit dem veralteten Kreisschlüssel 03152 (früherer Landkreis Göttingen) wurden dem heutigen Schlüssel 03159 zugeordnet.

Gemeinden mit gleichem Namen in verschiedenen Ländern bleiben getrennt, etwa Arnstein in Unterfranken (Landkreis Main-Spessart) und Arnstein in Sachsen-Anhalt.

## Weitere Quellen

- EEG 2023, insbesondere §§ 21, 23b, 53, in der Fassung des Solarpakets I (Anschlussvergütung bis 31.12.2032 für Anlagen bis 100 kW; anzulegender Wert = Jahresmarktwert, höchstens 10 ct/kWh). Vermarktungsabzug nach § 53 EEG, veröffentlicht von den Übertragungsnetzbetreibern auf [netztransparenz.de](https://www.netztransparenz.de) (2026: 0,228 ct/kWh; 2027: 0,601 ct/kWh; halbiert mit intelligentem Messsystem).
- Jahresmarktwerte Solar und Monatsmarktwerte: Übertragungsnetzbetreiber / netztransparenz.de.
- Haushaltsstrompreis: BDEW-Strompreisanalyse 10/2026 (Durchschnitt 37,3 ct/kWh; Neukunden zum 1.11.2026: 40,1 ct/kWh).
- Umbaukosten Eigenverbrauch: Verbraucherzentrale (ab etwa 200 €, mit neuem Zählerschrank bis etwa 2.000 €).
- Smart-Meter-Pflicht: § 29 Messstellenbetriebsgesetz (Pflichteinbau über 7 kW, Umsetzung durch den Messstellenbetreiber bis 2032).
- Einkommensteuer: § 3 Nr. 72 EStG (Einnahmen aus Anlagen bis 30 kWp auf Ein- und Zweifamilienhäusern seit 2022 steuerfrei). Keine Aussage zur Umsatzsteuer im Einzelfall.

Beispielrechnungen nutzen, wenn nicht anders angegeben, den Median der privaten Anlagen mit Förderende 2026 (6,84 kWp), 900 kWh je kWp und Jahr, den Jahresmarktwert Solar 2025 (4,508 ct/kWh) als Szenario und den Abzugsbetrag 2027 (0,601 ct/kWh). Das ist eine Rechnung, keine Prognose für 2027.

## Werkzeuge

Rohdaten als Tabelle liefert der Apify-Actor [pegton/mastr-energy-units](https://apify.com/pegton/mastr-energy-units). Aufbereitete Auszüge und Ratgeber liegen im [Gumroad-Shop von pegton](https://pegton.gumroad.com/). Der Verkauf der Produkte läuft über Gumroad (Merchant of Record).

Diese Website erhebt keine eigenen Nutzungsstatistiken und setzt keine Tracking-Cookies. Sie wird über GitHub Pages ausgeliefert. Beim Aufruf können technische Verbindungsdaten beim Hoster anfallen; dafür gelten die Bedingungen von GitHub.
