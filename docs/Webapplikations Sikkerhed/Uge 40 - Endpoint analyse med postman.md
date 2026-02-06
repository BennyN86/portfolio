# Uge 40 - Endpoint analyse med Postman

### crAPI øvelser
#### Øvelse 1
Opret en ny collection i Postman, og navngiv den "crAPI"

#### Øvelse 2
I Burp Suite, optag http-requesten for oprettelse af en ny bruger:
```http
POST /identity/api/auth/signup HTTP/1.1
Host: 127.0.0.1:8443
Cookie: language=en; cookieconsent_status=dismiss; continueCode=WEKRqkoBOy8DYvL3Zw52e7EJmGkxfpKcBx0XQj4K19prVzlanWMx6NgPb93O
Content-Length: 89
Sec-Ch-Ua: "Not;A=Brand";v="24", "Chromium";v="128"
Content-Type: application/json
Accept-Language: da-DK,da;q=0.9
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.6613.120 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */*
Origin: https://127.0.0.1:8443
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://127.0.0.1:8443/signup
Accept-Encoding: gzip, deflate, br
Priority: u=1, i
Connection: keep-alive

{
"name":"postman",
"email":"postman@test.com",
"number":"88776655",
"password":"awas2WAB!!"
}
```

I Postman oprettes en ny request, kaldet "signup". Fra Burp Suite kopieres URL'en `https://127.0.0.1:8443/identity/api/auth/signup` og metoden ændres til "POST".
I Body indsættes JSON indholdet fra Burp Suite requesten. Husk at trykke "Save".

#### Øvelse 3

Øvelsen her er at forsøge at oprette en bruger med et password som ikke lever op til kravene for et acceptabelt password. Fx fire bogstaver - abcd. 
Ud fra fejlmeddelelsen i Postman, kan man udlede at API'et bruger et "Spring-Framework" til at validere kvaliteten af passwordet ved brugeroprettelse.

```JSON
{
    "message": "Validation failed",

    "details": "org.springframework.validation.BeanPropertyBindingResult: 1 errors\nField error in object 'signUpForm' on field 'password': rejected value [abcd]; codes [Size.signUpForm.password,Size.password,Size.java.lang.String,Size]; arguments [org.springframework.context.support.DefaultMessageSourceResolvable: codes [signUpForm.password,password]; arguments []; default message [password],100,6]; default message [size must be between 6 and 100]"
}
```

#### Øvelse 4
Her skal jeg opsamle en "Bearer Token" gennem Burp Suite, til anvendelse i Postman.  
Jeg starter med at "intercepte" login-requesten i Burp Suite:
```http
POST /identity/api/auth/login HTTP/1.1
Host: 127.0.0.1:8443
Cookie: language=en; cookieconsent_status=dismiss; continueCode=WEKRqkoBOy8DYvL3Zw52e7EJmGkxfpKcBx0XQj4K19prVzlanWMx6NgPb93O
Content-Length: 52
Sec-Ch-Ua: "Not;A=Brand";v="24", "Chromium";v="128"
Content-Type: application/json
Accept-Language: da-DK,da;q=0.9
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/128.0.6613.120 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Accept: */*
Origin: https://127.0.0.1:8443
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://127.0.0.1:8443/login
Accept-Encoding: gzip, deflate, br
Priority: u=1, i
Connection: keep-alive

{"email":"postman@test.com","password":"awas2WAB!!"}
```

Fra response meddelelsen modtager jeg følgende token:
```JSON
{
"token":"eyJhbGciOiJSUzI1NiJ9.eyJzdWIiOiJwb3N0bWFuQHRlc3QuY29tIiwiaWF0IjoxNzI3NjgxNTU2LCJleHAiOjE3MjgyODYzNTYsInJvbGUiOiJ1c2VyIn0.ln3H8b06SIjxJrxZQOLvs5UCFLoDITx4d2I-LYJvYsUuQqeoBkQ9E935tXIxKaZIUT8qEpBdrkQAMNsfXeEb8UBiV-YZqhhDUB6UBB4Qqpr9r-9Zk5btKVhpzvgoOR9rMEPWEkTtAYwVyvzcyUD4oOaun1XeiXnG9pmIZ0Y-HYZyNT8y3e1iWEIIPSpZVKXRKHHdE3wCyx8KAfqmYqEVob095YADdvyvThnBdY5pT71-ABD5E6nsQwgGfw_Ui07TonyP6Uyhev2iUe2uNQg2zJqRNuWoVHrhc68pB4NEL7xTlzTQcEAARNVYfWwUUKR9qKTxULFRxJrwq7F0AbqJlQ",
 "type":"Bearer",
 "message":"Login successful",
 "mfaRequired":false
 }
```

