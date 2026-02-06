# Uge 41 - Domæne primitiver, samt 3. parts sårbarheder.

**Reflektions punkter efter forberedelsen og inden undervisningen:**

- Hvad er et system primitiv?
> Grundlæggende datatyper som fx `int`, `string` og `float`.  

- Hvordan adskiller domæne primitiver sig fra system primitiver?
> Domæne primitiver er mere end bare simple datatyper; de inkluderer regler (invarians) og validering. System primitiver har ingen domænespecifik betydning eller logik.

- Er opretholdelse af invarians et krav i et domæne primitiv? Og hvis ja, hvornår skal invarians så håndhæves?
> Ja, opretholdelse af invarians er et krav i et domæne primitiv.  
Invarians skal håndhæves på tidspunktet for skabelsen af domæne primitivet. Det betyder, at et domæne primitiv kun kan eksistere, hvis det er validt i henhold til de fastlagte regler for dets domæne.

- Bør man som udgangspunkt altid kunne stole på en domæne primitives integritet? (Er dataen den indeholder valid?)
> Ja, man bør altid kunne stole på en domæne primitives integritet.  
Eftersom domæne primitiver håndhæver validering og invarians ved skabelse, vil enhver gyldig instans af en domæne primitiv altid indeholde valid data.

- Er der et mønster man kan bruge til at minimere risikoen for at sensitiv data såsom passwords logges?
> Ja, mønsteret "read-once object" kan bruges til at minimere risikoen for, at sensitiv data som passwords logges.  
Et read-once object sikrer, at data kun kan læses én gang, hvorefter den slettes fra hukommelsen, hvilket forhindrer utilsigtet eksponering eller logning af dataen.

- Hvorfor er brugen af domæne primitiver et godt supplement til input validering?
> Brugen af domæne primitiver sikrer, at data altid er valideret på en konsistent måde, da valideringen sker i selve domæne primitiven.  
Dette reducerer risikoen for fejl, hvor input ikke valideres korrekt, fordi valideringen altid håndteres af primitiveklassen og ikke overlades til hver enkelt funktion eller metode, der bruger dataen.

--- 

### [Øvelse 25 - Grundlæggende øvelse med domæneprimitiver](https://24e-its-software-sikkerhed-ucl-pba-its-16896c745213acc3eaef8347.gitlab.io/exercises/25_Person_With_domain_primitives/#information)

Formålet med denne øvelse er at give en grundlæggende introduktion til domæneprimitiver. Et domæneprimitiv er en klasse, som repræsenterer en attribut (Value Object i DDD) i kildekoden.

Øvelsen består i at oprette domænemodellen "Person" indeholdende de fire domæneprimitiver:
* Firstname
* Lastname 
* Age
* CPR-Nummer

Hvert domæneprimitiv skal opretholde egen invarians. Herunder følger reglerne og koden for hvert domæneprimitiv.

Firstname og Lastname må kun indeholde alfabetiske tegn og skal have en længde på mellem 2 og 20 karakterer.

**Firstname.cs**
```C#
using System.Text.RegularExpressions;


public class Firstname

{
    private string firstname;


    public Firstname(string firstname)
    {
        if (!IsValidFirstname(firstname))
        {
            throw new ArgumentException("Firstname must contain between 2 and 20 alphabetic characters.");
        }
        this.firstname = firstname;
    }

    public string GetValue()
    {
        return firstname;
    }

    private bool IsValidFirstname(string firstname)
    {
        if (string.IsNullOrWhiteSpace(firstname) || firstname.Length < 2 || firstname.Length > 20) // Fornavn skal være mellem 2 og 20 tegn
        {
            return false;
        }

        // Regex der sikrer, at navnet kun indeholder bogstaver

        Regex namePattern = new Regex("^[a-zA-Z]+$");
        return namePattern.IsMatch(firstname);
    }
}
```

**Lastname.cs**
```C#
using System.Text.RegularExpressions;

public class Lastname
{
    private string lastname;

    public Lastname(string lastname)
    {
        if (!IsValidLastname(lastname))
        {
            throw new ArgumentException("Lastname must contain between 2 and 20 alphabetic characters.");
        }
        this.lastname = lastname;
    }

    public string GetValue()
    {
        return lastname;
    }

    private bool IsValidLastname(string lastname)
    {
        if (string.IsNullOrWhiteSpace(lastname) || lastname.Length < 2 || lastname.Length > 20) // Efternavn skal være mellem 2 og 20 tegn
        {
            return false;
        }
        // Regex der sikrer, at efternavnet kun indeholder bogstaver

        Regex namePattern = new Regex("^[a-zA-Z]+$");
        return namePattern.IsMatch(lastname);
    }
}
```


