# Uge 10 - Fysisk sikkerhed og Host-based firewall

## Fysisk Sikkerhed

1. **Hvad er de tre hovedbekymringer for fysisk sikkerhed, i prioritetsrækkefølge?**
      - Personer, data, og udstyr.

2. **Hvad er de tre hovedtyper af fysiske sikkerhedsforanstaltninger?**
      - Afskrækkende, detekterende, og forebyggende kontroller.

3. **Hvorfor vil du måske gerne bruge RAID?**
      - RAID bruges til at beskytte data ved at kopiere det til flere lagringsmedier, hvilket beskytter mod datatab ved en enkelt enheds fejl.

4. **Hvad er fysisk sikkerheds vigtigste bekymring?**
      - At beskytte personer, da de er vanskeligere at erstatte end data eller udstyr.

5. **Hvilken type fysisk adgangskontrol kunne du måske indføre for at blokere adgang til en bil?**
      - Fysiske barrierer som hegne, betonbarrierer, eller sikkerhedslandskab.

6. **Kan du give tre eksempler på fysiske kontroller, der fungerer som afskrækkende foranstaltninger?**
      - Skilte om overvågning, alarmselskabers logoer, og advarsler om vagthunde.

7. **Kan du give et eksempel på, hvordan et levende organisme kunne udgøre en trussel mod dit udstyr?**
      - Insekter eller små dyr kan forårsage elektriske kortslutninger eller ødelægge kabler.

8. **Hvilken kategori af fysisk kontrol kunne inkludere en lås?**
      - Forebyggende kontroller.

9. **Hvad er residual data, og hvorfor er det en bekymring ved beskyttelse af din datas sikkerhed?**
      - Residual data er resterende data på elektroniske medier, der ikke er blevet slettet korrekt. Det er en bekymring, fordi det kan indeholde følsomme oplysninger, der kan blive udsat for sikkerhedsrisici.

10. **Hvad er dit primære værktøj til at beskytte personer?**
      * Evakuering og beredskabsplanlægning for at sikre, at alle kan fjernes hurtigt og trygt fra farlige situationer.

## Host-based firewalls

#### Opgave 19 - Installation af iptables

1. Installer _iptables_ med kommandoen: `sudo apt install iptables`
2. Udskriv versionsnummeret med kommandoen `sudo iptables -V`
   ==iptables v1.8.10 (nf_tables)==
3. Installer iptables-persistent med kommandoen `sudo apt install iptables-persistent`
4. Udskriv alle firewall-regler (regelkæden) med kommandoen `sudo iptables -L`. Hvad betyder _INPUT, FORWARD & OUTPUT_?
      * Input = Indgående forbindelser. 
      * Output = Udgående forbindelser. 
      * Forward betyder at trafikken skal gå igennem maskinen fra en grænseflade til en anden. Trafikken er ikke tiltænkt denne host.

```
Chain INPUT (policy ACCEPT)                                                        target     prot opt source               destination                                             
Chain FORWARD (policy ACCEPT)                                                      target     prot opt source               destination                                             
Chain OUTPUT (policy ACCEPT)                                                       
target     prot opt source               destination
```

Reflekter over, hvad der menes med firewall-regler, samt hvad der menes med "regelkæden".

   * Regler filtrere trafikken, så vi kan tillade og forbyde trafik. 
   * Regelkæden er den rækkefølge trafikken holdes op i mod. Når trafikken matcher en regel, tillades eller blokeres trafikken og man kigger ikke på efterfølgende regler i kæden.

#### Opgave 20 - Bloker alt indgående trafik

Udskriv regelkæden og notér, hvordan den ser ud
```
Chain INPUT (policy ACCEPT)                                                        target     prot opt source               destination                                             
Chain FORWARD (policy ACCEPT)                                                      target     prot opt source               destination                                             
Chain OUTPUT (policy ACCEPT)                                                       
target     prot opt source               destination
```

Lav en regel i slutningen af regelkæden, som dropper alt trafik, med kommandoen `sudo iptables -A INPUT -j DROP`

![](screenshots/Pasted%20image%2020250304104707.png)

Forsøg at pinge hosten (F.eks. fra din Kali instans), kommer der noget svar?
===Nej===

#### Opgave 21 - Tillad indgående trafik fra etablerede forbindelser

**Instruktioner:**

1. Eksekver kommandoen `sudo iptables -F`
     - Flusher (rydder) alle eksisterende iptables-regler, så vi starter fra en ren konfiguration.
