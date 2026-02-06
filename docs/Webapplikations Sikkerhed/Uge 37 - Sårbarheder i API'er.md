# Uge 37 - Sårbarheder i API'er

### Opgave 7 - crAPI-system og sikkerhedsmål

#### Information

Alt software (herefter benævnt system) har et forretningsmæssigt formål, altså en tiltænkt anvendelse, som kan skabe værdi for en virksomhed eller organisation. Dette kalder man ofte systemmål (andre gange benævnt _system objectives_). Inden man kan teste et system for eventuelle sårbarheder, bør man først forstå, hvad systemet kan (systemmål), og derefter anvende denne viden til at udlede, hvordan systemet kan misbruges, benævnt misbrugstilfælde. Herefter kan sikkerhedsmålene (andre gange benævnt _security objectives_) for systemet fastsættes, da misbrugstilfældene fastslår, hvad man ikke må kunne gøre med systemet. Altså, hvad der skal gøres for at undgå, at systemet bliver misbrugt.

Når man har klarlagt system- og sikkerhedsmål, kan man lave en skitse af systemet for at fastslå, hvilke API'er der bliver anvendt. Her kan man f.eks. anvende Burp Suite for at se, hvor mange forskellige API'er der bliver kommunikeret med.

I denne øvelse skal I for første gang arbejde med system- og sikkerhedsmål. Vi vender tilbage til system- og sikkerhedsmål i faget Software Sikkerhed.

Det overordnede mål med øvelsen er, at I får indsigt i anvendelsen af crAPI'er samt deres funktionaliteter og desuden prøver at lave en indledende analyse ved at anvende system- og sikkerhedsmål. Herudover får I en introduktion til, hvordan en webapplikation kan skitseres ved at anvende Burp Suite.

#### Instruktioner