Alderen skal være mellem 18 og 99

**Age.cs**
```csharp
public class Age

{
    private int age;


    public Age(int age)
    {
        if (age < 18 || age > 99) // Alder skal være mellem 18 og 99
        {
            throw new ArgumentException("Age must be between 18 and 99.");
        }
        this.age = age;
    }

    public int GetValue()
    {
        return age;
    }
}
```

CPR-nummeret skal overholde syntaksen for et dansk CPR-nummer (kun syntaksen, køn kan undlades).

**CPR.cs**

```C#
using System.Text.RegularExpressions;

  
public class CprNumber
{
    private string cprNumber; // CPR nummeret gemmes som en string
    private bool isRead = false; // Flag der angiver om CPR nummeret er blevet læst


    public CprNumber(string cprNumber)
    {
        if (!IsValidCpr(cprNumber)) //  Validerer CPR nummeret
        {
            throw new ArgumentException("Invalid CPR number format. The correct format is DDMMYY-XXXX.");
        }
        this.cprNumber = cprNumber;
    }


    public string GetValue()  // Returnerer CPR nummeret
    {
        if (isRead) // Hvis CPR nummeret allerede er blevet læst, kastes en exception
        {
            throw new InvalidOperationException("CPR number can only be read once.");
        }
        isRead = true;
        return cprNumber;
    }

  
    private bool IsValidCpr(string cprNumber) // Validerer CPR nummeret
    {
        // Regex der matcher DDMMYY-XXXX formatet
        Regex cprPattern = new Regex(@"^\d{2}\d{2}\d{2}-\d{4}$");
        return cprPattern.IsMatch(cprNumber);
    }
}
```
  

Nu oprettes klassen **Person.cs** indeholdende et private field for hver af de fire domæneprimitiver. Konstruktøren i Person-klassen tager imod et argument for hver af de fire domæneprimitiver og bruger argumenterne til at initialisere de private fields.

```C#
public class Person

{
    private Firstname firstname;
    private Lastname lastname;
    private Age age;
    private CprNumber cprNumber;
  
    public Person(Firstname firstname, Lastname lastname, Age age, CprNumber cprNumber)

    {
    if (firstname == null || lastname == null || age == null || cprNumber == null) // Tjekker om parametrene er null
        {
            throw new ArgumentNullException("One or more arguments are null. Firstname, Lastname, Age, and CprNumber cannot be null.");
        }
      
        this.firstname = firstname;
        this.lastname = lastname;
        this.age = age;
        this.cprNumber = cprNumber;
    }
  
    public string GetFirstname()

    {
        return firstname.GetValue();
    }


    public string GetLastname()

    {
        return lastname.GetValue();
    }


    public int GetAge()

    {
        return age.GetValue();
    }


    public string GetCprNumber()

    {
        return cprNumber.GetValue();
    }
}
```

I konsolapplikationen kan det hele nu afprøves ved at oprette objekter i Person klassen og afprøve invarians for de forskellige domæneprimitiver.

**Program.cs**
```C#
class Program

{
    static void Main(string[] args)
    {
        try
        {
            Firstname firstname = new Firstname("Benny");
            Lastname lastname = new Lastname("Nielsen");
            Age age = new Age(38);
            CprNumber cprNumber = new CprNumber("010101-1234");
  
            Person person = new Person(firstname, lastname, age, cprNumber); // nyt objekt af klassen Person med de 4 parametre

            Console.WriteLine($"Firstname: {person.GetFirstname()}");
            Console.WriteLine($"Lastname: {person.GetLastname()}");
            Console.WriteLine($"Age: {person.GetAge()}");
            Console.WriteLine($"CPR Number: {person.GetCprNumber()}"); // kan kun læses én gang

        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
        }
    }
}
```


### [Øvelse 26 - Fra DTO til model med domæneprimitiver](https://24e-its-software-sikkerhed-ucl-pba-its-16896c745213acc3eaef8347.gitlab.io/exercises/26_Person_with_domain_primitivesWebApplication/)




















### [Øvelse 27 - Opsamlende øvelse med domæneprimitiver](https://24e-its-software-sikkerhed-ucl-pba-its-16896c745213acc3eaef8347.gitlab.io/exercises/27_Domain_primitives_from_domain_model/)

I denne øvelse skal vi implementere en domænemodel for "Bil".
Modellen indeholder tre domæneprimitiver:
* Registreringsnummer
* Årgang
* Farve

Hvert domæneprimitiv skal (inden for realismens grænser) håndhæve invarians, således at det overholder reglerne fra virkelighedens domæne.

