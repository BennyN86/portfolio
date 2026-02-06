# Uge 41 - Angreb på autentificering & password

### [Opgave 12 - Angreb på autentificering & password](https://24e-its-web-sikkerhed-ucl-pba-its-bd36043c2948d5aa4536abdfad07f.gitlab.io/exercises/12_Attacking_Authentication_password/)

Formålet med disse fem øvelser, er en grundlæggende introduktion til hvordan man kan udføre test af autentificering på web applikationer, som anvender brugernavn og passwords.

Som dokumentation for mit arbejde med øvelserne, har jeg valgt at skrive en "Test Case" for hver øvelse, ud fra den skabelon vil blev introduceret til i sidste uge. Formålet med Test Cases er at dokumentere sårbarheden, og hvordan den kan genskabes.

#### Øvelse 1 - [Dictionary attack with Burp Suite](https://github.com/mesn1985/HackerLab/blob/main/crAPI/5_basic_Authentication_Attacks.md#dictionary-attack-with-burp-suite)

**Test Case ID:**  
==crAPI-012-001==

**Summary:**  
Dette er en destruktiv testcase, der demonstrerer, hvordan crAPI er sårbar over for et brute force-angreb ved hjælp af Burp Suite Intruder og wordlists.

**Preconditions:**  

* Wordlist med 20 passwords der matcher kravene for passwords i crAPI.  
* Oprettet en bruger med et password fra wordlisten.  
* Burp Suite installeret

**Test Steps:**  

1. I Burp Suite opfanges en authentication request.  
2. Requesten sendes til Intruder.  
3. Vælg angrebsmetoden "Sniper".  
4. I requestens "content body", skal email adressen matche brugeren oprettet som precondition.  
5. Password værdien i content body skal highlightes med **§**-symbolet. (fx **§**password**§**).  
6. Gå til "Payloads" fanen.  
7. "Payload Sets" indstillingerne, sættes til "1" og "Simple List".  
8. I "Payload Settings" indlæses wordlisten fra preconditions.  
9. Under "Payload Encoding" fjernes fluebenet i "URL-encode these characters".  
10. Tryk på "Start Attack", øverst i højre hjørne.

**Expected Result:**  
Hvis testen lykkedes som forventet, skulle requesten med det korrekte password gerne have modtaget en ***200/OK*** statuskode.

**Actual Result:**  
![intruder_brute_force](screenshots/intruder.png)


#### Øvelse 2 - [Dictionary attack with WFuzz](https://github.com/mesn1985/HackerLab/blob/main/crAPI/5_basic_Authentication_Attacks.md#dictionary-attack-with-wfuzz)

**Test Case ID:**  
==crAPI-012-002==


**Summary:**  
Dette er en negativ testcase, der demonstrerer, hvordan crAPI er sårbar over for et brute force-angreb ved hjælp af "WFuzz" og wordlists.

**Preconditions:**  

* WFuzz installeret.
* Wordlisten *rockyou.txt* tilgængelig.

**Test Steps:**  

1. Opret en bruger med et password fra *rockyou.txt* listen. (fuzz@test.com; $Brst917).
2. Start brute force-angrebet med følgende kommando:  
```shell
wfuzz -d '{"email":"fuzz@test.com","password":"FUZZ"}' --hc 401 -H 'Content-Type: application/json' -z file,/usr/share/seclists/rockyou.txt https://127.0.0.1:8443/identity/api/auth/login
```

**Expected Result:**  
Hvis testen lykkedes som forventet, skulle requesten med det korrekte password gerne modtage en *200/OK* statuskode. Da flaget `--hc 401` bleve benyttet i wfuzz kommandoen, skulle der kun være et resultat i outputtet.

**Actual Result:**  
Jeg fik flere gange en timeout fejl med rockyou listen:  
![](screenshots/fuzz_error.png)

Men forsøgte jeg med en kortere wordlist, kunne jeg fremproducere det forventede resultat:  
![](screenshots/fuzz_ok.png)



