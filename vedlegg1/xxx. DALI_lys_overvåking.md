# xx. DALI lys overvåking
Det er et overordnet objekt som visser status og alarmer, styring skjer via objekt 20, Lysstyring.
DALI = Digital Addressable Lighting Interface
Det skal være en per type DALI gruppe, normalt vil dette være:

-	Innkjøringsbelysing portal 1
-	Innkjøringsbelysing portal 2
-	Innvending belysing
-	Sikkehetsbelysing

En armatur er definert som en eller flere drivere. Visst en armatur består av flere drivere skal styresystemet slå disse i sammen til en enhet.

## Status

|    Bit    |    Maske    |    Høy                                                      |    Lav                         |
|-----------|-------------|-------------------------------------------------------------|--------------------------------|
|    0      |    1        |    Trinn 1                                                  |    Trinn   0(av)               |
|    1      |    2        |    Trinn 2                                                  |    Trinn   0(av)               |
|    2      |    4        |    Trinn 3                                                  |    Trinn   0(av)               |
|    3      |    8        |    Trinn 4                                                  |    Trinn   0(av)               |
|    4      |    16       |    Trinn 5                                                  |    Trinn 0(av)                 |
|    5      |    32       |    Alarmer   for % feilete armaturer, blokkert              |    Frigitt/OK                  |
|    6      |    64       |    Høy alarm   for % feilete armaturer                      |    Normalt,   eller bit 7      |
|    7      |    128      |    Kritisk   høy alarm for % feilete armaturer              |    Normalt, eller   bit 6      |
|    8      |    256      |    Høy alarm   for brenntid, blokkert                       |    Frigitt                     |
|    9      |    512      |    Høy alarm   for brenntid                                 |    Normalt                     |
|    10     |    1024     |    Alarmer   for % feilete DALI kontrollere, blokkert       |    Frigitt                     |
|    11     |    2048     |    Høy alarm   for % feilete DALI kontrollere.              |    Normalt,   eller bit 12     |
|    12     |    4096     |    Kritisk   høy alarm for % feilete DALI kontrollere.      |    Normalt,   eller bit 11     |
|    13     |    8192     |    Alarmer   for % kommunikasjon til armaturer, blokkert    |    Frigitt                     |
|    14     |    16384    |    Høy alarm   for % kommunikasjon til armaturer            |    Normalt,   eller bit 15     |
|    15     |    32768    |    Kritisk   høy alarm for % kommunikasjon til armaturer    |    Normalt, eller   bit 14     |

Bit «0» til «4» er lagt til for å gi en visuell fremstilling av nå verdi.

Bit «6» og «7» er armaturer som har feilet er definert som:

•	Lampen har ikke antennes etter 20min.
•	Roterende lampe status (AV, PÅ, AV, PÅ …)
•	Glødene eller buene lampe oppdaget
•	Lampens end(avsluttet) levetid tid spenning detektert.

Bit «9» er forvarsel på at mange armaturer kan gi seg.

Bit «11» og «12» er alarmer som går på feilete PLS med DALI utganger eller dedikerte DALI kontrollere. 

Bit «14» og «15» er armaturer som en DALI kontrollere har mistet kontakt med. Tallet skal ikke inkludere armaturer som er koblet opp mot DALI kontroller som PLS ikke har kontakt med.

Kommunikasjonssvikt er definert som at DALI kontroller ikke har kommunikasjon ut til armatur.

### Trinn indikering

- Trinn 1:	Gjelder fra og med 1% til og med det som definert som trinn 1.
- Trinn 2:	Fra 1% mer enn det som er definert for trinn 1, til det % som er definert for trinn 2.
- Trinn 3:	Fra 1% mer enn det som er definert for trinn 2, til det % som er definert for trinn 3.
- Trinn 4:	Fra 1% mer enn det som er definert for trinn 3, til det % som er definert for trinn 4.
- Trinn 5:	Fra 1% mer enn det som er definert for trinn 4, til det % som er definert for trinn 5.

## Kommando

