# Uge 7 - Introduktion til faget

## Øvelse 10 - Grundlæggende netværksviden

| Spørgsmål                                                         | Svar                                                                                                                                                     |
| ----------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Hvad betyder LAN?                                                 | Local Area Network                                                                                                                                       |
| Hvad betyder WAN?                                                 | Wide Area Network                                                                                                                                        |
| Hvor mange bits er en ipv4 adresse?                               | 32 bits                                                                                                                                                  |
| Hvor mange forskellige ipv4 adresser findes der?                  | 2^32=4.294.967.296<br>4,29 milliarder unikke adresser                                                                                                    |
| Hvor mange bits er en ipv6 adresse?                               | 16 x 8 = 128                                                                                                                                             |
| Hvad er en subnet maske?                                          | Andelen af bits i en IP-adresse som er reserveret til netværket.                                                                                         |
| Hvor mange hosts kan der være på netværket 10.10.10.0/24          | 2^8 – 2 = 254   <br>Hosts=2(^32-subnet)-2.  <br><br>Man trækker 2 fra, fordi man skal reservere én IP til netværksadressen og én til broadcast adressen. |
| Hvor mange hosts kan der være på netværket 10.10.10.0/22          | 2^10 – 2 = 1022 <br>                                                                                                                                     |
| Hvor mange hosts kan der være på netværket 10.10.10.0/30          | 2^2 – 2 = 4                                                                                                                                              |
| Hvad er en MAC adresse?                                           | Media Access Control. NIC adresse.                                                                                                                       |
| Hvor mange bits er en MAC adresse?                                | 48 bits                                                                                                                                                  |
| Hvilken MAC adresse har din computers NIC?                        | E0-0A-F6-3A-78-43                                                                                                                                        |
| Hvor mange lag har OSI modellen?                                  | 7 lag                                                                                                                                                    |
| Hvilket lag i OSI modellen hører en netværkshub til?              | Lag 2                                                                                                                                                    |
| Hvilket lag i OSI modellen hører en switch til?                   | Lag 2                                                                                                                                                    |
| Hvilket lag i OSI modellen hører en router til?                   | Lag 3                                                                                                                                                    |
| Hvilken addressering anvender en switch?                          | MAC-adresser                                                                                                                                             |
| Hvilken addressering anvender en router?                          | IP-adresser                                                                                                                                              |
| På hvilket lag i OSI modellen hører protokollerne TCP og UDP til? | Lag 4                                                                                                                                                    |
| Hvad udveksles i starten af en TCP forbindelse?                   | 3-way handshake                                                                                                                                          |
| Hvilken port er standard for SSH?                                 | 22                                                                                                                                                       |
| Hvilken port er standard for https?                               | 443                                                                                                                                                      |
| Hvilken protokol hører port 53 til?                               | DNS                                                                                                                                                      |
| Hvilken port kommunikerer OpenVPN på?                             | 1194                                                                                                                                                     |
| Er FTP krypteret?                                                 | Nej                                                                                                                                                      |
| Hvad gør en DHCP server/service?                                  | Tildeler automatisk ledig ip adresser til enheder på netværket.                                                                                          |
| Hvad gør DNS?                                                     | Den protokol som oversætter IP-adresser til ord.                                                                                                         |
| Hvad gør NAT?                                                     | Gør det muligt for flere enheder at dele den samme WAN-adresse.                                                                                          |
| Hvad er en VPN?                                                   | Krypteret forbindelse mellem netværk                                                                                                                     |
| Hvilke frekvenser er standard i WIFI?                             | 2.4 og 5 ghz                                                                                                                                             |