#### Øvelse 3 - [Exhaustive brute force attack with Burpsuite](https://github.com/mesn1985/HackerLab/blob/main/crAPI/5_basic_Authentication_Attacks.md#exhaustive-brute-force-attack-with-burpsuite)

**Test Case ID:**  
==crAPI-012-003==


**Summary:**  
Dette er en negativ testcase, der demonstrerer, hvordan crAPI er sårbar over for et udtømmende brute force-angreb ved hjælp af Burp Suite Intruder og tilfældige tegnkombinationer.

**Preconditions:**  

* Bruger oprettet med adgangskoden `passwd` (brute2@force.com).  
*Brug Postman til dette for at omgå regler for passwords*.
* Burp Suite Installeret

**Test Steps:**  

1. I Burp Suite opfanges en authentication request.  
2. Requesten sendes til Intruder.  
3. Vælg angrebsmetoden "Sniper". 
4. I requestens "content body", skal email adressen matche brugeren oprettet som precondition.  
5. Password værdien i content body skal highlightes med **§**-symbolet. (fx **§**password**§**). Vælg "Add §" i højre side.  
6. Gå til "Payloads" fanen.  
7. "Payload Sets" indstillingerne, sættes til "1" og "Brute forcer".  
8. I "Payload Settings", sæt "character set" til `passwd` og "Min- og Max Length" sættes til 6.   
9. Under "Payload Encoding" fjernes fluebenet i "URL-encode these characters".  
10. Tryk på "Start Attack", øverst i højre hjørne.

**Expected Result:**  
Det forventes at Burp Suite lykkedes med at knække koden, ved at afprøve de 46.656 forskellige bogstavkombinationer en ad gangen.

**Actual Result:**  
Efter en time havde Burp Suite kun nået gennem 462 af de 46656 kombinationer, så jeg valgte at stoppe processen. Men det var muligt at udføre angrebet og altså dermed bevise sårbarheden og behovet for en foranstaltning.


#### Øvelse 4 - [Password Spraying](https://github.com/mesn1985/HackerLab/blob/main/crAPI/5_basic_Authentication_Attacks.md#password-spraying)

**Test Case ID:**  
==crAPI-012-004==


**Summary:**  
Dette er en negativ testcase, der demonstrerer, hvordan crAPI er sårbar over for et brute force-angreb af typen *Password Spraying*. I stedet for at teste mange forskellige adgangskoder for en enkelt brugerkonto (som ved klassisk brute force), forsøger password spraying at bruge en kort liste af almindelige adgangskoder (f.eks. "Password123") mod mange forskellige konti. Denne metode bruges når man har et begrænset antal forsøg til at indtaste den korekte adganskode.

**Preconditions:**  

* Burp Suite Installeret.


**Test Steps:**  

