# GDPR-avvikelser i CityBike

## Personuppgiftsinventering

- Users: Id, Name, Personnummer, Email, Phone,PasswordHash, Status, CreatedAt.
  Uppgifterna identifierar kunden direkt eller är kopplade till kundens konto.

- PaymentMethods: Id, UserId, ProviderToken, CardLast4,ExpiryMonth, ExpiryYear.
  Uppgifterna kan kopplas till en identifierbar kund genom UserId och beskriver kundens betalningsmetod.

- Bikes: Id, StationId, LastSeenAt. Uppgifterna kan indirekt kopplas till en kund genom Trips.BikeId och Trips.UserId.

- Stations: Inga personuppgifter direkt. Tabellen beskriver fasta cykelstationer och innehåller inga uppgifter om enskilda personer.

- Trips: Id, UserId, BikeId, StartStationId, EndStationId, StartedAt, EndedAt, PriceSek.
  Uppgifterna beskriver en kunds resa och kan kopplas till personen genom UserId.

- GpsPoints: Id, TripId, Latitude, Longitude,RecordedAt.
  GPS-positionerna kan kopplas till en kund genom TripId och Trips.UserId.

## A1

Rutin: R1

Vad: CityBike kräver personnummer vid registrering trots att åldern kan kontrolleras på ett annat sätt.
Artikel: Art. 5.1 c, dataskyddslagen 3 kap. 10 §
Risk: Hög
Åtgärd: Ta bort kravet på personnummer och kontrollera kundens ålder med en mindre integritetskänslig metod.
Testfall: Registrera en kund utan personnummer och kontrollera att registreringen fungerar (systemtest).

## A2

Rutin: R2

Vad: CityBike sparar kundernas resor och GPS-positioner utan någon bestämd tidsgräns, trots att uppgifterna inte behöver sparas för alltid.
Artikel: Art. 5.1 e, Lagringsminimering
Risk: Hög
Åtgärd: Inför tydliga lagringstider för resor och GPS-positioner samt automatisk gallring när uppgifterna inte längre behövs.
Testfall: Skapa GPS-positioner med datum äldre än den fastställda lagringstiden och kontrollera att de tas bort automatiskt (integrationstest).

## A3

Rutin: R3

Vad: När en kund avslutar sitt konto ändras bara statusen till Inactive, medan alla personuppgifter fortfarande finns kvar utan att behovet av lagring bedöms.
Artikel: Art. 17 – Rätt till radering
Risk: Hög
Åtgärd: Inför en funktion som hanterar begärandenom radering och tar bort personuppgifter som inte längre behöver sparas.
Testfall: Avsluta ett kundkonto och begär radering. Kontrollera att uppgifter utan fortsatt lagringsgrund raderas från databasen (integrationstest).

## A4

Rutin: R4

Vad: Kundtjänst använder ett gemensamt konto och delar lösenord i chatten. Alla har dessutom tillgång till personuppgifter som de inte behöver för sitt arbete.
Artikel: Art. 32 och Art. 5.1 f
Risk: Hög
Åtgärd: Inför personliga konton, multifaktorautentisering(MFA) och rollbaserad behörighetskontroll så att kundtjänst bara kan se nödvändiga uppgifter.
Testfall: Logga in med ett kundtjänstkonto och kontrollera att användaren inte kan se kolumnen Personnummer(integrationstest).

## A5

Rutin: R5

Vad: API:et loggar hela innehållet i varje anrop,vilket kan innebära att personnummer, lösenord och andra personuppgifter sparas i loggfiler.
Artikel: Art. 32 och Art. 5.1 c
Risk: Hög
Åtgärd: Begränsa loggningen till nödvändig information,
maskera personuppgifter och lösenord samt inför
en motiverad lagringstid för loggfiler.
Testfall: Skicka ett API-anrop med personuppgifter och kontrollera att lösenord och personnummer inte sparas i loggfilen (integrationstest).

## A6

Rutin: R6

Vad: CityBike skickar nyhetsbrev med erbjudanden till alla kunder utan att säkerställa en giltig rättslig grung eller möjlighet att tacka nej.
Artikel: Art. 6 och Art. 21.2–21.3
Risk: Medel
Åtgärd: Säkerställ en giltig rättslig grund,informera kunderna om marknadsföringen och ge dem möjlighet att tacka nej.
Testfall: Kontrollera att en kund som har motsatt sig direktmarknadsföring inte får några nyhetsbrev(integrationstest).

