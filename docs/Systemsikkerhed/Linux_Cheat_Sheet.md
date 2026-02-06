For mere information om hvordan en kommando kan bruges, kan følgende tastes: 
"man *command*" eller *"command* --help".
Alternativt installer [tldr](https://github.com/tldr-pages/tlrc) for mere simple hjælpetekster. fx "tdlr *find*"

### Kommandoer

| Kommando                 | Beskrivelse                                                              |
| ------------------------ | ------------------------------------------------------------------------ |
| pwd                      | Print Working Directory. Fortæller hvilket bibliotek man befinder sig i. |
| cd                       | Change Directory. Skifter bibliotek.                                     |
| cd ..                    | Navigerer en gang op i mappestrukturen.                                  |
| cd ../..                 | Kommandoen navigerer to gange op i mappestrukturen.                      |
| cd /                     | Skifter til systemets root-directory.                                    |
| cd ~                     | Skifter til systemets home-directory.                                    |
| ls                       | Lister alle mapper og filer i det aktive bibliotek.                      |
| ls -a                    | Lister og skjulte filer og mapper.                                       |
| ls -l                    | Printer liste med detaljerede filoplysninger.                            |
| mkdir                    | Opretter en ny mappe.                                                    |
| touch                    | Opretter en ny fil.                                                      |
| nano "filnavn"           | Åbner filen i den indbyggede text-editor.                                |
| find                     | Søg efter filer. Fx på filnavn, type eller størrelse.                    |
| find *sti* -name "*.txt" | Finder alle filer af typen .txt, i det angivede directory.               |
| │ grep "søgeord"         | Pipe- grep kommandoen bruges til at filtrere sit output.                 |
| ps                       | Liste over aktive processer.                                             |
| ps -aux                  | Liste over aktive processer for alle brugere.                            |
| │ less                   | Pipe- less kommandoen printer kun output én side ad gangen. Der kan navigeres med pil op/ned eller space for at springe en side frem.  |

### Brugerstyring

| Kommando                     | Beskrivelse                                                 |
| ---------------------------- | ----------------------------------------------------------- |
| sudo useradd -m Mandalorian  | Tilføj bruger. `-m` tilføjer et homedirectory til brugeren. |
| sudo passwd Mandalorian      | Sætter et password for brugeren.                            |
| sudo userdel Mandalorian     | Fjerner en bruger. OBS på at UID kan tildeles ny bruger     |
| chmod (ugo) +/- rwx          | Tilføj og fjern rettigheder til bruger/gruppe/andre         |
| sudo chown "user" myfile.txt | Ændre ejerskab for en fil/directory                         |


### Filstruktur

| Directory  | Tiltænkte filer                                                                                          |
| ---------- | -------------------------------------------------------------------------------------------------------- |
| **/bin**   | Indeholder essentielle brugerkommandoer (binære filer), såsom `ls`, `cp` og `cat`.                       |
| **/boot**  | Indeholder filer nødvendige for opstart af systemet, inkl. bootloader og kernel.                         |
| **/dev**   | Indeholder enhedsfiler for hardware og virtuelle enheder, f.eks. `sda` (harddisk) og `tty` (terminaler). |
| **/etc**   | Indeholder konfigurationsfiler for systemet og installerede programmer.                                  |
| **/home**  | Indeholder brugernes personlige mapper og data.                                                          |
| **/media** | Bruges til midlertidig montering af eksterne enheder, f.eks. USB-drev og cd-rom.                         |
| **/lib**   | Indeholder delte biblioteker (shared libraries) nødvendige for systemets funktion.                       |
| **/mnt**   | Bruges midlertidigt til montering af filsystemer, f.eks. netværksdrev eller ekstra partitioner.          |
| **/misc**  | Bruges ofte til diverse montering af enheder eller specialfiler, afhængigt af distributionen.            |
| **/proc**  | Et virtuelt filsystem, der indeholder information om kørende processer og systemstatus.                  |
| **/opt**   | Indeholder tredjeparts softwarepakker, der ikke er en del af standardinstallationen.                     |
| **/sbin**  | Indeholder systemadministrationsværktøjer og binære filer, som typisk kun bruges af root.                |
| **/root**  | Hjemmemappe for superbrugeren (root).                                                                    |
| **/tmp**   | Indeholder midlertidige filer, som ofte slettes ved genstart.                                            |
| **/usr**   | Indeholder brugerapplikationer, biblioteker og dokumentation, typisk opdelt i `bin`, `lib`, `share` osv. |
| **/var**   | Indeholder variable data, såsom logs, cache, mail- og spool-filer.                                       |


### Logning

| Kommando                     | Beskrivelse                          |
| ---------------------------- | ------------------------------------ |
| grep Network /var/log/syslog | Søg efter specifikt udtryk i syslog. |
| tail -n 100 /var/log/syslog  | Se de sidste 100 linjer i loggen.    |
| tail -f /var/log/syslog      | Se løbende hændelser i loggen.       |
| journalctl -u ssh            | Loghændelser specifikt for SSH       |
