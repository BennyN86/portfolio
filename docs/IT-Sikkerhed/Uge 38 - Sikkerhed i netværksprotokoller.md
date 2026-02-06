# Uge 38 - Sikkerhed i netværksprotokoller

### [Øvelse 18 - Udforskning af OPNsense](https://ucl-pba-its.gitlab.io/24e-its-intro/exercises/18_intro_opgave_vbox_opnsense_2/)

#### Information

I denne øvelse skal du oprette forbindelse til opnsense cli og webinterface.
Jeg kalder opnsense for en router, men i virkeligheden er det meget mere end det, feks. en firewall, DNS proxy, IDS/IPS og meget mere.

Du kan danne dig et overblik på https://opnsense.org/

#### Instruktioner

1. Log på CLI og kontroller at interfaces og netværks adresser stemmer overens med netværksdiagrammet.
    - Noter WAN adressen: 
    >10.0.2.15/24
    - Når du forstår diagram og CLI interface informationen så fortsæt til næste trin.

2. Log på web interfacet og gennemgå dokumentationen for opnsense på [https://docs.opnsense.org/](https://docs.opnsense.org/) samtidig med at i finder tingene på jeres opnsense installation i virtualbox.

3. Besvar følgende og dokumenter på gitlab pages:
    1. Hvilke features tilbyder opnsense? 
    >[Features](https://docs.opnsense.org/intro.html#feature-set)
       
    2. Hvordan kan du kontrollere om din opnsense version har kendte sårbarheder? 
       >System->Firmware->Status->RunAnAudit -> Security
       
    3. Hvad er lobby og hvad kan man gøre derfra? 
       >Lobby giver et overblik over routerens status og indstillinger.
       
    4. Kan man se [netflow](https://en.wikipedia.org/wiki/NetFlow) data direkte i opnsense og i så fald hvor kan man se det? 
       >Det kan man under "Reporting->NetFlow"
       
    5. Hvad kan man se omkring trafik i "Reporting"?
       >Systemets sundhed. Antallet af pakker på tværs af routeren. Kvaliteten af netværket, latency osv.
       
    6. Hvor oprettes vlan's og hvilken IEEE standard anvendes?
       >Interfaces -> Other Types -> VLAN. 802.1q
       
    7. Er der konfigureret nogle firewall regler for jeres interfaces (LAN og WAN)? Hvis ja, hvilke?
       >"Allow internal DNS"
       
    8. Hvilke slags VPN understøtter opnsense?
       >* IPsec
       >* OpenVPN
       >* WireGuard
         
    9. Hvilket IDS indeholder opnsense?
       >Suricata
       
    10. Hvor kan man installere `os-theme-cicada` i opnsense?
      >System -> Firmware -> Plugins. Temaet vælges derefter i Settings -> General -> Theme
        
    11. Hvordan opdaterer man firmware på opnsense? 
        >System->Firmware->Status->Check For Updates
        
    12. Hvad er nyeste version af opnsense? 
      >24.7
        
    13. Er jeres opnsense nyeste version? Hvis ikke så opdater den til nyeste version. 
      >24.1 -> 24.7

---

### Øvelse 26 - Wireshark analyse

#### Information

I denne øvelse skal du bruge wireshark til at analysere netværkstrafik fra et angreb.  
Wireshark er en del af kali linux. Trafikken er optaget i en `.pcap` fil som wireshark kan læse.

**Målet med øvelsen er at lære at bruge wireshark, altså at få brugt mulighederne i de forskellige menuer og undersøge hvordan man kan filtrere og analysere netværks trafik.**

Den specifikke pcap fil er ikke så relevant og spørgsmålene i instruktionerne er mest for at have noget at arbejde efter.  
Det vil sige at du selv bestemmer hvor meget tid du bruger på den her øvelse, men du lærer mest ved at være nysgerrig, bruge dokumentation og selv lave søgninger på ting du ikke kender eller forstår i wireshark.

Øvelsen tager udgangspunkt i filen `malware-traffic-analysis.net-2014-12-15.zip` som du kan finde på itslearning i dagens plan. I zip filen finder du index.html som giver et overblik over de øvrige filer, blandt andet en fil med svar.  
Der er også en ekstra .pdf fil som giver gode forklaringer: `2014-12-15-traffic-analysis-exercise-additional-information.pdf`

Inden du hopper til løsningen og den ekstra .pdf fil, så prøv at besvare alle spørgsmålene, dem du ikke kan besvare springer du bare over og gemmer til du ser løsningen.

Det er nok nødvendigt at bruge wireshark dokumentationen mens du arbejder, den er her:  
[https://www.wireshark.org/docs/wsug_html_chunked/](https://www.wireshark.org/docs/wsug_html_chunked/)

Der er også nogle wireshark tutorials her: [https://www.malware-traffic-analysis.net/tutorials.html](https://www.malware-traffic-analysis.net/tutorials.html)

#### Instruktioner

1. Hent `malware-traffic-analysis.net-2014-12-15.zip` og pak filen ud (zip password er `infected`)
2. Udpak `.pcap` filen fra `2014-12-15-traffic-analysis-exercise.pcap.zip`
3. Åbn `.pcap` filen i wireshark
4. Indstil hvad du ser i wireshark [https://unit42.paloaltonetworks.com/unit42-customizing-wireshark-changing-column-display/](https://unit42.paloaltonetworks.com/unit42-customizing-wireshark-changing-column-display/)
5. Besvar følgende:
    1. Hvad er host navnene på de 3 windows maskiner i pcap filen?
       >* MYHUMPS-PC  
       >* ROCKETMAN-PC  
       >* WORKSTATION6  
       >* Da windows maskiner ofte bruger NetBIOS til at annoncere hostnavne på et netværk, filtrerede jeg trafikken med filteret `nbns`, og kunne dermed finde host navnene.
       
    2. Hvad er IP adressen/adresserne, på windows maskinen/maskinerne som er blevet ramt af et exploit kit?
       >* 192.168.204.137 (MYHUMPS-PC)
       >* 192.168.204.139 (ROCKETMAN-PC)
       >* 192.168.204.146 (WORKSTATION6)  
       >* Kan aflæses i "source" kolonnen.
       
    3. Hvad er MAC adressen/adresserne, på windows maskinen/maskinerne som er blevet ramt af et exploit kit?
       >* 00:0c:29:61:c1:89 (MYHUMPS-PC)
       >* 00:0c:29:9d:b8:6d (ROCKETMAN-PC)
       >* 00:0c:29:fc:bc:2e (WORKSTATION6)  
       >* MAC-adresserne kan aflæses i data-link laget (Ethernet II), i pakkerne i Wireshark.
       
    4. Hvad er navnet/navnene på domænet/domænerne på den/de kompromitterede hjemmeside/hjemmesider?
       >* ~~sullymansion.com~~
       >* theopen.be
       >* ~~newsnow.co.uk~~
       
    5. Hvad er IP adressen/adresserne, på den/de kompromitterede hjemmeside/hjemmesider?
       >* ~~50.57.227.160~~
       >* 213.186.33.19
       >* ~~213.146.191.132~~  
       >* Aflæses fx i "destination" på http-request pakkerne.
       
    6. Hvad er navnet/navnene på domænet/domænerne på exploit kittet (måske flere?)?
      >epzqy.iphaeba.eu:22780
       
    7. Hvad er IP adressen/adresserne, på exploit kittet (måske flere?)?   
      >168.235.69.248
       
    8. Er der nogen af windows maskinerne der er blevet inficeret, hvis ja hvilke?
       
    9. Hvilket/hvilke exploit kit(s) er nævnt i pcap filen?
       
    10. Hvilken exploit er anvendt ved hjælp af expolit kittet/kitne? (Flash, Java, IE, etc)
        
    11. Hvilken URL(s) er anvendt som redirect mellem den/de kompromitterede hjemmeside(r) og exploit kittet/kitne?
        
    12. Hvad er ip redirect adressen/adresserne på redirect URL(s)?
        
6. kontroller dine svar i forhold til øvelsens svar og ekstra information som du kan finde i `malware-traffic-analysis.net-2014-12-15.zip`