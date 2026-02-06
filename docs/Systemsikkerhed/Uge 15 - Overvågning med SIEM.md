## Øvelse 54: Design af detekteringspipeline – 7 abstraktionslag

Formålet med denne øvelse er at træne jeres evne til at analysere og designe en detekteringspipeline ved hjælp af modellen _detekteringens 7 abstraktionslag_. Øvelsen styrker jeres forståelse for, hvordan angreb manifesterer sig i systemet, og hvordan man strukturerer effektiv overvågning, klassificering og alarmering.

### 🧭 Hvad er en detekteringspipeline?

En **detekteringspipeline** er den kæde af trin, hvor system- og netværksdata bliver omsat til en meningsfuld alarm:

> Logs → Events → Alarmer → Reaktion

Det handler om at opdage angreb hurtigt og præcist – og om at filtrere støj fra, for at undgå falske positiver.

For at designe en effektiv pipeline kræver det, at man forstår både:

- Hvor angreb kan observeres
- Hvordan de bedst detekteres
- Hvornår og hvordan man bør alarmere

### 🔍 De 7 lag forklaret

Modellen _detekteringens 7 abstraktionslag_ er en struktureret metode til at analysere og designe detektion. Hvert lag repræsenterer et vigtigt spørgsmål i en detekteringspipeline – fra det første tekniske tegn på angreb til den endelige alarmering.

Tabellen herunder forklarer hvert lag og introducerer desuden et ekstra perspektiv: hvordan man **kan inddrage MITRE ATT&CK** som støtte i analysen. Det er ikke en forudsætning for at bruge modellen, men en nyttig måde at styrke arbejdet med klassificering, datakilder og detekterings mønstre.

_MITRE-tilknytningen er altså ét muligt redskab blandt flere – og kan bruges når det giver mening i jeres analyse._

| Lag                          | Spørgsmål                                  | Forklaring                                                                                                                                                                                                                                                                                          |
| ---------------------------- | ------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Feature Selection**     | Hvor og hvordan manifesterer angrebet sig? | Identificer hvilke systemaktiviteter, netværkshændelser eller adfærdsmønstre der potentielt indikerer et angreb. Dette er det _tekniske udtryk_ for angrebet.                                                                                                                                       |
| **2. Feature Extraction**    | Hvor findes informationen?                 | Find ud af hvilke datakilder (fx logs, sensorer, auditdata) der indeholder de nødvendige oplysninger for at observere den valgte feature.                                                                                                                                                           |
| **3. Event Selection**       | Hvilken del af datastrømmen er relevant?   | Udpeg de specifikke datapunkter eller loglinjer, der kan være interessante – og filtrér støjen fra. Dette skaber fokus for detektionen.                                                                                                                                                             |
| **4. Event Detection**       | Hvad definerer en konkret hændelse?        | Formuler regler eller mønstre, der kan genkende en enkelt hændelse som muligvis ondsindet – fx en mistænkelig kommando eller ændring.                                                                                                                                                               |
| **5. Attack Detection**      | Hvornår er det et angreb?                  | Kombinér flere hændelser eller kontekstuelle signaler og vurder, om der er tale om en _sammenhængende trusselsaktivitet_.                                                                                                                                                                           |
| **6. Attack Classification** | Hvilken type angreb er det?                | Klassificér angrebet ud fra en kendt trusseltype eller rammeværk – fx MITRE ATT&CK. Her angives både _teknikken_ (hvordan angrebet udføres, fx T1059 – Command and Scripting Interpreter) og den _taktik_ det tilhører (fx Execution). Dette muliggør standardiseret analyse og bedre prioritering. |
| **7. Attack Alarming**       | Hvem skal alarmen nå og hvordan?           | Beslut hvem der skal informeres, hvordan det skal ske (mail, dashboard, automatisk reaktion), og med hvilken alvorlighed/alarmtype. Dette muliggør effektiv reaktion og prioritering.                                                                                                               |
## 🛠️ Instruktioner[¶](https://25f-its-syssec-a5e8ac.gitlab.io/exercises/54_Detekterings7lag_og_Detection_Pipeline/#instruktioner "Permanent link")

Denne øvelse skal udføres individuelt.  
Men i næste opgave skal I præsentere og diskutere øvelsen med jeres gruppe i den næste opgave.

---

### 📌 Trin 1 – Vælg et scenarie[¶](https://25f-its-syssec-a5e8ac.gitlab.io/exercises/54_Detekterings7lag_og_Detection_Pipeline/#trin-1-vlg-et-scenarie "Permanent link")

- Brute force login via SSH
- Reverse shell fra kompromitteret maskine
- Ændring i systemfil (fx `/etc/shadow`)
- Uautoriseret `sudo`
- Egne scenarier fra semesterprojekt

---

### 📌 Trin 2 – Brug denne skabelon til at designe din pipeline:

```
### Angreb:
Port Scanning

### 1. Feature Selection
*Hvordan manifesterer angrebet sig?*
Mange TCP-SYN pakker i netværket.

### 2. Feature Extraction
*Hvor kan informationen hentes? (logkilder, processer, sockets)*
OPNsense IDS log, Netflow?

### 3. Event Selection
*Hvordan udvælger I det relevante?*
Forbindelsesforsøg til flere porte fra samme kilde-IP inden for et kort tidsinterval (fx 1-2 minutter).

### 4. Event Detection
*Hvordan identificeres en begivenhed teknisk?*
- Konfigurer Wazuh til at overvåge OPNsense-logs for mønstre af forbindelsesforsøg til flere porte fra samme IP-adresse. 
- Definer en regel, der udløser en alarm, hvis der ses mere end et bestemt antal forsøg inden for et givent tidsinterval.

### 5. Attack Detection
*Hvordan bestemmes, at det er et angreb?*
Kombiner flere hændelser, såsom gentagne forbindelsesforsøg til forskellige porte fra samme kilde, og vurder om dette mønster er usædvanligt i forhold til normal trafik. Hvis mønstret matcher kendte port scanning-teknikker, kan det klassificeres som et angreb.

### 6. Attack Classification
*Hvordan navngives og kategoriseres hændelsen?*
Klassificer angrebet som "Port Scanning" og kategoriser det under rekognosceringsaktiviteter i henhold til MITRE ATT&CK-rammeværket. Dette kan inkludere teknikker som T1046 (Network Service Scanning).

### 7. Attack Alarming
*Hvordan og til hvem alarmeres der?*
Konfigurer Wazuh til at sende alarmer til netværksadministratorer via e-mail eller et sikkerhedsdashboard. Angiv alvorligheden af angrebet og yderligere detaljer om kilden og målene for scanningen. Automatiseret reaktion, såsom blokering af den mistænkelige IP-adresse på OPNsense firewallen, kan også være relevant.
```


---

## 🤔 Refleksionsspørgsmål

```
- Hvilket lag var mest uklart at definere?
- Hvad kunne give falske positiver i jeres pipeline?
- Hvordan ville en angriber forsøge at undgå jeres detektion?
- Hvordan kan I bruge denne tilgang i eksamensprojektet?
```

---

💡 _Tip: Brug de 7 lag som en metode til at klarlægge og strukturere jeres detekteringsstrategi –_