|    Bit    |    Maske    |    Høy                                                     |
|-----------|-------------|------------------------------------------------------------|
|    0      |    1        |    Blokker, Alarmer   for % feilete armaturer              |   
|    1      |    2        |    Frigi, Alarmer for %   feilete armaturer                |    
|    2      |    4        |    Blokker,   Høy alarm for brenntid                       |    
|    3      |    8        |    Frigi, Høy   alarm for brenntid                         |   
|    4      |    16       |    Blokker,   Alarmer for % feilete DALI kontrollere       |    
|    5      |    32       |    Frigi,   Alarmer for % feilete DALI kontrollere         |    
|    6      |    64       |    Blokker,   Alarmer for % kommunikasjon til armaturer    |    
|    7      |    128      |    Frigi,   Alarmer for % kommunikasjon til armaturer      |
|    8      |    256      |    Blokker   alle alarmer                                  |    
|    9      |    512      |    Frigi alle   alarmer                                    |    
|    10     |    1024     |                                                            |   
|    11     |    2048     |                                                            |   
|    12     |    4096     |                                                            |   
|    13     |    8192     |                                                            |    
|    14     |    16384    |                                                            |    
|    15     |    32768    |    Reset time   tellere                                    |    

## Verdi

Timetellere skal være 32 bit. Dersom kun 16 bits tellere støttes skal time tellingen bestå av 2 tellere. En for antall 10.000 timer og en som nullstilles etter 9.999. Ved kommando bit 15, reset alle time tellere.

|    Verdi                                          |    Beskrivelse                                                                                                                                                                                          |
|---------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Nå verdi   dimmenivå                           |    Tilbakemelding   fra armatur om faktisk dimmenivå                                                                                                                                                    |
|    Antall   feilete armaturer                     |    Antall   DALI enheter med feil.                                                                                                                                                                      |
|    % armaturene   som feilet.                     |    % av de   totale armaturene som er en del av gruppen som har feil.                                                                                                                                   |
|    Konstant   lys utbytte dimme verdi.            |    Ved bruk   av LED så er det lys nivå redusert ved installasjon, men økes gradvis i takt   med redusert lys utbyttet i armatur.   Beregnet   verdi, basert på timeteller og tabell fra leverandør.    |
|    Gjenværende   levetid på LED, %                |    Kalkulert   med bruk av brenntid og LEDs maks levetid.                                                                                                                                               |
|    Antall   feilete DALI kontrollere.             |    DALI   kontrollere, eller utgang på PLS.                                                                                                                                                             |
|    Antall   armaturer med kommunikasjons svikt    |    Kommunikasjonssvikt   med armatur.                                                                                                                                                                   |
|    %   armaturer med kommunikasjons svikt         |    Kommunikasjonssvikt   med armatur.                                                                                                                                                                   |
|    Timeteller   Trinn 1                           |    Gjelder   fra og med 1% til og med det som definert som trinn 1                                                                                                                                      |
|    Timeteller   Trinn 2                           |    Fra 1%   mer enn det som er definert for   trinn 1, til det % som er definert for trinn   2.                                                                                                         |
|    Timeteller   Trinn 3                           |    Fra 1%   mer enn det som er definert for   trinn 2, til det % som er definert for trinn   3.                                                                                                         |
|    Timeteller   Trinn 4                           |    Fra 1%   mer enn det som er definert for   trinn 3, til det % som er definert for trinn   4.                                                                                                         |
|    Timeteller   Trinn 5                           |    Fra 1%   mer enn det som er definert for   trinn 4, til det % som er definert for trinn   5.                                                                                                         |

## Parameter

|    Parameter                                                            |    Beskrivelse                                                                                                                                                                                                                                                        |
|-------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    Kritisk høy alarm, % feilete   armaturer, grenseverdi                |    Setpunkt   for alarm                                                                                                                                                                                                                                               |
|    Høy alarm, brenntid, grenseverdi                                     |    Setpunkt   for alarm                                                                                                                                                                                                                                               |
|    Høy alarm, % feilete DALI   kontrollere, grenseverdi                 |    Setpunkt   for alarm                                                                                                                                                                                                                                               |
|    Kritisk høy alarm, % feilete DALI   kontrollere, grenseverdi         |    Setpunkt   for alarm                                                                                                                                                                                                                                               |
|    Høy alarm,   % kommunikasjon til armaturer,   grenseverdi            |    Setpunkt   for alarm                                                                                                                                                                                                                                               |
|    Kritisk   høy alarm, % kommunikasjon til armaturer,   grenseverdi    |    Setpunkt   for alarm                                                                                                                                                                                                                                               |
|    Dimmetid   mellom 2 punkt                                            |    (DALI fade time) tiden det skal   ta å endre å gå fra en verdi til enn annen. Oppgitt i sekunder med 1 komma.    fra 0 til 90,5s, avrundes til nærmeste gyldige verdi.   Denne Parameter bør være med   frem det er definert en vegvesen standard for dimmetid.    |

