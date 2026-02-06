# Uge 38 - Rekognoscering

#### Reflektions punkter efter forberedelsen og inden undervisningen:

- Hvad er passiv rekognoscering?
>Det er at fremsøge og gennemgå alle tilgængelige informationer om et mål, uden at være i direkte kontakt med målet.
- Hvad er OSINT?
>Open-source Intelligence. Offentlig tilgængelig dokumentation. Det kan fx være API-dokumentation.
- Hvad er de 3 faser i den passive rekognoscering proces?
>1. Bred søgning. 
>2. Fokuseret søgning på mulige targets. 
>3. Dokumenter resultaterne.
- Hvad er aktiv rekognoscering?
>Direkte scanning på målet med værktøjer som NMAP og GoBuster.
- Hvad er de 4 faser i den aktive rekognoscerings proces?
>Hver fase bygger videre på den foregående. Startende med portscanning.

---

### Opgave 9 - OSINT med github

#### Information
Kildekoden til en applikation i en offentligt repository på Github, udstiller uhensigtsmæssigt et stykke information for meget. Kan du finde den information?

[Link til repo:](https://github.com/mesn1985/DotNetApplicationWithToMuchInformation)


appsettings.json
```
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*",
  "Shop": {
    "ApiToken": "abc123def456ghi789",
    "ConnectionString": "Server=(localdb)\\Shop;Integrated Security=true;"
  }
}
```

---

### Opgave 10 - Aktiv Rekognoscering

#### Information

I denne øvelse skal du arbejde med aktiv rekognoscering.
I de første to øvelser skal netværksskanningsværktøjet _Nmap_ bruges til at finde frem til, hvilke porte der aktivt anvendes af services på en host.

Herefter skal du anvende en teknik, der kaldes _API enumeration_. Tilgangen er, at du bruger en liste kaldet en _Wordlist_, som indeholder flere forskellige ord. Hvert af disse ord testes som en _path_ mod et API, og man ser, hvilken HTTP-statuskode der returneres. Der anvendes værktøjer til automatisk at afprøve hvert ord. I disse øvelser skal du anvende Gobuster og OWASP ZAP.

Der er meget læring i øvelserne, så bevæg dig langsomt frem. Du kommer til at skulle læse op på værktøjerne, mens du udfører øvelserne.
**Husk at gemme de Juiceshop- og crAPI-wordlister, du laver løbende gennem øvelserne.**

[Link til øvelserne](https://github.com/mesn1985/WebApplicationSecurityBasicsLab/blob/main/crAPI/3_Active_reconnaissance.md#prerequisites)

**Øvelse 1**  
Jeg starter med at køre en NMAP skanning op imod min localhost, da det er her crAPI, Juiceshop er installeret. Jeg udfører skanningen fra WSL Kali med kommandoen:
```
sudo nmap -sS 127.0.0.1
```
Det giver følgende output:
```
┌──(benny㉿BennyStudiePC)-[/usr/share/seclists/Discovery]
└─$ sudo nmap -sS 127.0.0.1
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-09-16 10:11 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.0000090s latency).
Not shown: 997 closed tcp ports (reset)
PORT     STATE SERVICE
3000/tcp open  ppp
8443/tcp open  https-alt
8888/tcp open  sun-answerbook
```
Jeg kan se at der er tre åbne porte, men ikke meget information omkring hvilken services der er i brug.

**Øvelse 2**  
For at se mere information omkring services på de åbne porte, kører jeg en NMAP skanning med flagene:
```
sudo nmap -sC -sV -v 127.0.0.1
```
-sC = Default script scan
-sV = Service version detection
-v = verbosity

Med den udvidede skanning finder jeg informationer der fortæller mig at port 3000 har en connection til Juice Shop. Port 8443 og 8888 har begge crAPI, angivet i http-title.

**Øvelse 3**  
Opgaven her er at enummerere crAPI med programmet GoBuster, og en seclists wordlist der hedder common.txt. For at få det til at lykkedes, skal der bruges en del flag i kommandoen.
```
sudo gobuster dir -u https://127.0.0.1:8443 -w /usr/share/seclists/Discovery/Web-Content/common.txt -k --exclude-length 2837
```
Man skal huske https i URL'en. 
"-k" skipper TLS certifikat verificering.
--exclude-length "integer" kommandoen bruges til at omgå at alle stier modtager en 200 statuskode. Se en mere dybdegående forklaring i opgaveteksten.

Skanningen giver følgende output, som også gemmes i filen crAPI_wordlist.txt:
```
===============================================================
Gobuster v3.6
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     https://127.0.0.1:8443
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] Exclude Length:          2837
[+] User Agent:              gobuster/3.6
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
/.env                 (Status: 200) [Size: 201]
/.well-known/jwks.json (Status: 200) [Size: 528]
/community            (Status: 301) [Size: 175] [--> https://127.0.0.1/community/]
/favicon.ico          (Status: 200) [Size: 3150]
/identity             (Status: 301) [Size: 175] [--> https://127.0.0.1/identity/]
/images               (Status: 301) [Size: 175] [--> https://127.0.0.1/images/]
/robots.txt           (Status: 200) [Size: 67]
/workshop             (Status: 301) [Size: 175] [--> https://127.0.0.1/workshop/]
Progress: 4734 / 4735 (99.98%)
===============================================================
Finished
===============================================================
```

**Øvelse 4**  
Her er opgaven at udføre den samme skanning, med en anden wordlist "quickhits.txt":
De har det til fælles at de angiver path'en /.env som en fælles sti.  
Tilgår man den path (https://127.0.0.1:8443/.env) i en browser, downloades en fil der indeholder default brugernavne og passwords.

**Øvelse 5**  

* Initiate an automated scan (use Ajax spiders) and wait for the scan to complete.  
  >ZAP startes med kommandoen "zaproxy" i WSL.

* Review the site tree in the left site. What information does it provide?  
  >Et overblik over det mål man scannede. Man kan se de paths der returnerede et 200 statuskode svar. Vælger man dem, kan man nærstudere request og response trafikken.
* Review the alerts. Can you correlate any of the alerts to the vulnerability you discovered in exercise 4?
  >.env Information Leak. 
* What information can you find in the pane AJAX Spider?
>???
  
---