**Registreringsnummer Klassen**

I Danmark består et registreringsnummer typisk af to bogstaver efterfulgt af fem eller seks tal. Vi kan bruge en Regex til at sikre, at formatet overholdes.
```C#
using System.Text.RegularExpressions;

public class RegistrationNumber
{
    private string registrationNumber;

    public RegistrationNumber(string registrationNumber)
    {
        if (registrationNumber == null)
        {
            throw new ArgumentNullException(nameof(registrationNumber), "Registration number cannot be null.");
        }
        if (!IsValidRegistrationNumber(registrationNumber))
        {
            throw new ArgumentException("Invalid registration number format. It must be two letters followed by 5 or 6 digits.");
        }
        this.registrationNumber = registrationNumber;
    }

    public string GetValue()
    {
        return registrationNumber;
    }

    private bool IsValidRegistrationNumber(string registrationNumber)
    {
        Regex regPattern = new Regex(@"^[A-Z]{2}\d{5,6}$");
        return regPattern.IsMatch(registrationNumber);
    }
}
```

**Årgang klassen**

Årgang skal være et realistisk årstal for en bil, så vi kan sikre, at årgangen er inden for rimelige grænser (f.eks. mellem 1886, da den første bil blev opfundet, og indeværende år).
```C#
public class Year
{
    private int year;

    public Year(int year)
    {
        if (year < 1886 || year > DateTime.Now.Year)
        {
            throw new ArgumentException($"Year must be between 1886 and {DateTime.Now.Year}.");
        }
        this.year = year;
    }

    public int GetValue()
    {
        return year;
    }
}
```

**Farve klassen**
Farve skal være en gyldig farve. For simpelhedens skyld kan vi tillade en liste af tilladte farver.
```C#
public class Color
{
    private string color;
    private static readonly List<string> validColors = new List<string> { "Red", "Blue", "Green", "Black", "White", "Silver", "Gray" };

    public Color(string color)
    {
        if (color == null)
        {
            throw new ArgumentNullException(nameof(color), "Color cannot be null.");
        }
        if (!IsValidColor(color))
        {
            throw new ArgumentException($"Invalid color. Allowed colors are: {string.Join(", ", validColors)}.");
        }
        this.color = color;
    }

    public string GetValue()
    {
        return color;
    }

    private bool IsValidColor(string color)
    {
        return validColors.Contains(color);
    }
}
```

**Bil klassen**
```C#
public class Car
{
    private RegistrationNumber registrationNumber;
    private Year year;
    private Color color;

    public Car(RegistrationNumber registrationNumber, Year year, Color color)
    {
        if (registrationNumber == null)
        {
            throw new ArgumentNullException(nameof(registrationNumber), "Registration number cannot be null.");
        }
        if (year == null)
        {
            throw new ArgumentNullException(nameof(year), "Year cannot be null.");
        }
        if (color == null)
        {
            throw new ArgumentNullException(nameof(color), "Color cannot be null.");
        }

        this.registrationNumber = registrationNumber;
        this.year = year;
        this.color = color;
    }

    public string GetRegistrationNumber()
    {
        return registrationNumber.GetValue();
    }

    public int GetYear()
    {
        return year.GetValue();
    }

    public string GetColor()
    {
        return color.GetValue();
    }
}
```

Registreringsnummer: Valideres med en Regex for at sikre, at det følger formatet med to bogstaver og 5-6 cifre.
Årgang: Validerer, at årgangen er realistisk (mellem 1886 og det nuværende år).
Farve: Accepterer kun en liste over tilladte farver.

Med denne implementering har vi håndhævet realistiske regler for hvert domæneprimitiv og sikret robusthed i Bil-klassen.

### [Øvelse 28 - Detektering af sårbare afhængigheder med Dotnet CLI-værktøjet](https://24e-its-software-sikkerhed-ucl-pba-its-16896c745213acc3eaef8347.gitlab.io/exercises/28_Vulnerable_dependencies_Dotnet/)

**Information:**

Formålet med denne øvelse er at introducere tredjeparts sårbarheder og CVE'er. Det meste software i dag bygger på software, som andre har lavet tidligere. Dette importeres i projekter og omtales ofte som pakker eller eksterne biblioteker.

Når man skal lave en opgørelse over alle de 3. parts biblioteker, man har anvendt i et stykke software eller i et helt softwaresystem, omtaler man det ofte som Software Bill of Materials (SBOM).

I .NET importerer man eksterne biblioteker som NuGet-pakker. Man kan se en liste over alle anvendte eksterne biblioteker i projektets projektfil, som altid slutter på .csproj.

