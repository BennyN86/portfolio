# Uge 08 - CIS benchmarks og Mitre ATT&CK

## CIS Kontrollerne
CIS (Center for Internet Security) har udviklet **CIS Critical Security Controls (CIS18)**, som er en samling af 18 kontroller, der hjælper organisationer med at beskytte sig mod cybertrusler. Kontrollerne er opdelt i tre kategorier baseret på modenhedsniveauer og organisationens sikkerhedsbehov:

| **Nr.** | **CIS-kontrol**                                        | **Beskrivelse**                                                                                   |
| ------- | ------------------------------------------------------ | ------------------------------------------------------------------------------------------------- |
| 1       | Inventory and Control of Enterprise Assets             | Identificér og administrér alle enheder (pc’er, servere, IoT-enheder) i netværket.                |
| 2       | Inventory and Control of Software Assets               | Identificér og administrér alt software for at forhindre uautoriserede programmer.                |
| 3       | Data Protection                                        | Beskyt følsomme data ved hjælp af kryptering, backup og adgangskontrol.                           |
| 4       | Secure Configuration of Enterprise Assets and Software | Implementér sikre konfigurationer for systemer, netværksenheder og software.                      |
| 5       | Account Management                                     | Administrér brugerkonti og rettigheder for at reducere risikoen for kompromitterede konti.        |
| 6       | Access Control Management                              | Begræns adgang til systemer og data baseret på "least privilege" og "need to know."               |
| 7       | Continuous Vulnerability Management                    | Identificér og udbedr sårbarheder gennem scanning og patching.                                    |
| 8       | Audit Log Management                                   | Overvåg og analyser logs for at opdage sikkerhedshændelser.                                       |
| 9       | Email and Web Browser Protections                      | Beskyt brugere mod phishing, malware og andre webtrusler.                                         |
| 10      | Malware Defenses                                       | Implementér beskyttelse mod malware, herunder antivirus og EDR-løsninger.                         |
| 11      | Data Recovery                                          | Sikr backup og test af gendannelsesplaner.                                                        |
| 12      | Network Infrastructure Management                      | Sikr netværksudstyr og implementér segmentering for at reducere angrebsflader.                    |
| 13      | Security Awareness and Skills Training                 | Uddan medarbejdere i cybersikkerhed og phishing-awareness.                                        |
| 14      | Service Provider Management                            | Evaluer og administrér sikkerheden hos tredjepartsleverandører.                                   |
| 15      | Application Software Security                          | Sikr udvikling og brug af applikationer gennem sikker kodning og tests.                           |
| 16      | Incident Response Management                           | Udarbejd og test en beredskabsplan for håndtering af sikkerhedshændelser.                         |
| 17      | Penetration Testing                                    | Udfør pentests for at identificere svagheder og simulere angreb.                                  |
| 18      | Security Management Program                            | Implementér et overordnet sikkerhedsprogram for at sikre en systematisk tilgang til IT-sikkerhed. |

## CIS Benchmarks
En liste over hvad der skal udføres for at forstærke et system som fx Ubuntu eller Windows.
De forskellige benchmarks mappes til CIS18 kontrollerne og MITRE ATT&CK.
CIS sælger pre-hardened udgaver.

## MITRE ATT&CK

#### Mitre Att&ck TA0004 - [Privilege Escalation](https://attack.mitre.org/tactics/TA0004/)
Dette er en taktik hvor en trusselsaktør forsøger at skaffe sig rettigheder i et system, som gør det muligt at gennemføre deres mål. Det kunne fx være at aktøren har skaffet sig adgang til en almindelig brugerkonto. Gennem forskellige teknikker vil aktøren efterfølgende forsøge at opnå administrator rettigheder eller lignende.

#### Teknikkerne T1548 - Abuse Elevation Control Mechanism
T1548 indeholder seks forskellige teknikker en trusselsaktør kan bruge til at misbruge eksisterende kontrolmekanismer for at opnå højere rettigheder.

F.eks **T1548.003** som udnytter `sudo` funktionen i Linux OS. `sudo` kommandoen giver en bruger mulighed for at køre kommandoer med root-brugerens rettigheder. En trusselsaktør kan udnytte dette ved fx at ændre i sudoers-filen `/etc/sudoers`.

#### Mitigeringen M1026
M1026 er en liste over mitigeringsmetoder, relateret til de enkelte teknikker listet under taktikken TA0004. Mitigeringerne beskriver hvordan man kan sikre sig mod at en trusselsaktør kan udnytte de enkelte teknikker.

F.eks. er der også en mitigering til teknikken der udnyttede sudo-kommandoen. Mitigeringen består i at konfigurere systemet så der kræves et kodeord for at køre sudo-kommandoen. Hvis et password kræves for sudo, skal en trusselsaktør kende det, selv hvis de har terminaladgang. Ved at sætte `timestamp_timeout` til 0, skal brugeren indtaste passwordet ved hver kørsel af sudo.

#### Detekteringen DS0022
DS0022 er et detektionstype som overvåger om der ændres i filer, herunder deres tilladelser, attributter og indhold. Formålet er at opdage, hvis en angriber ændrer filer eller rettigheder for at opnå højere privilegier.

F.eks. kan man overvåge om der sker ændringer i filen `/etc/sudoers`, fra eksemplet fra før. Til at overvåge kan værktøjet **autid (Linux Audit Daemon)** anvendes.
