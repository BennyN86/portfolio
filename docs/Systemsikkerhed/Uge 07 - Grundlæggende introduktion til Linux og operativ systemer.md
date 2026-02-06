# Uge 07 - Grundlæggende Introduktion

### Forberedelse

**Noter fra PDF: *"1- Introduction To OS.pdf"***

* Et operativ system er software som fungerer som distributør og oversætter, mellem hardware og software (apps) der kører på systemet. 
* Et OS administrere og distribuere et systems ressourcer. Det er fx CPU-processer, hukommelse, filer/lagerplads og administration af enheder som mus, tastatur, skærme osv.
* Et OS består typisk af tre komponenter: 
	* En "Kernel" håndterer *"low-level"* interaktionen direkte med hardwaren.
	* En "shell" fungerer som brugerflade mellem bruger og system. Fx en CLI eller GUI.
	* "Drivere", der gør det muligt for OS at kommunikere med hardware.
* I forhold til sikkerhed, håndterer et OS, autentificering og autorisation.

### Opgave 3 - Navigation i Linux

| Opgave                                                                                                           | Hvad sker der?                                                                                                                           |
| ---------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Eksekver kommandoen `pwd`                                                                                        | Skriver den aktuelle placering i filsystemet. Print Working Directory.                                                                   |
| Eksekver kommandoen `cd /`                                                                                       | Skifter placering til systemets root-directory. (cd = change directory)                                                                  |
| Lokationen `/` har en bestemt betydning i Linux-filsystemet. Hvad er det?                                        | Systemets root-directory. Altså det første bibliotek i filsystemet.                                                                      |
| Eksekver kommandoen `cd /etc/ca-certificates/`. Hvad findes der i directoriet?                                   | Systemets CA-certifikater. Og filer og mapper til håndtering af dem.                                                                     |
| Hvor mange _directories_ viser outputtet fra `pwd` kommandoen når den eksekveres i directoriet fra forrige trin. | 2 - /etc/ca-certificates                                                                                                                 |
| Eksekver kommandoen `cd ../..`                                                                                   | Kommandoen navigerer to gange op i mappestrukturen.                                                                                      |
| Hvor mange _directories_ viser outputtet fra `pwd` kommandoen nu?                                                | 1. "/" altså root.                                                                                                                       |
| Eksekver kommandoen `cd ~` (Karakteren hedder tilde)                                                             | Navigerer til systemets home-directory.                                                                                                  |
| Kommandoen `~` er en "genvej" i Linux, hvad er det en genvej til?                                                | Systemets home-directory. Brugerens default bibliotek.                                                                                   |
| I filsystemets rod (`/`), eksekver kommandoen `ls`.                                                              | Printer en liste over alle tilgængelige mapper og filer i det aktuelle directory.                                                        |
| I brugerens _home directory_, eksekver kommandoen `touch helloworld.txt`.                                        | Opretter en fil med navnet *"helloworld.txt"*.                                                                                           |
| I brugerens _home directory_, eksekver kommandoen `nano helloworld.txt`.                                         | Åbner filen i terminalens indbyggede text-editor: Nano.                                                                                  |
| List alle filer og mapper i brugerens _home directory_.                                                          | Det gøres med kommandoen `ls`, og viser filen helloworld.txt.                                                                            |
| List alle filer og mapper i brugerens _home directory_ med flaget `-a`.                                          | Lister også skjulte mapper og filer i biblioteket.                                                                                       |
| List alle filer og mapper i brugerens _home directory_ med flaget `-l`.                                          | Printer detaljerede oplysninger. Her kan ses hvem der ejer filen, hvad den fylder og hvilke tilladelser filen har. Er den fx executable. |
| List alle filer og mapper i brugerens _home directory_ med flaget `-la`.                                         | Tilføjelsen af `a`, gør at skjulte mapper og filer tilføjes den detaljerede oversigt.                                                    |
| I brugerens _home directory_, eksekver kommandoen `mkdir helloWorld`.                                            | Opretter en ny mappe med navnet *"helloWorld"*                                                                                           |
| Eksekver kommandoen `ls -d`.                                                                                     | Viser kun mappenavne og ikke deres indhold. Fx `ls -d */`, vil vise alle mapperne i det aktuelle directory, men skjuler filerne.         |
| Eksekver kommandoenn `ls -f`.                                                                                    | Lister filer og mapper uden at sortere dem. Printer også skjulte filer og mapper.                                                        |

### Opgave 6 - Søgning i Linux filstrukturen

