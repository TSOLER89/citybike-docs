# Testnivåkarta CityBike

## T1

Nivå: enhet
Motivering: Testet kontrollerar att PriceCalculator beräknar rätt pris för en resa på 18 minuter utan att använda databas eller andra systemdelar, därför gäller fråga fyra.

## T2

Nivå: enhet
Motivering: Testet kontrollerar att PriceCalculator följer prisregeln om max 250kr utan att använda databas eller andra systemdelar, därför gäller fråga fyra.

## T3

Nivå: integration
Motivering: Testet kontrollerar att API:et och en riktig SQL Server-databas fungerar tillsammans genom att starta en resa och skapa en rad i Trips, därför gäller fråga tre.

## T4

Nivå: enhet
Motivering: Testet kontrollerar att BookingService anropar upplåsningsklienten exakt en gång. Eftersom klienten ersätts av en testdubbel testas BookingService isolerat, därför gäller fråga fyra.

## T5

Nivå: system
Motivering: Testet kontrollerar hela upplåsningsflödet
från kundappen via API:et till en simulerad styrenhet
och tillbaka, därför gäller fråga två.

## T6

Nivå: acceptans
Motivering: Tre pilotkunder testar att registrera sig, boka och avsluta en resa utan hjälp. Projektledaren godkänner resultatet, därför gäller fråga ett.

## T7

Nivå: integration
Motivering: Testet kontrollerar att GpsPoints-repositoryt kan spara och läsa tillbaka koordinater från en riktig SQL Server-databas, därför gäller fråga tre.

## T8

Nivå: system
Motivering: Testet kontrollerar systemets svarstid
vid 500 samtidiga användare i en driftlik
testmiljö, därför gäller fråga två.

## T9

Nivå: enhet
Motivering: Testet kontrollerar att AgeValidator.IsAdult
returnerar false för en person som ännu inte fyllt
18 år. Endast en metod testas utan andra systemdelar,
därför gäller fråga fyra.

## T10

Nivå: acceptans
Motivering: Ekonomiansvarig granskar och godkänner att kvittot innehåller rätt information och följer prisreglerna P1–P3, därför gäller fråga ett.

## T11

Nivå: integration
Motivering: Testet kontrollerar att API:ets HTTP-hantering och autentisering fungerar tillsammans i en testserver och returnerar 401 för en användare som inte är inloggad, därför gäller fråga tre.

## T12

Nivå: system
Motivering: Testet kontrollerar hela kundresan från registrering till betalning genom flera systemdelar i en testmiljö, därför gäller fråga två.

## T13

Nivå: enhet
Motivering: Testet kontrollerar att EmailValidator.IsValid
avvisar en ogiltig e-postadress utan att använda
databas eller andra systemdelar, därför gäller fråga fyra.

## T14

Nivå: acceptans
Motivering: Kundtjänst testar tolv användarfall
i adminpanelen och godkänner resultatet genom
att skriva under ett protokoll, därför gäller fråga ett.

## T15

Nivå: integration
Motivering: Testet kontrollerar att StationService och databasen fungerar tillsammans och returnerar rätt antal lediga cyklar, därför gäller fråga tre.

## T16

Nivå: system
Motivering: Testet kontrollerar hela firmwareuppdateringsflödet med 20 styrenheter i en testrigg och verifierar att upplåsning och positionsrapportering fungerar efter uppdateringen, därför gäller fråga två.

## Sammanställning

Enhet: 5
Integration: 4
System: 4
Acceptans: 3

Totalt: 16 testfall.

Fördelningen liknar testpyramiden eftersom enhetstesterna är flest, men skillnaden mellan nivåerna är liten. I en typisk testpyramid finns betydligt fler enhetstester än system och acceptanstester.

## Eget testfall

Krav: Krav 2 – Security (docs/01-kvalitetsanalys.md)

Givet: Två kundkonton finns i testmiljön och
kund A är inloggad. Minst 100 resor tillhör
andra kunder.

När: Kund A skickar 100 API-anrop för att läsa
resor som tillhör andra kunder genom att
ändra resans ID.

Då: API:et ska neka 100 % av de 100 obehöriga
förfrågningarna utan att lämna ut några
reseuppgifter.

Nivå: integration

Testtyp: Funktionellt säkerhetstest, svartlåda.

## Kontrollfrågor

### 1. Felaktigt pris för 61 sekunder

Felet hittas billigast med ett enhetstest eftersom
vi kan testa prisberäkningen direkt utan andra
systemdelar. T1 ligger närmast eftersom det testar
PriceCalculator, men vi behöver lägga till ett
test för 61 sekunder där rätt pris ska vara 15 kr.

### 2. Svåraste testfallet

T5 var svårast att placera eftersom jag tvekade mellan
integrationstest och systemtest. Jag tänkte först att det
var ett integrationstest eftersom flera komponenter
kommunicerar med varandra, men eftersom hela
upplåsningsflödet testas passar systemtest bättre.
