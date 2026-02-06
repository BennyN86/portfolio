# Uge 36 - Introduktion til faget

### Opgave 3 - OWASP Top 10: Injection Sårbarheder
***Gruppeøvelse***

#### Formål

I denne øvelse vil du dykke ned i OWASP Top 10's kategori nummer 3, Injection. Målet er at forstå, hvad injection-sårbarheder er, hvordan de kan udnyttes, og hvilke sikkerhedsforanstaltninger der kan beskytte mod dem. Herudover er det en generel introduktion til begreber og termanologier.

#### Instruktioner

1. **Forstå Injection-sårbarheder**  
    Gå til [OWASP Top 10 Injection](https://owasp.org/Top10/A03_2021-Injection/) og læs beskrivelsen af injection-sårbarheder.  
    Opgave: Forklar med dine egne ord, hvad en injection-sårbarhed er, og hvordan den typisk kan udnyttes i webapplikationer. Giv eksempler på forskellige typer af injection-angreb, såsom SQL injection og command injection.

    >Injektion sårbarheder opstår når en trusselsaktør opnår upassende adgang til data, for eksempelvis igennem et søgefelt eller api kald, hvor der hverken bliver inputvalideret. Derved kan brugeren muligvis redigere objekter i backend, til eget truende formål. 
    
    >En injektion kan fx være: Aktør beder et api kald om ikke kun og hente sin egen data men hele tabellen, så de kan se andres data.

2. **Forståelse af Positive Server-Side Input Validation**
    Læs afsnittet How to Prevent på OWASP-siden om injection.  
    Opgave: Forklar, hvad der menes med "positive server-side input validation" som en metode til at forhindre injection-angreb. Hvorfor er denne metode effektiv, og hvordan adskiller den sig fra "negative validation" (sortlisting)?
    
    >Inputs skal leve op til en validering fra serverens side, for at kunne godkendes.(allowlist).
    
    >Derimod definerer en blokliste en liste over ugyldige data, som afvises specifikt. Tilladelseslister (allowlisting) er en foretrukken metode, fordi det normalt er lettere at definere eller opremse alle mulige gyldige input end at forsøge at definere alle de mulige ugyldige input.

3. **CWE-20 - Improper Input Validation**  
    Find afsnittet List of Mapped CWEs på OWASP injection-siden og læs om CWE-20.  
    Opgave: Beskriv, hvad CWE-20 "Improper Input Validation" dækker over. Hvorfor er korrekt inputvalidering vigtig for applikationssikkerhed?
    
    >CWE-20 dækker over det forhold, at en modtager(server) får noget data, som ikke bliver valideret(undersøgt eller tjekket) korrekt og at det dermed ikke fremgår, om den modtagne data har de rette parametre til at data kan behandles sikkert eller passende.

    >Korrekt inputvalidering er vigtigt for at sikre, at systemer ikke udleverer forkerte eller sårbare data, som brugere ikke har adgang til. Det sikrer også, at systemet kun kan foretage de handlinger, som er korrekte og f.eks. Ikke udfører handlinger, som brugerne ikke har ret til, f.eks. Sletter tabeller.

---

### Opgave 4 - OWASP Top 10: Insecure Design
***Gruppeøvelse***

#### Information

I denne øvelse skal du arbejde med [OWASP top 10 number 4 - Insecure design](https://owasp.org/Top10/A04_2021-Insecure_Design/). Inde på siden (og de tilhørende links) skal du forsøge at finde svar på de tre nedenstående spørgsmål.

Mange begreber der introduceres i denne øvelse, er ikke nødvendigvis nogen du tidligere har stødt på. Så det er helt okay hvis du endnu ikke kender dem. Det er en del af læringsprocessen. Vi arbejder videre med mange af begreberne senere i faget software sikkerhed.

#### Instruktioner

1. **Requirements and Resource Management**  
    Læs afsnittet Requirements and Resource Management på OWASP-siden.  
    Opgave: Hvad beskrives der overordnet i dette afsnit, og kender du en tilgang, som dækker dele af det beskrevne?

    >Vigtigheden af at undersøge tekniske krav til funktion og sikkerhed i starten af designfasen. Anvend Cis18 og undersøg hvilket kontroller skal implementeres. 

2. **Forståelse af Secure Design**  
    Opgave: Hvad menes der med "Secure design"? Forklar begrebet, og hvordan det adskiller sig fra andre tilgange til sikkerhed i softwareudvikling.
    
    >“Secure Design” er en metode hvor man fra starten fokuserer på at ens kode er robust og sikret mod kendte sårbarheder og angrebsteknikker.

3. Modellering i Secure Design  
    I afsnittet Secure Design bliver der nævnt en type modellering, som bør indgå i en udviklingsproces.  
    Opgave: Hvilken type modellering er der tale om, og hvorfor er den vigtig for sikker softwareudvikling?
    
    >OWASP Software Assurance Maturity Model (SAMM). SAMM består af forskellige domæner, som hver dækker nøgleaspekter af softwareudvikling, såsom governance, design, implementering og verifikation. Organisationer kan bruge SAMM til at identificere deres nuværende modenhedsniveau, sætte mål og udvikle en handlingsplan for at forbedre sikkerheden i deres software.

---

### Opgave 5 - ASVS
***Gruppeøvelse***

#### Information

I denne øvelse skal du besvare en række spørgsmål om [OWASP Application Security Verification Standard (ASVS)](https://github.com/OWASP/ASVS/tree/v4.0.3#latest-stable-version---403).  

#### Instruktioner

1. Input Validering  
Opgave: I hvilket afsnit kan man finde et overblik over, hvad man bør sikre sig i forhold til inputvalidering?

    >Afsnit “V5 Validation, Sanitization and Encoding”

2. V1 Architecture, Design and Threat Modelling  
Opgave: Hvad beskrives der i afsnittet V1 Architecture, Design and Threat Modelling? Giv en kort opsummering af de vigtigste punkter.

>Applikationers overordnede arkitektur og design.
>Kravene i afsnit V1 dækker områder som:

>* Definering af sikker arkitektur: At sikre, at applikationen er designet i overensstemmelse med anerkendte sikkerhedsprincipper.
>* Trusselsmodellering: At gennemføre en systematisk analyse af potentielle trusler og at tage skridt til at afbøde dem.
>* Designbeslutninger: At dokumentere sikkerhedsrelevante designbeslutninger og sikre, at de er velbegrundede.
>* Komponent- og tjenestevalidering: At sikre, at de valgte komponenter og tjenester er passende og sikre til deres tilsigtede formål.

3. Referencer til NIST  
    Opgave: I nogle afsnit refereres der til NIST. Hvad er NIST, og hvilken rolle spiller det i forhold til applikationssikkerhed?
    
>NIST (National Institute of Standards and Technology) er en amerikansk føderal institution, der udvikler teknologiske standarder, retningslinjer og bedste praksis for at fremme innovation og sikre konkurrenceevnen i den industrielle sektor. NIST spiller en vigtig rolle inden for applikationssikkerhed ved at udarbejde rammer og standarder, der hjælper organisationer med at beskytte deres informationsteknologi og applikationer mod sikkerhedstrusler.

>I forhold til applikationssikkerhed udgiver NIST vejledninger, såsom NIST Special Publication 800-serien, der omhandler sikkerhedskrav, kontroller og processer for at sikre applikationer mod sårbarheder. Et eksempel er NIST SP 800-53, som beskriver sikkerhedskontroller for føderale informationssystemer, og NIST SP 800-63, som dækker digitale identitetsretningslinjer. Disse vejledninger bruges ofte af organisationer over hele verden, ikke kun i USA, til at opbygge og forbedre sikkerhedsprogrammer i deres applikationer og IT-miljøer. ***(chatgpt)***

---

### Opgave 17 - Request for comments
***Gruppeøvelse***

#### Information

I denne øvelse skal i arbejde med forskellige RFC'er (Request for Comments) for at forstå standard specifikationer for diverse teknologierne. RFC'er er officielle dokumenter, der definerer internetstandarder, protokoller og teknologier. I skal læse udvalgte RFC'er og besvare spørgsmål om deres indhold og struktur.

Målet med øvelserne er ikke at forstå alle detaljer i RFC'erne, men at kunne finde og udlede overordnet relevant information om de beskrevne standarder.

Øvelsen fungere også som gruppe øvelse ift. gruppen arbejdes process. Hvordan løser gruppen bedst opgaven? er det F.eks. samlet gennemgang af RFC'erne, eller individuel gennemgang opfulgt at vidensdeling i gruppen? Tilgangen er op til jer, men det er vigtig at i afstemmer det i gruppen.

Vi samler op på alle besvarelser i klassen.

#### Instruktioner

##### Introduktion til RFC'er  
   Opgave: Læs introduktionen til RFC'er på [RFC Editorens hjemmeside](https://www.rfc-editor.org/). Beskriv med jeres egne ord, hvad en RFC er, og hvorfor den er vigtig for internettet.

##### HTTP/1.1: Semantics and Content (RFC 7231)  
    Gå til [RFC 7231 - HTTP/1.1: Semantics and Content](https://tools.ietf.org/html/rfc7231).  
    Opgave: Beskriv kort, hvad RFC 7231 omhandler. Hvilke HTTP-metoder er defineret i RFC 7231, og hvad er formålet med hver metode? Hvordan er metoderne struktureret i dokumentet, og hvordan relaterer de sig til HTTP-standarden?

>**RFC 7231 er nu erstattet af RFC 9110.**

>RFC 7231 omhandler semantikken omkring HTTP v1.1 meddelelser. Den definerer hvordan en request udformes, og fx hvilke metoder der kan anvendes, statuskoder osv.  
  
>Metoderne er defineret i nedenstående oversigt, efterfulgt af en udførlig beskrivelse af hver enkelt metode.:  
>![](https://lh7-rt.googleusercontent.com/docsz/AD_4nXe9-L2cSgGUy8ImjqMvoqp5xyXHnC3M2UeyH-Sj7x_qlYm8VtwPzVyXIdohAIQBez58AnpRMtXzgnZaSjJlBM4tKX_cXwfQZmAwA2ZKJie1w4HFM9Eq_fjxTXJUqASsQv3Qk-NixKFWGSj5jnhPDKZGIGs9?key=wpszcARQR39wODeqOPQxLw)

>Metoderne er struktureret i følgende grupper:  
>“Safe Methods”: GET, HEAD, OPTIONS og TRACE.
>“Idempotetent Methods”: PUT og DELETE
>“Cacheable Methods”: GET, HEAD og POST


##### Transport Layer Security (TLS) Protocol Version 1.3 (RFC 8446)
Gå til [RFC 8446 - TLS Protocol Version 1.3](https://datatracker.ietf.org/doc/html/rfc8446).

Opgave: Beskriv kort, hvad RFC 8446 omhandler. Hvad er TLS 1.3, og hvilke hovedkomponenter og funktioner beskrives i dokumentet? Hvordan er dokumentet struktureret, og hvilke sektioner fokuserer på de vigtigste aspekter af TLS 1.3?  
  
>RFC 8446 omhandler TLS 1.3 som er designet til at gøre det muligt at kommunikere  sikkert over internettet(mellem klienter og servere) uden at blive overvåget eller snydt undervejs(se: Forgery/Eavesdropping). 
Grundlæggende består TLS 1.3 af 2 hovedkomponenter: “Handshake Protocol” og en “Record Protocol”

A handshake protocol (Section 4) that authenticates the communicating parties, negotiates cryptographic modes and parameters, and establishes shared keying material.  The handshake protocol is designed to resist tampering; an active attacker should not be able to force the peers to negotiate different parameters than they would if the connection were not under attack.

A record protocol (Section 5) that uses the parameters established by the handshake protocol to protect traffic between the communicating peers.  The record protocol divides traffic up into a series of records, each of which is independently protected using the traffic keys.

Dokumentet er struktureret, så der først er en generel oversigt og introduktion, efterfulgt af et afsnit hvor de anvendte termer og definitioner fremgår(presentation language), inden de 2 hovedkomponenter præsenteres mere detaljeret. 

Dokumentet består af 12 afsnit, fulgt af 5 ekstra afsnit (appendixer), der dækker over bredere elementer relateret til TLS 1.3, som f.eks. Backwards compatibility og implementation notes. Afsnit 4 og 5 er de vigtigste, men særligt de 4, der beskriver handshake-protocollen. Dette afsnit er også langt det længste.

##### OAuth 2.0 Authorization Framework (RFC 6749) John
   Gå til [RFC 6749 - OAuth 2.0 Authorization Framework](https://tools.ietf.org/html/rfc6749).

Opgave: Beskriv kort, hvad RFC 6749 omhandler. 

>RFC 6749, med titlen "The OAuth 2.0 Authorization Framework," er en standard, der beskriver en sikker måde at give en tredjepartsapplikation adgang til data på en webtjeneste. Denne standard blev udgivet i oktober 2012 af Internet Engineering Task Force (IETF). 

Hvad er hovedformålet med OAuth 2.0, og hvilke roller og komponenter defineres i dokumentet? 

>Formålet med OAuth 2.0 er at give en tredjepartsapplikation begrænset adgang til en webtjeneste, enten på vegne af en bruger (kaldet "ressourceejeren") eller ved at give applikationen adgang til sine egne ressourcer.

Hvordan beskriver dokumentet de forskellige autorisationsmetoder og -flows, og hvordan er de struktureret?

>Godkendelsestilladelse (Authorization Grant): Dette er en tilladelse, som brugeren giver en applikation for at få adgang til deres data. Der findes forskellige typer af godkendelsestilladelser:

>Godkendelseskode: Bruges typisk af serverbaserede applikationer, hvor applikationen kan opbevare en hemmelig nøgle sikkert.

>Implicit tilladelse: Bruges af browser- eller mobilapplikationer, hvor det ikke er muligt at opbevare en hemmelig nøgle sikkert.

>Brugerens loginoplysninger: Bruges, når brugeren har tillid til applikationen, som f.eks. et operativsystem på deres enhed.

>Klientlegitimationer: Bruges, når applikationen handler på egne vegne og ikke på vegne af en bruger.

>Nøglebegreber:

>Adgangstoken (Access Token): Et token, som applikationen bruger til at få adgang til brugerens data på webtjenesten.

>Klient: Den applikation, der ønsker adgang til webtjenesten (f.eks. en tredjepartsapp).

>Ressourceejer: Den person eller enhed, der kan give adgang til beskyttede data (normalt brugeren).

>Autoriseringsserver (Authorization Server): Den server, der udsteder adgangstokens til applikationen, efter at have bekræftet og godkendt brugeren.

>Ressourceserver (Resource Server): Den server, der gemmer de beskyttede data og kan svare på forespørgsler, når en applikation præsenterer et gyldigt adgangstoken.

>**Sådan fungerer det i praksis:**

>1. Applikationen anmoder om tilladelse: Applikationen beder brugeren om tilladelse til at få adgang til deres data via autoriseringsserveren.
    
>2. Autoriseringsserveren udsteder en godkendelsestilladelse: Hvis brugeren giver tilladelse, udsteder serveren en godkendelsestilladelse til applikationen.
    
>3. Applikationen bruger godkendelsestilladelsen til at få et adgangstoken: Applikationen anmoder om et adgangstoken fra autoriseringsserveren ved hjælp af godkendelsestilladelsen.
    
>4. Autoriseringsserveren udsteder et adgangstoken: Hvis alt er i orden, udsteder serveren et adgangstoken til applikationen.
    
>5. Applikationen får adgang til beskyttede data: Applikationen bruger adgangstokenet til at få adgang til de data, den har fået tilladelse til.
    

>RFC 6749 beskriver de detaljerede trin og sikkerhedsovervejelser, der sikrer, at OAuth 2.0-rammen fungerer sikkert og pålideligt.

---