| Opgave                                                                                                                                                  | Hvad sker der?                                                                                                                              |
| ------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------- |
| I `Home directory`, eksekver kommandoen `find`.                                                                                                         | Outputtet er en lang liste med fulde stier til alle filer og mapper i din home-mappe                                                        |
| I `Home directory`, eksekver kommandoen `find /etc/`.                                                                                                   | Laaang liste over fulde stier til alle filer og mapper i /etc/ mappen.                                                                      |
| Eksekver kommandoen `sudo find /etc/ -name passwd`. _Sudo foran kommandoen betyder, at kommandoen eksekveres med de rettigheder, som sudo-gruppen har._ | Outputtet viser stien til de filer/mapper i /etc-folderen, som med navnet "passwd"                                                          |
| Eksekver kommandoen `sudo find /etc/ -name pasSwd`. **Husk stort S**.                                                                                   | Finder ingen filer, da søgningen er case-sensitive.                                                                                         |
| Eksekver kommandoen `sudo find /etc/ -iname pasSwd`. **Husk stort S**.                                                                                  | Finder resultaterne med "passwd", da søgningen nu ikke længere er case-sensitive. (-iname)                                                  |
| Eksekver kommandoen `sudo find /etc/ -name pass*`.                                                                                                      | Finder alle filer og mapper som starter med "pass". (* = wildcard.)                                                                         |
| I `Home directory`, eksekver kommandoen `truncate -s 6M filelargerthanfivemegabyte`                                                                     | Opretter en fil på 6 MB.                                                                                                                    |
| I `Home directory`, eksekver kommandoen `truncate -s 4M filelessthanfivemegabyte`.                                                                      | Opretter en fil på 4 MB.                                                                                                                    |
| I roden (/), eksekver kommandoen `find /home -size +5M`.                                                                                                | Viser alle filer i home-directory, med en størrelse over 5 MB.                                                                              |
| I roden (/), eksekver kommandoen `find /home -size -5M`.                                                                                                | Viser alle filer i home-directory, med en størrelse under 5 MB.                                                                             |
| I `Home directory`, opret to directories, et der hedder `test` og et andet som hedder `test2`.                                                          | `mkdir test` og `mkdir test2`                                                                                                               |
| I `test2`, skal der oprettes en fil som hedder `test`.                                                                                                  | `cd test2` og `touch test`                                                                                                                  |
| I `Home directory`, eksekver kommandoen `find -type f -name test`.                                                                                      | Finder filen med navnet "test" i mappen "test 2". På grund af flaget `-type f` ignoreres mappen "test", da ser kun søges efter typen filer. |
| I `Home directory`, eksekver kommandoen `find -type d -name test`.                                                                                      | I denne søgning ignoreres filer, da der kun søges efter mapper `-type d`. Derfor viser output kun stien til mappen "test".                  |

### Opgave 7 - Ændring og søgning i filer


| Opgave                                                                     | Hvad sker der?                                                                                      |
| -------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------- |
| I `Home directory`, eksekver kommandoen `touch minfile.txt`                | Opretter filen `minfile.txt`                                                                        |
| I `Home directory`, eksekver kommandoen `cp minfile.txt kopiafminfile.txt` | Opretter en kopi af filen. Først vælges filen man vil kopiere, efterfølgende navnet på den nye fil. |
| I `Home directory`, eksekver kommandoen `mkdir minfiledir`                 | Opretter en mappe med navnet `minfiledir`                                                           |
| I `Home directory`, eksekver kommandoen, `mv minfile.txt minfiledir`       | Flytter filen `minfile.txt` til mappen `minfiledir`.                                                |
| I `Home directory`, eksekver kommandoen, `rm -r minfiledir`                | Sletter mappen og dens indhold.                                                                     |

I de næste trin skal der arbejdes med oprettelse af filer med tekst indhold, her bliver redirect operatoren introduceret (>). Operatoren tager outputet fra kommandoen på venstre side og skriver til filen på højre side.

| Opgave                                                                                      | Hvad sker der?                                                                                                 |
| ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| I `Home directory`, eksekver kommandoen `echo "hej verden" > hejverdenfil.txt`              | Indsætter teksten "hej verden" i filen `hejverdenfil.txt`. Hvis ikke filen findes i forvejen, så oprettes den. |
| I `Home directory`, eksekver kommandoen `cat hejverdenfil.txt`                              | Printer indholdet af filen.                                                                                    |
| I `Home directory`, eksekver kommandoen `echo "hej ny verden" > hejverdenfil.txt`           | Overskriver indholdet i filen.                                                                                 |
| I `Home directory`, eksekver kommandoen `cat hejverdenfil.txt`                              | Printer indholdet af filen.                                                                                    |
| I `Home directory`, eksekver kommandoen `echo "hej endnu en ny verden" >> hejverdenfil.txt` | Tilføjer sætningen til filen.                                                                                  |
| I `Home directory`, eksekver kommandoen `cat hejverdenfil.txt`                              | Printer indholdet af filen.  


I de næste trin introduceres pipe operatoren (|). Den tager outputtet fra kommandoen på venstre side, og giver det videre til kommandoen på højre side.

| Opgave                                                          | Hvad sker der?                                                                                                                                                          |
| --------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| I `/etc/`, eksekver kommandoen `cat adduser.conf`.              | Printer hele indholdet af filen.                                                                                                                                        |
| I `/etc/`, eksekver kommandoen `cat adduser.conf \| grep 1000`. | Printer linjer i filen som indeholder tallet `1000`.                                                                                                                    |
| I `/etc/`, eksekver kommandoen `cat adduser.conf \| grep no`.   | Printer linjer i filen som indeholder ordet `no`.                                                                                                                       |
| I `/`, eksekver kommandoen `grep no /etc/adduser.conf`          | Det samme som kommandoen herover.                                                                                                                                       |
| I `/`, eksekver kommandoen `ls -al \| grep proc`                | `ls -al` printer en en detaljeret liste over alle synlige og skjulte filer. Med `grep \| proc` filtreres output så der kun printes filer og mapper med ordet "proc"     |
| I `/etc/`, eksekver kommandoen `ls -al \| grep shadow`          | `ls -al` printer en en detaljeret liste over alle synlige og skjulte filer. Med `grep \| shadow` filtreres output så der kun printes filer og mapper med ordet "shadow" |
