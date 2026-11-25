# Regelverkschecklista för CityBike

## GDPR

### G1 Radering av kundens uppgifter

Gäller: Ja

Skäl: Art. 17, kunden har rätt att få uppgifterna raderade.

Ansvarig: Backend-utvecklare

Verifieras med: Nivå integration, testtyp funktionell

Bevis: En testrapport som visar att uppgifter som ska raderas verkligen har tagits bort från databasen.

### G2 Behörighetskontroll

Gäller: Ja

Skäl: Enligt artikel 32 måste CityBike skydda kundernas personuppgifter. Kundtjänst ska inte kunna se mer information än de behöver.

Ansvarig: Backend-utvecklare

Verifieras med: Nivå integration, testtyp funktionell

Bevis: En testrapport som visar att kundtjänst inte kan se kundernas personnummer.

### G3 Säker loggning

Gäller: Ja

Skäl: Enligt artikel 32 och 5.1 c ska CityBike skydda personuppgifter och inte spara mer information än nödvändigt i loggarna.

Ansvarig: Backend-utvecklare

Verifieras med: Nivå integration, testtyp funktionel

Bevis: En testlogg som visar att lösenord och personnummer inte sparas i loggfilerna.

### G4 Information till kunder

Gäller: Ja

Skäl: Enligt artikel 13 måste CityBike informera kunderna om hur deras personuppgifter används och varför de samlas in.

Ansvarig: Frontend-utvecklare och dataskyddsansvarig

Verifieras med: Nivå system, testtyp funktionell

Bevis: En testrapport och skärmbilder som visar att informationen finns i appen vid registrering.

### G5 Säker lösenordshantering

Gäller: Ja

Skäl: Enligt artikel 32 måste CityBike skydda kundernas lösenord. SHA-256 utan salt ger inte tillräckligt skydd.

Ansvarig: Backend-utvecklare

Verifieras med: Nivå integration, testtyp funktionell

Bevis: En testrapport som visar att lösenorden lagras med en säker hashmetod och unikt salt.

## CE och produktkrav

### C1 Maskindirektivet och maskinförordningen

Gäller: Ja

Skäl: CityBike använder elcyklar med motorstöd.Därför behöver cyklarna uppfylla tillämpliga säkerhetskrav.

Ansvarig: Produktansvarig

Verifieras med: Nivå system, testtyp icke-funktionell

Bevis: Testrapport som visar att cykelns säkerhetsfunktioner har kontrollerats.

### C2 Standarden EN 15194

Gäller: Osäkert

Skäl: EN 15194 gäller elcyklar med motorstöd, men har ett undantag för uthyrning från obemannade stationer. CityBike hyr ut cyklar via en app, så vi behöver kontrollera om undantaget gäller.

Ansvarig: Produktansvarig och certifieringsspecialist

Verifieras med: Nivå system, testtyp icke-funktionell

Bevis: Dokumenterad bedömning av standardens tillämplighet och relevanta säkerhetstester.

### C3 Radioutrustningsdirektivet 2014/53/EU

Gäller: Ja

Skäl: Styrenheten har ett LTE-M-mobilmodem som kommunicerar via mobilnätet. Därför måste den uppfylla kraven för radioutrustning.

Ansvarig: Produktansvarig

Verifieras med: Nivå system, testtyp icke-funktionell

Bevis: Testrapport från radioprovning som visar att styrenheten uppfyller kraven.

### C4 Batterikrav – EN 50604-1 och EU 2023/1542

Gäller: Osäkert

Skäl: Batteriförordningen gäller CityBikes batterier, men vi vet inte vilken batterityp cyklarna använder. Därför behöver vi kontrollera om standarden EN 50604-1 är tillämplig.

Ansvarig: Produktansvarig och provningslaboratorium

Verifieras med: Nivå system, testtyp icke-funktionell

Bevis: Testrapport som visar att batteriets säkerhetsfunktioner har testats.

### C5 Elektromagnetisk kompatibilitet (EMC)

Gäller: Ja

Skäl: CityBikes styrenhet innehåller elektroniska komponenter som kan störa annan utrustning. Därför behöver styrenheten uppfylla EMC-kraven enligt radioutrustningsdirektivet.

