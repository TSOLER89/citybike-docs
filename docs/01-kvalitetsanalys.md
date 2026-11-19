# Kvalitetsanalys CityBike

## 1. Observationer och egenskaper

### K1

Egenskap: Performance efficiency
Delegenskap: Time behaviour
Motivering: Kartan tar 9 sekunder att ladda vid 300 samtidiga användare, vilket visar på problem med systemets svarstid.

### K2

Egenskap: Reliability
Delegenskap: Fault tolerance
Motivering: Systemet hanterar inte upprepade betalningsbegäranden vid dålig nätverksanslutning, vilket leder till dubbeldebitering och påverkar även betalningsfunktionens korrekthet.

### K3

Egenskap: Reliability
Delegenskap: Fault tolerance
Motivering: Systemet ska kunna fortsätta fungera vid förlorad mobiltäckning och skicka sparade positioner när anslutningen återkommer.

### K4

Egenskap: Interaction capability
Delegenskap: Learnability
Motivering: Nya kunder har svårt att hitta funktionen för att avsluta resan, vilket leder till 40 samtal till kundtjänst varje vecka.

### K5

Egenskap: Security
Delegenskap: Confidentiality
Motivering: En inloggad kund kan se andra kunders resor genom att ändra id i adressen, vilket innebär att obehöriga får tillgång till information.

### K6

Egenskap: Maintainability
Delegenskap: Modularity
Motivering: Prisberäkningen och bokningsflödet ligger i samma klass, vilket gör det svårt att ändra prisregler utan att riskera att påverka bokningsfunktionen.

### K7

Egenskap: Interaction capability
Delegenskap: Operability
Motivering: Adminpanelen är svår att använda på surfplattor eftersom knapparna är för små och innehållet går utanför skärmen.

### K8

Egenskap: Safety
Delegenskap: Hazard warning
Motivering: Om batteritemperaturen överstiger 60 grader ska systemet avbryta laddningen och varna teknikerna för att förhindra farliga situationer.

### K9

Egenskap: Compatibility
Delegenskap: Interoperability
Motivering: API:et måste kunna ta emot och hantera positionsdata från både nuvarande och framtida styrenheter trots att de använder olika dataformat.

### K10

Egenskap: Flexibility
Delegenskap: Adaptability
Motivering: Systemet är svårt att anpassa till nya städer eftersom namn, priser och zoner är hårdkodade och kräver ändringar på tolv ställen.

## 2. De fyra viktigaste egenskaperna för CityBike

1. Safety: Ett överhettat batteri kan orsaka brand eller
   personskador om laddningen inte avbryts, se K8 och avsnitt 7.

2. Security: Kundernas personuppgifter och reseinformation
   riskerar att exponeras genom bristande behörighetskontroll,
   se K5 och R4 i avsnitt 6.

3. Reliability: Systemet måste hantera förlorad mobiltäckning
   så att resor kan fortsätta och positionsdata skickas senare,
   se K3 och avsnitt 4.

4. Functional suitability: Felaktig betalningshantering kan
   leda till att kunder debiteras flera gånger för samma resa,
   se K2 och avsnitt 2.

## 3. Mätbara krav

### Krav 1: Safety

Krav: När batteritemperaturen överstiger 60 °C
ska laddningen avbrytas inom 60 sekunder
och ett larm skickas till teknikerna.

Mått: Tid från att temperaturen överstiger
60 °C tills laddningen avbryts.

Gränsvärde: Högst 60 sekunder.

Mätmetod: Simulera en batteritemperatur över
60 °C och mäta tiden tills laddningen avbryts.
Kontrollera även att larmet skickas.

Hänvisning: K8, avsnitt 7.

### Krav 2: Security

Krav: Systemet ska neka 100 % av testade försök
att läsa andra kunders resor genom att ändra
resans ID i API-anropet.

Mått: Andel obehöriga åtkomstförsök som nekas.

Gränsvärde: 100 % av testade obehöriga försök.

Mätmetod: Genomför API-tester med två olika
kundkonton och kontrollera att en kund inte
kan läsa den andra kundens resor.

Hänvisning: K5, avsnitt 4.

### Krav 3: Reliability

Krav: Systemet ska överföra minst 99 % av sparade
GPS-positioner inom 30 sekunder efter att
uppkopplingen återkommer.

Mått: Andel överförda GPS-positioner och tiden
det tar att överföra dem.

Gränsvärde: Minst 99 % inom 30 sekunder.

Mätmetod: Simulera ett avbrott i mobilanslutningen
under en resa. Återställ anslutningen och kontrollera
hur många GPS-positioner som överförs till API:et
inom 30 sekunder.

Hänvisning: K3, avsnitt 4.

### Krav 4: Functional suitability

Krav: Systemet ska genomföra exakt en debitering
per resa även om samma betalningsbegäran skickas
10 gånger inom 60 sekunder.

Mått: Antal genomförda debiteringar per resa.

Gränsvärde: Exakt 1 debitering vid 10 upprepade
betalningsbegäranden inom 60 sekunder.

Mätmetod: Genomför ett API-test där samma
betalningsbegäran skickas 10 gånger inom
60 sekunder. Kontrollera att endast en
debitering registreras hos betaltjänsten.

Hänvisning: K2, avsnitt 2 och 4.

### Beräkning av tillåten nedtid

Krav på tillgänglighet: 99,7 % under 30 dagar.

Total tid: 30 × 24 × 60 = 43 200 minuter.

Tillåten nedtid: 100 % − 99,7 % = 0,3 %.

Beräkning: 43 200 × 0,003 = 129,6 minuter.

Svar: API:et får vara otillgängligt i högst
129,6 minuter under en period på 30 dagar.

## 4. Skillnad mot ett annat par
