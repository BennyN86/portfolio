# Uge 44 - Programmering med SQL

### SQl Syntaks og basisviden
SQL står for "Structured Query Language", og er sproget der bruges til at snakke med relationelle databaser.

**Vigtigste SQL-kommandoer:**

* SELECT - extracts data from a database.
* UPDATE - updates data in a database.
* DELETE - deletes data from a database.
* INSERT INTO - inserts new data into a database.
* CREATE DATABASE - creates a new database.
* ALTER DATABASE - modifies a database.
* CREATE TABLE - creates a new table.
* ALTER TABLE - modifies a table.
* DROP TABLE - deletes a table.
* CREATE INDEX - creates an index (search key).
* DROP INDEX - deletes an index.

---

### [Øvelse 9 - Programmering med database](https://ucl-pba-its.gitlab.io/24e-its-intro/exercises/9_intro_opgave_python_db/)

**Information:**
I denne øvelse skal du bruge python til at lave et program der kan interagere med en relationel sqlite3 database ved hjælp af CRUD og SQL.
Python bruger sqlite3 biblioteket til at interagere med sqlite databaser.

Hvis du behov for at åbne din database i et gui program er der et her https://sqlitebrowser.org/

**Instruktioner**
Recap SQL syntax ved at prøve denne quiz https://www.w3schools.com/sql/sql_quiz.asp
Læs om databaser og sql i dette kapitel https://www.py4e.com/html3/15-database
Læs om sqlite biblioteket https://docs.python.org/3/library/sqlite3.html


Brug dette eksempel som udgangspunkt for dit første database program. Analyser eksemplet inden du går til næste punkt.

```py title="sqlite3_example.py"
import sqlite3
from pathlib import Path # read https://realpython.com/python-pathlib/#creating-paths
files_path = Path(str(Path.cwd()) + '/databases/')
print(files_path)

# Create and connect to database
conn = sqlite3.connect(files_path / 'music.db')

cur = conn.cursor()

# Create table in database
with conn:
    cur.execute('DROP TABLE IF EXISTS Tracks')
    cur.execute('CREATE TABLE Tracks (title TEXT, plays INTEGER)')

# Insert rows in tracks table
with conn:
    cur.execute('INSERT INTO Tracks (title, plays) VALUES (?, ?)', ('Thunderstruck', 20))
    cur.execute('INSERT INTO Tracks (title, plays) VALUES (?, ?)', ('My Way', 15))

# info to user
print('All rows in the Tracks table:')

# select values from table
cur.execute('SELECT title, plays FROM Tracks')

# print out the values
for row in cur:
    print(row)
cur.execute('DELETE FROM Tracks WHERE plays < 100')

# Close database connection
conn.close()
```

* Udvid programmet til at opdatere en af linjerne i tracks tabellen.
>Følgende kode opretter en funktion til at opdatere antallet af afspilninger for et track.  
>Efterfølgende kaldes funktionen og afspilninger sættes til 101:
```py
# Function to update a row in the Tracks table
def update_track(title, new_plays):
    with conn:
        cur.execute('UPDATE Tracks SET plays = ? WHERE title = ?', (new_plays, title))

# Update a track
update_track('Thunderstruck', 101)
```
* Udvid programmet til at lave en ny tabel med et valgfrit antal felter.
> Følgende kode opretter en en funktion til at oprette en ny tabel, og derfter oprettes tabellen "Albums" med kolonnerne 'title' og 'artist':
```py
# Function to create a new table with a variable number of fields
def create_table(table_name, fields):
    with conn:
        fields_str = ', '.join([f"{field_name} {field_type}" for field_name, field_type in fields.items()])
        cur.execute(f'CREATE TABLE IF NOT EXISTS {table_name} ({fields_str})')

# Create a new table 'Albums' with fields 'title' and 'artist'
create_table('Albums', {'title': 'TEXT', 'artist': 'TEXT'})
```
* Indsæt 10 linjer i den nye tabel
>
```py
# Insert rows in Albums table
with conn:
    cur.execute('INSERT INTO Albums (title, artist) VALUES (?, ?)', ('Call of the Wild', 'D-A-D'))
    cur.execute('INSERT INTO Albums (title, artist) VALUES (?, ?)', ('D.A.D. Draws a Circle', 'D-A-D'))
    cur.execute('INSERT INTO Albums (title, artist) VALUES (?, ?)', ('No Fuel Left for the Pilgrims', 'D-A-D'))
    cur.execute('INSERT INTO Albums (title, artist) VALUES (?, ?)', ('Riskin It All', 'D-A-D'))
    cur.execute('INSERT INTO Albums (title, artist) VALUES (?, ?)', ('Helpyourselfish', 'D-A-D'))
    cur.execute('INSERT INTO Albums (title, artist) VALUES (?, ?)', ('Simpatico', 'D-A-D'))
    cur.execute('INSERT INTO Albums (title, artist) VALUES (?, ?)', ('Everything Glows', 'D-A-D'))
    cur.execute('INSERT INTO Albums (title, artist) VALUES (?, ?)', ('Soft Dogs', 'D-A-D'))
    cur.execute('INSERT INTO Albums (title, artist) VALUES (?, ?)', ('Scare Yourself', 'D-A-D'))
    cur.execute('INSERT INTO Albums (title, artist) VALUES (?, ?)', ('Monster Philosophy', 'D-A-D'))
```
* Hent linjerne og udskriv dem i alfabetisk orden.
>
```py
# Select values from table in alphabetical order
cur.execute('SELECT title, artist FROM Albums ORDER BY title')

# print out the values
for row in cur:
    print(row)
```
* Slet en af linjerne.
>
```py
# delete row in albums table
cur.execute('DELETE FROM Albums WHERE title = "Monster Philosophy"')
```

---
