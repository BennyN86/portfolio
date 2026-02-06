# ISO 27001/2

ISO 27001 og ISO 27002 er internationale standarder udviklet af **International Organization for Standardization (ISO)**. De beskriver krav og anbefalinger til håndtering af informationssikkerhed med fokus på at beskytte fortrolighed, integritet og tilgængelighed af data.  

### ISO 27001 – Krav til et ISMS  

ISO 27001 specificerer kravene til at etablere, implementere, vedligeholde og forbedre et Information Security Management System (ISMS). Standarden anvender risikostyring til at identificere og håndtere sikkerhedsrisici gennem både tekniske løsninger og organisatoriske processer.  

Den bygger på **PDCA-cirklen (Plan-Do-Check-Act)** for kontinuerlig forbedring.  

ISO 27001 indeholder et Anneks A, som definerer 93 sikkerhedskontroller opdelt i fire hovedkategorier:  
- **Organisatoriske** (f.eks. politikker og risikostyring)  
- **Personrelaterede** (f.eks. medarbejderuddannelse)  
- **Fysiske** (f.eks. adgangskontrol og overvågning)  
- **Teknologiske** (f.eks. kryptering og netværkssikkerhed)  

### ISO 27002 – Praktisk vejledning  

ISO 27002 fungerer som en vejledende standard og giver praktiske anbefalinger til, hvordan man implementerer kontrollerne beskrevet i ISO 27001.  

En ofte anvendt analogi er, at ISO 27001 er "opskriften", mens ISO 27002 er "kokkens vejledning".  

!!! info "Eksempel"
    - **ISO 27001 (Anneks A, 5.26)** fastsætter, at sikkerhedshændelser skal håndteres efter dokumenterede procedurer.  
    - **ISO 27002** beskriver i detaljer, hvordan hændelser bør håndteres, herunder:  
    - Etablering af et incident response-team  
    - Hurtig indsamling af bevismateriale  
    - Aktivering af beredskabsplaner  
    - Efterfølgende analyse for at identificere og mitigere sårbarheder  

ISO 27002 henviser ofte til andre kontroller i standarden, så relaterede emner håndteres i en sammenhængende proces.  

### Backup – Et konkret eksempel  

Gennem risikostyring kan en organisation identificere behovet for **backup** af data.  

- **ISO 27001 (Anneks A)** fastslår: "Backup af information, software og systemer skal vedligeholdes og testes regelmæssigt i overensstemmelse med den aftalte politik."  
- **ISO 27002** specificerer:  
  - Hvilke data der skal sikkerhedskopieres  
  - Hvor ofte backup skal foretages  
  - Hvordan backup-data beskyttes (f.eks. kryptering)  
  - Hvor længe en backup skal gemmes  
  - At backup skal opbevares på en separat lokation for at sikre redundans  

!!! info "Eksempel"
    En virksomhed vælger at implementere en daglig inkrementel backup af kundedata og en ugentlig fuld backup i en krypteret cloud-løsning. Derudover beslutter de at teste gendannelse af backup en gang i kvartalet for at sikre, at data er intakte.  

    Dette adresserer **integritet** (data skal være uændrede og præcise) og **tilgængelighed** (backup sikrer adgang til data ved fejl eller katastrofer).  

### ISO 27001 og 27002 i praksis  

ISO 27001 fokuserer på krav til informationssikkerhed, mens ISO 27002 giver vejledning til implementering af sikkerhedskontroller. Sammen understøtter de **CIA-modellen** (Confidentiality, Integrity, Availability – fortrolighed, integritet og tilgængelighed).  

Fordele ved implementering af ISO 27001 og ISO 27002:  
- Beskytter information mod trusler gennem etablering af et ISMS  
- Styrker organisationens evne til at håndtere risici  
- Sikrer compliance med lovgivning og branchekrav  

### Statement of Applicability  

Ved implementering af ISO 27001 skal en organisation udarbejde en **Statement of Applicability (SoA)**, der dokumenterer, hvilke af de 93 sikkerhedskontroller fra Anneks A, der er relevante, og hvordan de er implementeret.  

Yderligere dokumentation udarbejdes gennem arbejdet med de specifikke kontroller.  

### Afslutning  

ISO 27001 og 27002 hjælper organisationer med at sikre en struktureret tilgang til informationssikkerhed. Ved at implementere et ISMS kan virksomheder beskytte deres data, overholde lovgivning og minimere risici.  

Den praktiske vejledning i ISO 27002 gør det muligt at omsætte kravene i ISO 27001 til konkrete foranstaltninger, hvilket gør det lettere for organisationer at styrke deres sikkerhedsarbejde og dokumentere deres indsats.  