Ansvarig: Produktansvarig

Verifieras med: Nivå system, testtyp icke-funktionell

Bevis: En testrapport från EMC-provning som visar att styrenheten uppfyller kraven.

## Cyber Resilience Act

### CRA1 Förteckning över programvarukomponenter (SBOM)

Gäller: Ja

Skäl: CityBike ansvarar för styrenhetens firmware och behöver dokumentera vilka komponenter och versioner som används för att kunna upptäcka säkerhetsbrister.

Ansvarig: Firmware-utvecklare

Verifieras med: Nivå integration, testtyp icke-funktionell

Bevis: En SBOM-fil med komponenter och versioner samt en rapport som visar att innehållet har kontrollerats.

### CRA2 Säkerhetsuppdateringar av firmware

Gäller: Ja

Skäl: CityBike måste kunna uppdatera styrenhetens firmware på ett säkert sätt för att åtgärda säkerhetsbrister.

Ansvarig: Firmware-utvecklare

Verifieras med: Nivå system, testtyp icke-funktionell

Bevis: En testrapport som visar att en godkänd firmwareuppdatering kan installeras och att en manipulerad uppdatering nekas.

### CRA3 Hantering av sårbarheter

Gäller: Ja

Skäl: CityBike behöver ha en rutin för att ta emot och hantera säkerhetsbrister i styrenhetens firmware.

Koppling till R11: CityBike använder SHA-256 utan salt för lösenord. Det är en säkerhetsbrist som behöver åtgärdas. R11 gäller backend-systemet, så vi behöver också bedöma om den delen omfattas av CRA eller främst av GDPR.

Ansvarig: Säkerhetsansvarig och firmware-utvecklare

Verifieras med: Nivå acceptans, testtyp icke-funktionell

Bevis: En rapport som visar hur en säkerhetsbrist har undersökts och åtgärdats.

## Spårbarhet

### K1 Batteriet får inte överstiga 60 °C vid laddning

Källa: C4 – Batterikrav och K8 i CityBikes kvalitetsanalys.
Testfall: Simulera att batteritemperaturen stiger över 60 °C. Kontrollera att styrenheten avbryter laddningen inom 5 sekunder och att ett larm når teknikerna inom 60 sekunder.
Nivå och testtyp: Systemtest, icke-funktionellt säkerhetstest.
Resultat som ska sparas: Temperatur, tiden det tog att avbryta laddningen, tiden till larm och om testet blev godkänt. Även firmwareversionen ska dokumenteras.
Teknisk dokumentation: Testrapport som sparas i CityBikes tekniska dokumentation tillsammans med testloggar och information om firmwareversionen.

### K2 Säker lösenordshantering

Källa: G5 – Säker lösenordshantering, R11 och GDPR artikel 32.

Testfall: Registrera två testkonton med samma lösenord. Kontrollera att lösenorden inte sparas i klartext eller med vanlig SHA-256 utan salt. Kontrollera också att båda användarna kan logga in.

Nivå och testtyp: Integrationstest, funktionellt test.

Resultat som ska sparas: Testresultat, om båda inloggningarna fungerade och om lösenorden lagrades med olika saltade hashvärden. Dokumentera även vilken version av backend som testades.

Teknisk dokumentation: En testrapport som sparas tillsammans med testloggar och information om backend-versionen.

## Kontrollfrågor

### 1. Varför räknas CityBike som tillverkare?

CityBike räknas som tillverkare eftersom styrenheten tillverkas under deras eget varumärke. Även om en underleverantör bygger den är det CityBike som ansvarar för att produkten uppfyller säkerhetskraven.

### 2. Vad händer med testningen vid en ny firmwareversion?

Varje gång CityBike släpper en ny firmwareversion behöver den testas. Vi måste kontrollera att
uppdateringen fungerar och att tidigare funktioner, som låsning, GPS och batteriövervakning, fortfarande fungerar som de ska. Det kallas regressionstestning.

### 3. Vilket fält gör att en inspektör kan lita på testresultatet?

Resultat som ska sparas är viktigt eftersom det visar vad testet gav för resultat och vilken version som testades. Då kan en inspektör kontrollera testet även flera år senare. Den tekniska dokumentationen visar var resultaten finns sparade.