I denne øvelse skal du anvende CLI-værktøjet Dotnet til at scanne et .NET-projekts NuGet-pakker for sårbarheder.

Du skal klone repoet [](https://github.com/tobyash86/WebGoat.NET), som er et bevidst sårbart ASP.NET Core-projekt.

**Instruktioner:**

1. Gå ind i mappen til det klonede repository.
2. Følg instrukserne fra denne tutorial for at udføre en tredjeparts sårbarhedsskanning (se afsnittet om Dotnet CLI).  
   >`dotnet restore` og `dotnet list package --vulnerable`
4. Følg linket til den fundne sårbarhed, og noter CVE-nummeret (i toppen af siden fra linket).  
   >Scanningen finder to sårbarheder: CVE-2022-41064 (5.8) og CVE-2024-0056 (8.7)
5. Slå CVE'en op på CVE.org. Kan der findes mere information om CVE'en?
   >På cve.org kan man fx se en beskrivelse af sårbarheden, dens CVSS score, hvilke versioner der er berørte og infomration om hvordan den blev opdaget og eventuelt patchet.
6. Brug --include-transitive for at se underliggende sårbarheder (se tutorial for yderligere information).
   > Med flaget --include-transitive vises eventuelle sårbarheder i de berørte top-level pakkers dependencies. 
```shell
Project `WebGoat.NET` has the following vulnerable packages
   [net8.0]:
   Top-level Package            Requested   Resolved   Severity   Advisory URL
   > System.Data.SqlClient      4.8.3       4.8.3      Moderate   https://github.com/advisories/GHSA-8g2p-5pqh-5jmc
                                                       High       https://github.com/advisories/GHSA-98g6-xh36-x2p7

   Transitive Package                    Resolved   Severity   Advisory URL
   > NuGet.Packaging                     6.6.1      Critical   https://github.com/advisories/GHSA-68w7-72jg-6qpp
   > System.Formats.Asn1                 5.0.0      High       https://github.com/advisories/GHSA-447r-wph3-92pm
   > System.Net.Http                     4.3.0      High       https://github.com/advisories/GHSA-7jgj-8wvc-jh57
   > System.Text.Json                    7.0.0      High       https://github.com/advisories/GHSA-hh2w-p6rv-4g7w
   > System.Text.RegularExpressions      4.3.0      High       https://github.com/advisories/GHSA-cmhx-cq75-c4mj
```


### [Øvelse 29 - Skanning for tredjeparts sårbarheder med Snyk CLI-værktøjet](https://24e-its-software-sikkerhed-ucl-pba-its-16896c745213acc3eaef8347.gitlab.io/exercises/29_Vulnerable_dependencies_Snyk/)

**Information:**

Formålet med denne øvelse er blot at vise, at der findes flere værktøjer, som kan anvendes til skanning for tredjeparts sårbarheder. I denne øvelse skal værktøjet Snyk anvendes.

Du skal anvende det samme projekt som i forrige øvelse.

**Instruktioner:**

1. Du skal installere Windows package manageren Chocolatey. Følg guiden til installation [her](https://chocolatey.org/install).
2. Brug Chocolatey til at installere Snyk CLI. Se guiden [her](https://community.chocolatey.org/packages/snyk).
3. Anvend kommandoen snyk test i projektmappen Webgoat.NET folder (Altså det skal eksekveres i Webgoat.NET\Webgoat.NET).
4. Sammenlign resultaterne med dotnet scanneren.
>snyk finder den samme sårbarhed som dotnet scanneren:

```shell
PS C:\Users\benzd\OneDrive\1. Git\IT-Sikkerhed\Software Sikkerhed\WebGoat.NET\WebGoat.NET> snyk test

Testing C:\Users\benzd\OneDrive\1. Git\IT-Sikkerhed\Software Sikkerhed\WebGoat.NET\WebGoat.NET...

Tested 236 dependencies for known issues, found 11 issues, 66 vulnerable paths.


Issues to fix by upgrading:

  Upgrade System.Data.SqlClient@4.8.3 to System.Data.SqlClient@4.8.6 to fix
  ✗ Information Exposure [Medium Severity][https://snyk.io/vuln/SNYK-DOTNET-SYSTEMDATASQLCLIENT-3110424] in System.Data.SqlClient@4.8.3
    introduced by System.Data.SqlClient@4.8.3
  ✗ Unprotected Storage of Credentials [High Severity][https://snyk.io/vuln/SNYK-DOTNET-SYSTEMDATASQLCLIENT-6149433] in System.Data.SqlClient@4.8.3
    introduced by System.Data.SqlClient@4.8.3
```