2. Eksekver kommandoen `sudo iptables -A INPUT -i lo -j ACCEPT`
    - Tillader trafik på loopback-interface (`lo`), hvilket sikrer, at systemet kan kommunikere med sig selv.
3. Eksekver kommandoen `sudo iptables -A OUTPUT -o lo -j ACCEPT`
    - Tillader udgående trafik på loopback-interface, så interne processer fungerer korrekt.
4. Eksekver kommandoen `sudo iptables -A OUTPUT -p tcp --dport 80 -j ACCEPT`
    - Tillader udgående HTTP-trafik (port 80), som bruges til at hente websider via `curl` eller en browser.
5. Eksekver kommandoen `sudo iptables -A OUTPUT -p tcp --dport 443 -j ACCEPT`
    - Tillader udgående HTTPS-trafik (port 443), som er nødvendig for sikre webforbindelser.
6. Eksekver kommandoen `sudo iptables -A OUTPUT -p udp --dport 53 -j ACCEPT`
    - Tillader udgående DNS-opslag via UDP (port 53), hvilket er nødvendigt for at oversætte domænenavne til IP-adresser.
7. Eksekver kommandoen `sudo iptables -A OUTPUT -p tcp --dport 53 -j ACCEPT`
    - Tillader udgående DNS-opslag via TCP (port 53), som bruges til større DNS-forespørgsler.
8. Eksekver kommandoen `sudo iptables -A INPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT`
    - Tillader indkommende pakker, der er en del af eksisterende eller relaterede forbindelser (så svar på forespørgsler kommer tilbage).
9. Eksekver kommandoen `sudo iptables -A OUTPUT -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT`
    - Tillader udgående pakker, der hører til allerede etablerede forbindelser.
10. Eksekver kommandoen `sudo iptables -A INPUT -j DROP`
    - Blokerer al anden indgående trafik, hvilket øger sikkerheden ved at afvise uønskede forbindelser.

med `-m conntrack` (conntrack = connection tracking), ved iptables at der skal holdes styr på, hvilke forbindelser der er etableret, og hvem der har initieret dem. Med `--ctstate ESTABLISHED,RELATED -j ACCEPT`, tillades indadgående pakker fra servere, hvor klienten (OS) selv har oprettet forbindelsen.

#### Opgave 23 - Tillad indgående trafik fra specifikke ICMP-beskeder

**Instruktioner**

1. Tillad ICMP-pakker af type 3 med kommandoen `sudo iptables -A INPUT -p icmp --icmp-type destination-unreachable -m conntrack --ctstate NEW,ESTABLISHED,RELATED -j ACCEPT`.
2. Tillad ICMP-pakker af type 11 med kommandoen `sudo iptables -A INPUT -p icmp --icmp-type time-exceeded -m conntrack --ctstate NEW,ESTABLISHED,RELATED -j ACCEPT`.
3. Tillad ICMP-pakker af type 12 med kommandoen `sudo iptables -A INPUT -p icmp --icmp-type parameter-problem -m conntrack --ctstate NEW,ESTABLISHED,RELATED -j ACCEPT`.
4. Udskriv regelkæden. Kan du finde fejlen? Og hvordan kan den rettes?
   ![](screenshots/Pasted%20image%2020250304110144.png)
Det er en fejl at de nyoprettede tilladelser er placeret til sidst i regelkæden, efter "drop all" reglen. 
En løsning kunne være at slette "drop all" reglen og tilføje den igen, sidst i regelkæden. Alternativt kan man bruge kommandoen `sudo iptables -P INPUT DROP`, som sætter firewallen til som udgangspunkt at droppe alt trafik, uden at det er konfigureret i regelkæden.

For at slette "drop all" reglen, køres først kommandoen `sudo iptables -L --line-numbers
Dette vil vise alle regler med deres tilsvarende numre i hver kæde. Herefter kan reglen slettes med følgende kommando `sudo iptables -D <kæde> <regelnummer>

I dette tilfælde `sudo iptables -D INPUT 3`
`
De ICMP-pakker, der er blevet tilladt, er hver især af en bestemt type. Undersøg hvad hver af de tre typer betyder.
		1. ICMP type 3: "Destination unreachable" beskeder. Nyttige fejlbeskeder som også fortæller hvorfor forbindelsen ikke lykkedes. Fx for store pakker.
		2. ICMP type 11: Time-to-live information. Nyttig i fejlfinding og giver ingen brugbar information til trusselsaktører.
		3. ICMP type 12: "Paramater problem". Manglende eller fejl i IP-header.


