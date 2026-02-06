# Uge 9 - Netværksdesign og segmentering

### Øvelse 52 - Praktisk netværkssegmentering med OPNsense
Opgaven består af at tilføje et management interface til OPNsense routeren, og konfigurere ip-adressen og en dhcp-server for interfacet. Efterfølgende startes en virtuel host forbundet til interfacet, og det kontrolleres at hosten modtager en ip-adresse gennem DHCP.

![](screenshots/Pasted%20image%2020250225093802.png)

![](screenshots/Pasted%20image%2020250225093809.png)

---

### Øvelse 61 - Konfigurering af firewall i OPNsense
Denne øvelse har til formål at lære at konfigurere og teste firewall regler i OPNsense.

Ved starten af øvelsen er det muligt at pinge fra LAN til MGMT, men ikke omvendt. Det skyldes at der mangler en regel i firewallen som tillader trafikken fra MGMT til LAN. Opgaven er så at oprette en "allow-all" regel, så ping funktionen virker.

I næste del af øvelsen skal man køre en NMAP scanning af MGMT-hosten fra en host på LAN netværket. Scanningen viser at ingen porte er åbne. Så starter man en webserver på mgmt-hosten og kører NMAP igen. Nu kan man se at port 80 (HTTP) er åben.  

![](screenshots/Pasted%20image%2020250225111920.png)

Så skal der oprettes en firewall regel der blokerer TCP-trafik på port 80 fra LAN til MGMT. Reglen skal placeres over "allow-all" reglen, da firewallen læser reglerne en ad gangen fra toppen. Øvelsen går på at oprette reglen under management interfacet, og man skal derfor huske at sætte direction til "out". Man kunne også have oprettet reglen under LAN-interfacet.

Man skal tænke på indgående trafik som den trafik der tilgår firewallen fra interfacet. Altså den vej trafikken krydser firewallen.  

```
Direction: out
Protocol: TCP
Source: LAN net
Destination: management net
Port range: HTTP(80)
```
![](screenshots/Pasted%20image%2020250225112131.png)

Nu er porten ikke længere åben. NMAP viser den som filtered, hvilket som regel betyder at en firewall har nægtet forbindelsen.

![](screenshots/Pasted%20image%2020250225112600.png)

I den resterende del af øvelsen oprettes en ny webserver på port 443. Man opretter en ny regel som blokerer UDP-trafik på port 443. Det påvirker naturligvis ikke NMAP-scanningen da webserveren anvender TCP-protokollen. 

![](screenshots/Pasted%20image%2020250225112828.png)