1. Opret en ny bruger i crAPI. (Mail:passwd@spraying.com; Pass:Testing3#).  
2. Lav en wordlist med fem sandsynlige passwords. Den netop oprettede brugers password. skal være en af de fem.
3. Lav en wordlist med alle crAPI brugernes mailadresser. Inklusiv brugeren der blev oprettet i step 1.
4. I Burp Suite opfanges en authentication request. Og denne sendes til Intruder.
5. Vælg angrebsmetoden "Cluster bomb".
6. Både værdien for "email" og "password" i content body, skal highlightes med **§**-symbolet. (fx **§**password**§**).  
Marker og vælg =="Add §"== i højre side.
7. Gå til "Payloads" fanen. 
8. "Payload Sets" indstillingerne, sættes til "1" og "Simple List".  
9. Indlæs wordlisten med brugernes mailadresser.
10. Sørg for der ikke er flueben i feltet i "Payload encoding".
11. Vælg nu "Payload set" 2, og indlæs listen med de fem passwords.
12. Igen, sørg for der ikke er flueben i feltet i "Payload encoding".
13. Tryk på "Start Attack", øverst i højre hjørne.

**Expected Result:**  
Hvis testen lykkedes som forventet, skulle requesten der indeholder den korrekte kombination af email og password, gerne modtage en *200/OK* statuskode.

**Actual Result:**  
![](screenshots/password_spray.png)


#### Øvelse 5 - [Brute forcing JuiceShop](https://github.com/mesn1985/HackerLab/blob/main/JuiceShop/5_basic_Authentication_Attacks.md#brute-forcing)

**Test Case ID:**  
==JuiceShop-012-005==


**Summary:**  
Dette er en negativ testcase, der demonstrerer, hvordan JuiceShop er sårbar over for et brute force-angreb ved hjælp af "WFuzz" og wordlists.

**Preconditions:**  

* Adgang til JuiceShop.
* Kendskab til administratorens mailadresse: `admin@juice-sh.op`.
* Burp Suite Installeret.
* WFuzz installeret.
* Wordlisten `best1050.txt` fra SecLists tilgængelig.

**Test Steps:**  

1. I Burp Suite opfanges og analyseres en authentication request, for at finde den korrekte path.
2. Start brute force-angrebet med følgende kommando:  
```shell
wfuzz -d '{"email":"admin@juice-sh.op","password":"FUZZ"}' --hc 401 -H 'Content-Type: application/json' -z file,/usr/share/seclists/Passwords/Common-Credentials/best1050.txt http://127.0.0.1:3000/rest/user/login
```

**Expected Result:**  
Hvis wordlisten `best1050.txt` indeholder administratorens password, skulle wfuzz gerne finde det ved at afprøve alle kombinationer.

**Actual Result:**  
![](screenshots/juice_brute.png)


---

### [Opgave 13 - Angreb på autentificering & tokens](https://24e-its-web-sikkerhed-ucl-pba-its-bd36043c2948d5aa4536abdfad07f.gitlab.io/exercises/13_Attacking_Authentication_token/)

Formålet med disse øvelser en grundlæggende introduktion til hvordan man kan udføre test af autentificering på web applikationer, som anvender tokens, som uddeles efter en succesfuld autentificering.

Som dokumentation for mit arbejde, har jeg valgt at skrive en "Test Case" for Øvelse 2 og 3. Den første øvelse herunder 

#### Øvelse 1 - [Getting to know the JWT token](https://github.com/mesn1985/HackerLab/blob/main/crAPI/6_Basic_Token_Attacks.md#getting-to-know-the-jwt-token)

Første øvelse handler om at se nærmere på JSON Web Tokens (JWT).  
Gennem hjemmesiden [https://jwt.io](https://jwt.io), skal følgende spørgsmål besvares:

* Try altering the value of sub(abbreviation for subject). How much does the signature change?  
> Når værdien for "sub" ændres, medfører det en ændring af den *Payload* som "sub" er en del af. Når payloaden ændres, vil det også betyde at *Signature* ændres.

* Try altering the value of iat(abbreviation for Issued At). How much does the signature change?
> Ligesom i det første spørgsmål vil det resultere i en ændring af *Payload* og *Signature*, da "iat" er en del af payloaden.

* Copy the encoded jwt header, and decode it in a base64 decoder, What values do you see?
> {"alg":"HS256","typ":"JWT"}

* Copy the encoded jwt payload, and decode it in a base64 decoder, What values do you see?
> {"sub":"1234567890","name":"John Doe","iat":1516239022}

På hjemmesiden er der også en rigtig god forklaring om JWT'ers opbygning og anvendelse. [Learn more about JWT.](https://jwt.io/introduction).

Et andet sted med brugbar information om JWT, er i [RFC 7519.](https://datatracker.ietf.org/doc/html/rfc7519)


#### Øvelse 2 - [Manipulating the payload](https://github.com/mesn1985/HackerLab/blob/main/crAPI/6_Basic_Token_Attacks.md#manipulate-the-payload)

**Test Case ID:**  
==crAPI-013-002==


**Summary:**  
Dette er en negativ test case der demonstrerer en fejl i valideringen af JSON Web Tokens i crAPI.

**Preconditions:**  

* Burp Suite med extension "JWT Editor" installeret.
* To brugere oprettet i crAPI. A og B.

**Test Steps:**  

1. Log ind med bruger A.
2. I Burp Suite opfanges den GET-request der forekommer når "Dashboard" tilgås.
3. Send requesten til "Repeater", og gå til fanen.
4. I "Repeater", vælg fanen "JSON Web Token".
5. I "Payload" tekstfeltet, under nøglen "sub". Ret bruger A's emailadresse, til bruger B's mailadresse.
6. Tryk på den orange "Send" knap, øverst til venstre i vinduet.
7. I "Response" modtages nu bruger B's oplysninger. Der er altså ingen eller fejlagtig kontrol med valideringen af JWT'en.
8. Noter den kodede token, til senere brug.
```
eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJiYnJ1Z2VyQHRlc3QuY29tIiwiaWF0IjoxNzI4NDA5ODU5LCJleHAiOjE3MjkwMTQ2NTksInJvbGUiOiJ1c2VyIn0.cBoA3HfryB-ZddNe2BaqtuJH0qDbX1BbdZl35L4N2fVrQ99HfjeeEtX1uKpJRhpjh9rr6zPd1TB_gsO80o3G8iQ-XxkF1czOpH4u3FRHoZiTwyyC3sixuWb32uNpoM9eARKsTyrIFqd9bn_h_vJ7ZKThNWmeU9fhEq6dg9qL1HoM28eJni2AxuxB0uS3AWXauzr6KlbzIpFrgzROXZuCwe1QS8q__FV5U8q0y-pWFVO6oTosStckiUbUweVP9R6buaa2558iNOrLjslMCBPPyUyZo7weUfIXloTBJlGOzUXkaBucmT1gq6Gg2QS4cHl957Q_q3L09FJ06jnaEmr6eA
```

Næste skridt er at se, om vi kan oprette en ny adgangskode ved hjælp af den manipulerede token.

1. I Burp Suite opfanges den POST-request der forekommer når man forsøger at resette sit password.
2. Send requesten til "Repeater", og gå til fanen.
3. Under "Authorization", indsættes den token vi gemte tidligere.
4. Tryk på den orange "Send" knap, øverst til venstre i vinduet.
5. Svaret er en 401-statuskode med beskeden "Invalid Token". I denne del af applikationen, virker det altså til at valideringen er på plads.

**Expected Result:**  
Første halvdel af øvelsen tillader bruger A, at modtage bruger B's oplysninger.

Anden del af øvelsen, viser at man ikke kan resette sit password, uden at være i besiddelse af den rigtige token.

**Actual Result:**  
Denne øvelse udviklede sig egentlig i retningen af to separate test cases. Og derfor blev denne dokumentations form lidt noget rod, i netop denne øvelse.


#### Øvelse 3 - [Abusing unsigned tokens in JuiceShop](https://github.com/mesn1985/HackerLab/blob/main/JuiceShop/6_Basic_Token_Attacks.md#manipulating-and-abusing-unsigned-tokens)

Denne øvelse resulterede i fire følgende test cases:

**Test Case ID:**  
==JuiceShop-013-003.1==


**Summary:**  
Dette er en negativ test case der viser at der på server siden kun valideres på JWT i cookie delen af en request, mens Authorization headeren helt kan udelades.

**Preconditions:**  

* Bruger oprettet i JuiceShop
* Burp Suite med extension "JWT Editor" installeret.

**Test Steps:**  

1. Ved brugerlogin, opfang i Burp Suite GET-forespørgselen til /rest/user/whoami. den med en 200 statuskode i response.
2. Manipuler token i authorization headeren og tryk "send".
3. Genskab værdien af token i headeren.
4. Manipuler token i cookien og tryk "send".
5. Genskab værdien af token i headeren.
6. Fjern hele Authorization Headeren, og tryk "Send"

**Expected Result:**  
Kun token i cookie valideres. Fjernelse af Authorization Headeren, giver stadig kode 200.

**Actual Result:**  


 **Test Case ID:**  
==JuiceShop-013-003.2==


**Summary:**  
Dette er en positiv test case der bekræfter, at det ikke er muligt at manipulere JWT-payloaden til at ændre brugerens rettigheder.

**Preconditions:**  

* Bruger oprettet i JuiceShop
* Burp Suite med extension "JWT Editor" installeret.
* GET-requesten fra test case: ==JuiceShop-013-003.1==

**Test Steps:**  

1. Åbn GET-Requesten i Burp Suite Repeater.
2. Under fanen "JSON Web Token", ret emailadressen i payloaden til admin@juice-sh.op.
3. Send forespørgselen med den manipulerede JWT.

**Expected Result:**  
Det forventes ikke at at serveren svare med den 200/OK statuskode.


**Actual Result:**  
Som forventet lykkedes det ikke, serveren returnerer en "304 Not Modified" kode.



**Test Case ID:**  
==JuiceShop-013-003.3==


**Summary:**  
Denne destruktive test case beviser, at systemet er sårbart over for manipulation af JWT, ved at ændre algoritmen til "none". 

**Preconditions:**  

* Bruger oprettet i JuiceShop
* Burp Suite med extension "JWT Editor" installeret.
* GET-requesten fra test case: ==JuiceShop-013-003.2==

**Test Steps:**  

1. Åbn GET-Requesten i Burp Suite Repeater.
2. Under fanen "JSON Web Token", tryk på "Attack" knappen, og vælg `"none" Signing Algorithm`.
3. I pop-up vinduet med værdien "none", tryk på OK.
4. Send forespørgslen.
5. Noter den nye Token, da den skal anvendes i næste case.

**Expected Result:**  
Serveren returnerer en 200/OK statuskode.

**Actual Result:**  
Serveren returnerer en 200/OK statuskode.


**Test Case ID:**  
==JuiceShop-013-003.4==


**Summary:**  
Dette er en destruktiv test case der beviser at der muligt at rette en anden brugers kodeord, og dermed opnå "Escalation of Privilege"

**Preconditions:**  

* Bruger oprettet og authentificeret.
* Token fra test case: ==JuiceShop-013-003.3==
* Burp Suite med extension "JWT Editor" installeret.

**Test Steps:**  

1. Gå til "Change Password", og ret passwordet til det ønskede password for admin kontoen.
2. Opfang i Burp Suite forespørgslen for "change password". og send den til Repeater.
3. Ret token i både Authorization Header og Cookie, til værdien af token fra sidste case.
4. Send forespørgslen. Der returneres med en 200/ok response der indeholder administratorens email og password i MD5 hash.
5. MD5 hask kan forsøges cracket i Crackstation, for bekræftelse af at det nu er ændret.
6. Log ind som JuiceShop administrator.

**Expected Result:**  
Det er nu muligt at logge ind som administrator med det nye kodeord.

**Actual Result:**  
Det er nu muligt at logge ind som administrator med det nye kodeord.

#### Øvelse 4 - [Attacking Password Recovery and OTP](https://github.com/mesn1985/WebApplicationSecurityBasicsLab/blob/main/crAPI/5_basic_Authentication_Attacks.md#attacking-password-recovery-and-otp)

I denne opgave undersøges det hvordan crAPI håndterer et password reset, ved hjælp af en OTP, sendt til brugerens mail.  
OTP værdien er altid en 4-cifret kode og dermed ikke ret sikker, da der kun kan være 10.000 forskellige kombinationer.

I opgaven finder man gennem Burp Suite frem til at der er et max antal forsøg på 10, ved password reset med en forkert OTP. Men man kan også i requestens path se at det er version 3, af API'et der anvendes: `POST /identity/api/auth/v3/check-otp HTTP/1.1`.  
Ved at ændre forespørgslen til `POST /identity/api/auth/v2/check-otp HTTP/1.1` fjernes begrænsningen på de 10 forsøg.

Ved hjælp af Burp Suite Intruder og en wordlist med de 10.000 kombinationer, er det muligt at brute force sig frem til et password reset.

![](screenshots/otp_brute.png)


---