I Postman indsættes denne token ved at trykke på `ooo` knappen ud for min collection. Under "Edit", vælges "Auth" og token indsættes:  
![Bearer Token](screenshots/20240930093705.png)

Husk at gemme!

#### Øvelse 5
Opret en ny post i crAPI community og gem requesten i Postman collectionen.  
Som i [Øvelse 2] starter jeg med at intercepte requesten i Burp, og kopiere URL'en og Body-indholdet til Postman. For at validere requesten, oprettes en ny post gennem Postman, og jeg kontrollere at den kan ses i browseren:
![Request](screenshots/20240930094717.png)
![Community Post](screenshots/20240930094728.png)

#### Øvelse 6

I de foregående opgaver har jeg manuelt opbygget en collection af requests. Postman har også en funktion til automatisk at generere en collection, mens jeg gennemgår hjemmesiden og alle dens endpoints. Det vil jeg forsøge i denne øvelse.

Jeg starter med at installere browser-extensionen "[Postman Interceptor](https://learning.postman.com/docs/sending-requests/capturing-request-data/interceptor/)".

Inden jeg begynder at bruge proxyen, oprettes en ny collection "CrAPI-Automated".  
Så fortsætter jeg med at gennemgå alle sider og funktioner på hjemmesiden. I Postman stoppes optagelsen og alle requests vælges og gemmes til crAPI-Automated collectionen.

!!! info "INFO"
    All of the captured requests, have been captured with the bearer token included in the header.  
    You could change the token by adding a token for the collection in the same way you did in exercise 5.  
    Remember to remove the Authorization header from the requests

#### Øvelse 7

I sidste øvelse kombineres brugen af Burp og Postman, ved at benytte Burp Suite som en proxy for Postman. På den måde kan Burp Suites analyse værktøjer anvendes sammen med requests fra Postman.

1. Open Postman settings from `File -> Settings`
2. Click the `proxy` tab
3. Click the on switch for `Use custom proxy configurations`
4. The server ip should be `127.0.0.1` and the port should be `8080`(Default proxy port Burp suite)
5. Click the `General` tab of the settings.
6. Click on the switch for `SSL certificate verification` to turn off verification of certificates.
7. Ensure that burp suite is running.
8. In the `proxy` tab of Burp suite, select HTTP History.
9. Send a couple of requests from Postman, and verify the These request appears in Burp suites HTTP history.

--- 

### JuiceShop øvelser

#### Øvelse 1
Øvelsen består i at gennemgå JuiceShop endpointet med Swagger-dokumentationen. `/api-docs/swagger.json`.  

1. Try experimenting with what you can find in the documentation at the endpoint `/api-docs/swagger.json`, does quantity have validation in the backend API?

 Ud fra det ledende spørgsmål i øvelsen, vil jeg forsøge at execute en en POST-metode, med et negativt antal. 

For at kunne "execute" API-kald skal man "Authorize" øverst på siden. Log ind på JuiceShop og gennem Inspector, kopier Bearer token og indsæt på swagger siden. 

Dokumentationen angiver at quantity skal være en integer, mellem 1 og 500:  

![API_Doc](screenshots/20240930103412.png)

Men når jeg afprøver API-kald med quantity på 1000 og -10, returneres der stadigvæk en 200 OK statuskode.

#### Øvelse 2
Opret en automatisk collection for JuiceShop. Samme fremgangsmåde som med crAPI.

---

## Opgave 19 - Skabelon for Test Cases

### Test Case ID: ==JuiceShop-001==

### Summary
En negativ test case der viser hvordan Juice Shop tillader brugere at se indholdet af andre brugeres indkøbskurv.
### Preconditions

1. Opret to brugere, A og B.
2. Burp Suite installeret.
### Test Steps

1.  Log ind som bruger A.
2.  Åben Burp Suite og tænd "Intercept".
3. Læg varer i Indkøbskurven.
4. I Burp Suite noteres basket-ID, fra Get-Requestens path. 
   `GET /rest/basket/6 HTTP/1.1`
5. Log ind som bruger B.
6. Intercept en request, når varer tilføjes i kurven.
7. Send requesten til "Repeater"
8. Ret basket ID til bruger A's ID og tryk "Send".
### Expected Result
Hvis testen lykkedes som forventet, vil bruger B modtage indholdet af bruger A's indkøbskurv i http-response.
### Actual Result
Som forventet modtager bruger B, indholdet af bruger A's indkøbskurv.
### Status
Succes