## A6

Rutin: R6

Vad: CityBike skickar nyhetsbrev med erbjudanden
till alla kunder utan att säkerställa en giltig
rättslig grund eller möjlighet att tacka nej.
Artikel: Art. 6 och Art. 21.2–21.3
Risk: Medel
Åtgärd: Säkerställ en giltig rättslig grund,informera kunderna om marknadsföringen och ge dem möjlighet att tacka nej.
Testfall: Kontrollera att en kund som har motsatt sig direktmarknadsföring inte får några nyhetsbrev (integrationstest).

## A8

Rutin: R8
Vad: CityBike informerar inte kunderna om hur deras personuppgifter samlas in, används och lagras.
Artikel: Art. 13 – Informationsskyldighet
Risk: Medel
Åtgärd: Lägg till en tydlig integritetspolicy i appen som förklarar vilka personuppgifter som behandlas, varför de behövs och vilka rättigheter kunderna har.
Testfall: Registrera ett nytt konto och kontrollera att information om personuppgiftsbehandlingen finns tillgänglig under registreringen (systemtest).

## A9

Rutin: R8

Vad: CityBike saknar en förteckning över hur företaget behandlar personuppgifter, vilket gör det svårt att visa att GDPR följs.
Artikel: Art. 30 Register över behandlingar
Risk: Medel
Åtgärd: Skapa och underhåll en förteckning över personuppgiftsbehandlingar med information om ändamål, kategorier av uppgifter, mottagare och lagringstider.
Testfall: Granska förteckningen och kontrollera att alla CityBikes personuppgiftsbehandlingar är dokumenterade (acceptanstest).

## A10

Rutin: R11

Vad: CityBike lagrar lösenord med SHA-256 utan salt,
vilket gör det lättare för angripare att knäcka
lösenorden om databasen läcker.

Artikel: Art. 32 – Säkerhet vid behandling

Risk: Hög

Åtgärd: Ersätt SHA-256 med en säker metod för
lösenordshashning, exempelvis ASP.NET Core Identity
PasswordHasher, som använder salt och en
lösenordsanpassad hashfunktion.

Testfall: Registrera två användare med samma lösenord
och kontrollera att olika saltade hashvärden lagras
i databasen och att båda kan logga in (integrationstest).

## Incident R10

Tidpunkt för kännedom: tisdag 3 mars 2026 kl. 14:20
Frist för anmälan (datum och klockslag): fredag 6 mars 2026 kl. 14:20
Anmäla till IMY: JA
Informera berörda kunder: Nej, inte utifrån den information som finns.
Motivering: Incidenten bör anmälas till IMY eftersom personuppgifter om 2 100 kunder har skickats till en obehörig mottagare. Risken för exempelvis nätfiske och oönskade kontakter kan inte uteslutas.
Däremot bedöms risken inte som tillräckligt hög för att information till kunderna ska vara obligatorisk
enligt artikel 34. Bedömningen behöver omprövas om det visar sig att uppgifterna har spridits vidare.

Åtgärder för att förhindra upprepning:

1. Inför en varning när e-postmeddelanden med personuppgifter skickas till externa mottagare. Användaren måste kontrollera mottagaradressen innan meddelandet skickas.

2. Begränsa möjligheten att exportera kunduppgifter genom rollbaserad behörighetskontroll. Endast behörig personal ska kunna exportera större mängder personuppgifter.

## Kontrollfrågor

### 1. När får personnummer användas?

Personnummer får användas när det finns ett tydligt behov av säker identifiering. CityBike använder personnummer för ålderskontroll och påminnelsefakturor, men behöver visa varför hela personnumret är nödvändigt. Åldern kan exempelvis kontrolleras på ett annat sätt.

### 2. Varför räcker inte Inactive?

Att ändra Users.Status till Inactive räcker inte eftersom kundens personuppgifter fortfarande finns kvar i databasen. Enligt GDPR artikel 17 måste CityBike kunna hantera begäranden om radering och ta bort uppgifter som inte längre behöver sparas, om det inte finns en giltig anledning att behålla dem.

### 3. Vilken testnivå används för behörighetskontroll?

Integrationstest passar bäst om vi testar API:ets behörighetskontroll tillsammans med dataåtkomsten.
