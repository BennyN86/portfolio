# Uge 44 - Unit Test & Semesteropsamling

**Reflektionspunkter efter forberedelsen og inden undervisningen:**

Hvordan kan unit testing hjælpe med at opretholde sikkerheden?  
>Når man sikrer design med unittests, er det vigtigt at teste både forventet og uforudset adfærd for at opdage potentielle sikkerhedsproblemer. Her anvendes fire forskellige testtyper:
>
>1. **Normal input testing** - Tester om designet accepterer korrekt input (fx gyldige e-mailadresser).
>2. **Boundary input testing** - Tester input på grænserne af det accepterede område (fx længde og format).
>3. **Invalid input testing** - Tester om designet korrekt håndterer ugyldig input, som fx null-værdier og uventede karakterer.
>4. **Extreme input testing** - Tester systemets modstandsdygtighed over for ekstreme input (fx meget lange strenge).
>
For eksempel skal en e-mailadresse i et hospitalsystem kun kunne indeholde alfabetiske tegn, tal og én punktum, og den skal ende på "hospital.com". Hvis valideringslogikken ikke håndterer dette korrekt, kan det resultere i sikkerhedsrisici, fx SQL-injektion eller langsom ydeevne pga. ineffektiv regex-behandling.

___

### [Øvelse 35 - Introduktion til automatiseret testing og unit tests](https://24e-its-software-sikkerhed-ucl-pba-its-16896c745213acc3eaef8347.gitlab.io/exercises/35_Basic_Unit_testing/)

**Information**
Formålet med denne øvelse er at introducere dig til det overordnede koncept bag automatiseret test af implementeret kildekode samt hvordan en unit test kan anvendes til at teste kode.

**Automatiseret test**
En automatiseret test er en metode til at verificere, at software fungerer som forventet ved at bruge specialiserede testværktøjer eller scripts til at udføre testcases. I stedet for at teste manuelt, hvor en person interagerer med softwaren, kan automatiserede tests hurtigt køre en række testscenarier uden menneskelig indblanding. En af de største fordele ved automatiserede tests er, at de relativt nemt kan eksekveres efter f.eks. ændringer i kildekoden for at sikre, at ændringerne ikke har "ødelagt" implementeringen eller skabt fejl (som kunne blive til en CVE).

**Unit test**
I faget software sikkerhed fokuserer vi på en bestemt type automatiseret test, kaldet unit test. Unit tests anvendes til at teste et specifikt stykke isoleret kildekode, typisk en metode eller funktion – deraf navnet unit test, "Testing a unit of code". Overordnet set fungerer en unit test ved, at man giver et input til den metode, man ønsker at teste, og herefter validerer, at metoden opfører sig som forventet. Opførelsen kan være den værdi, der bør returneres ved det givne input, eller den fejl (f.eks. en exception), som inputtet bør skabe.

**Instruktioner**
1. Gennemfør denne tutorial som en grundlæggende introduktion til unit testing.
2. I tutorialen implementeres et bestemt antal testcases. Hvordan mener du, at antallet og typen af testcases kan påvirke softwarekvaliteten og sikkerheden i software?
>Antallet af testcases skal være passende for at sikre funktionaliteten og teste for både ventede og uventede inputs.
3. Hvad er din forståelse af Test Driven Development (TDD), og hvordan kan det bidrage til et Secure by Design (SBD) mindset?'
>TDD betyder at man tænker testmetoderne ind, før koden skrives.
4. Tabel 8.1 fra dagens forberedelse præsenterer fire forskellige testtyper. Hvordan mener du, at den testtype, I har implementeret i tutorialen, adskiller sig fra de andre typer?  
Hvilke fordele og ulemper ser du ved at anvende netop denne testtype?
>I eksemplet anvendes der kun testtype 1 og 2.
5. Mange udviklingsteams undlader at anvende unit tests til deres implementeringer. Hvad tænker du er grunden til dette?
>Kunden vil ikke betale for det?