1. Sæt jer ind i den grundlæggende brug af crAPI ved at følge denne [guide](https://github.com/mesn1985/WebApplicationSecurityBasicsLab/blob/main/crAPI/1_Getting_To_Know_the_Application.md).
2. Identificer systemmålene for crAPI (kun webdelen; MailHog kan I ignorere).
3. Identificer sikkerhedsmålene for hvert systemmål.

| Systemmål                                      | Sikkerhedsmål                                                                      |
| ---------------------------------------------- | ---------------------------------------------------------------------------------- |
| Overblik over bilens grundlæggende oplysninger | Stjæle bilens VIN-nummer. Fx til misbrug i form af omregistrering                  |
| Kontakte Mekaniker                             | Kan ikke misbruges til andet end irritation.                                       |
| Bilens GPS-position                            | Positionsdata kan både misbruges i form af overvågning, eller til at stjæle bilen. |
| Tilkøb til bilen                               | Bestille varer. Det ser dog ikke ud til at leveringsadressen kan ændres.           |
| Indløse rabatkuponer                           | Kan ikke misbruges. Kræver man har en rabatkupon.                                  |
| Se købshistorik                                | Kan ikke misbruges.                                                                |
| Community forum posts                          | Kan misbruges til at udgive sig for at være en anden.                              |

4. Identificer mulige angrebsflader ved at tegne en skitse over applikationsarkitekturen (hvilke API'er anvendes).  

    Da det er en blackbox test, er det kun de API'er, som kan identificeres fra browseren, der kan tegnes ind på skitsen. Burp Suite eller browserens udviklerværktøjer kan hjælpe med at identificere de API'er, som browseren sender forespørgsler til.  

    Antag at roden at path afgør om der er tale om forskellige api'er. F.eks. /user/api og /workshop/api kan i antage er forskellig api'er. Dette er blot for øvelsens skyld, i praksis kan man ikke antage dette (selv om ville være en god struktur )

>identity/api/auth  
>identity/api/v2/user  
>identity/api/v2/vehicle  
>workshop/api/mechanic  
>workshop/api/shop  
>community/api/v2/community  
>/maps/api (https://maps.googleapis.com)

![](screenshots/opg7_app.png)

---

### Opgave 8 - BOLA & Excessive data exposure

#### Information
I denne øvelse skal der arbejdes med identificering af sårbarheder.
Øvelserne er beskrevet her [crAPI - BOLA and Excessive data exposure](https://github.com/mesn1985/HackerLab/blob/main/crAPI/2_Exploiting_BOLA_And_Excessive_Data_Exposure.md).

Formålet med øvelserne er at give en grundlæggende introduktion til BOLA sårbarheder, samt excessive data exposure. Du kan anvende denne tutorial til at lære hvordan man analyser trafik med Burp suite [Intercepting traffic with burp suite](https://www.youtube.com/watch?v=Nr2fYpStshA)

Det er vigtig at du noterer fremgangsmåden fra alle øvelserne ned, da du skal bruge dem til at udarbejde test cases i næste uge
#### Instruktioner

Udfør øvelse [1 - BOLA using object ids without authorization](https://github.com/mesn1985/HackerLab/blob/main/crAPI/2_Exploiting_BOLA_And_Excessive_Data_Exposure.md)

>Som nævnt i guiden oprettede jeg to brugere, der hver fik tilføjet en bil. Med Burp Suite analyserede jeg de HTTP requests der blev eksekveret når jeg brugte Dashboard siden. Ved "Contact Mechanic" bemærkede jeg at requesten indeholdte bilens VIN-nummer.  

>Jeg sendte requesten til Repeater, og ændrede VIN-nummeret til en anden bil. Jeg fik stadig et svar fra serveren, og altså adgang til noget jeg ikke er autoriseret til. En såkaldt BOLA.  
Bruger A VIN: **9LULW59NVRJ641761**  
Bruger B VIN: **8NVME98JHDO129421**

>Det samme gør sig gældende for funktionen "refresh location". Requesten der sendes indeholder bilens unikke ID.  
Bruger A bil id: 9008fa7c-b9ef-4579-a915-9e2f3986d3f2  
Bruger B bil Id: f7cf1ff5-d11a-4f10-908b-4055bc51763a

>Bruger B, kan ved at ændre ID'et i requesten få adgang til Bruger A's bil's lokation.

Udfør øvelse [2 - Excessive Data exposure](https://github.com/mesn1985/HackerLab/blob/main/crAPI/2_Exploiting_BOLA_And_Excessive_Data_Exposure.md)

>I forummet på siden, sendes der et HTTP-response indeholdende info omkring andre brugere der har postet i forummet.   
Dette response indeholder deres email og ikke mindst bilens ID.

Brug resultaterne fra øvelse 1 og 2 til at finde køretøjs placeringen fra en bruger du ikke selv har oprettet.

>Med brugeren "Pogba"s ID, kan jeg genbruge teknikken fra øvelse 1, og finde bilen placering.
```
{"carId":"cd515c12-0fc1-48ae-8b61-9230b70a845b",
"vehicleLocation":{"id":2,"latitude":"31.284788","longitude":"-92.471176"},
"fullName":"Pogba",
"email":"pogba006@example.com"}
```

Udfør øvelse [3 - Explorer OWASP Application Security Verification standard.](https://github.com/mesn1985/HackerLab/blob/main/crAPI/2_Exploiting_BOLA_And_Excessive_Data_Exposure.md)  
OWASP ASVS, defines verifications which one should perform on an application, to ensure that the security levels are correct. Examine which section you can find information about Access controls, and if there are any specific mentioning about BOLA/IDOR. Also examine the standard for any reference on how to test for BOLA/IDOR.

>Information om Access Control findes i følgende afsnit:  
* v1.4 - Access Control Architecture  
* v4 - Access Control.

>BOLA nævnes ikke, men der refereres til IDOR i v4.2.1:   
*Verify that sensitive data and APIs are protected against Insecure Direct Object Reference (IDOR) attacks targeting creation, reading, updating and deletion of records, such as creating or updating someone else's record, viewing everyone's records, or deleting all records.*

Udfør øvelse [4 - Get access to other customers mechanic reports.](https://github.com/mesn1985/HackerLab/blob/main/crAPI/2_Exploiting_BOLA_And_Excessive_Data_Exposure.md)

>I Burp Suite kunne jeg se at når jeg kontaktede mekanikeren, blev der genereret et HTTP-response med følgende indhold:
```
{
"response_from_mechanic_api":{
"id":12,
"sent":true,
"report_link":"http://127.0.0.1:8888/workshop/api/mechanic/mechanic_report?report_id=12"}
"status":200
}
```
>Men jeg kunne ikke regne ud hvad jeg skulle stille op med informationen. Ved hjælp med en walktrough, fandt jeg frem til at jeg kunne bruge strengen *"/mechanic/mechanic_report?report_id=xx"* i en GET request. Ved at ændre "report_id", til fx 2, lykkedes det at få adgang til andres brugeres værkstedshistorik.
```
GET /workshop/api/mechanic/mechanic_report?report_id=2 HTTP/1.1
Host: 127.0.0.1:8888
sec-ch-ua: "Chromium";v="127", "Not)A;Brand";v="99"
sec-ch-ua-platform: "Windows"
Accept-Language: da-DK
sec-ch-ua-mobile: ?0
Authorization: Bearer eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJ1c2VyYkBib2xhLmNvbSIsImlhdCI6MTcyNTg3MTkzOCwiZXhwIjoxNzI2NDc2NzM4LCJyb2xlIjoidXNlciJ9.GmrgzC-Z281HwL5RFPmbQ6FEiJvePwyNBDanvBmEwfm7fvGcBIqwcNsSioTi9XocHxzHRYlVGNFXn99xGqgrv9f7Jb78ltKDp4dZAfcEgpi_RB9GrOR0H6bEfGZe1bqv_y75ywh4YzsgdOqZz3PduYaSCVhLmMt1BOgDYy_7fCQlzOWHBjyGN7yhFMPvDO_qw2gy8yb-BMS9h8RjY5EnQhkF2Nc-HVBEWIDHb8Ozbk1bFQ1K_PebXHID3yo0X1pEsvhErlWmCJpUuvbz95UwBnn7BXq-YYjLof7mQ_A37eCkSawARh40M1lwHsacUjQ1kalCsmvxbahwP5iyhV5GKQ
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/127.0.6533.100 Safari/537.36
Content-Type: application/json
Accept: */*
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: http://127.0.0.1:8888/dashboard
Accept-Encoding: gzip, deflate, br
Connection: keep-alive
```

```
{"id":2,"mechanic":{"id":1,"mechanic_code":"TRAC_JHN","user":{"email":"jhon@example.com","number":""}},"vehicle":{"id":4,"vin":"8IGEF39BZUJ159285","owner":{"email":"test@example.com","number":"9876540001"}},"problem_details":"My car MG Motor - Hector Plus is having issues.\nCan you give me a call on my mobile 9876540001,\nOr send me an email at test@example.com\nThanks,\nTest.\n","status":"finished","created_on":"09 September, 2024, 05:46:36"}
```

(Ekstra opgave) Anvend [OWASP ZAP's](https://www.zaproxy.org/) [Automatisk skanning](https://www.youtube.com/watch?v=wLfRz7rRsH4) og se om du kan finde yderlige sårbarheder.  

>CWE:215 - .env Information Leak  
CWE:693 - Content Security Policy (CSP) Header Not Set  
CWE:1021 - Missing Anti-clickjacking Header

(Ekstra opgave) Se om du kan eksekverer flere af dine tidligere identificeret misbrugstilfælde.

>Ved hjælp af "requester" lykkedes det at finde andre brugeres lokation.

---





