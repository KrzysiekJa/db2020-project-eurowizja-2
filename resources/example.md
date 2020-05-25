# Eurowizja.

| Nazwisko i imię | Wydział | Kierunek | Semestr | Grupa | Rok akademicki |
| :-------------: | :-----: | :------: | :-----: | :---: | :------------: |
| Krzysztof Jarek         | WIMiIP  | IS       |   4     | 3     | 2019/2020      |
| Krzysztof Klimczyk        | WIMiIP  | IS       |   4     | 3     | 2019/2020      |

## Projekt bazy danych
![enter image description here](https://raw.githubusercontent.com/phajder-databases/db2020-project-eurowizja-2/master/resources/eurowizja%20-%20diagram.png)

Tutaj ma znaleźć się opis projektu bazy danych. Na wstępie proszę zagnieździć obraz schematu w formie wektorowej, najlepiej plik SVG. Dodatkowo, w tej sekcji należy zawrzeć kilka przykładowych zapytań tworzących (lub w razie konieczności, modyfikujących) tabelę, tj. grupa DDL.

## Implementacja zapytań SQL

**1. Wyświetlenie zwycięscy wraz z danymi:**

    SELECT artists.Name 
    from (points INNER JOIN artists on artists.ID_points = points.ID_points)
    order by points.Score DESC limit 1;

**2. Wyświetlenie zwycięskiej trójki:**

    SELECT artists.Name, points.Score
    from (points INNER JOIN artists on artists.ID_points = points.ID_points)
    order by points.Score DESC limit 3;

**3. Wylistowanie wszystkich wykonawców według ilości przyznanych punktów:**

    SELECT artists.Name,points.Score FROM artists 
    LEFT JOIN points ON artists.ID_points=points.ID_points 
    ORDER BY points.Score DESC;

**4. Wylistowanie wszystkich wykonawców wraz z ich danymi:**

    SELECT artists.Name,people.Name,people.Surname,songs.Name,songs.Gendre,countries.Name
    FROM artists
    LEFT JOIN people ON people.ID_artist=artists.ID_artist
    LEFT JOIN countries ON countries.ID_country=artists.ID_country
    LEFT JOIN songs ON songs.ID_song=artists.ID_song
    ORDER BY artists.Name ASC;

**5. Wyświetlenie wszystkich państw i ilości uczestników dla każdego państwa:**

    SELECT countries.Name, count(countries.ID_country) AS Ilosc_uczestnikow FROM countries
    RIGHT JOIN artists ON artists.ID_country=countries.ID_country
    RIGHT JOIN people ON people.ID_artist=artists.ID_artist
    GROUP BY countries.Name
    ORDER BY Ilosc_uczestnikow DESC;

**6. Wylistowanie gatunów piosenek razem z liczbą wystąpień:**

    SELECT songs.Gendre, COUNT(songs.Gendre) AS ilosc_piosenek FROM songs
    GROUP BY songs.Gendre
    ORDER BY ilosc_piosenek DESC;

**7. Dodanie wykonawcy/zespołu:**

    INSERT INTO points VALUES(NULL,-1);
    INSERT INTO songs VALUES(NULL,"-1","-1");
    
    INSERT INTO artists
    VALUES (NULL,
            new_name,
            (SELECT countries.ID_country FROM countries WHERE countries.Name=new_country),
            (SELECT points.ID_points FROM points ORDER BY points.ID_points DESC LIMIT 1),
            (SELECT songs.ID_song FROM songs ORDER BY songs.ID_song DESC LIMIT 1));

**8. Dodanie państwa:**

    INSERT INTO countries
    VALUES(NULL,new_country);

**9. Dodanie pojedyńczej osoby:**

    INSERT INTO people
    VALUES(NULL,person_name,person_surname,
          (SELECT artists.ID_artist FROM artists WHERE artists.Name = person_art ));

**10. Zaktualizowanie nazwy i gatunku piosenki:**

    UPDATE songs
    LEFT JOIN artists ON artists.ID_song =songs.ID_song
    SET songs.Name = new_song_name,songs.Gendre = new_gendre
    WHERE artists.Name = artistName;

**11. Zaktualizowanie danych osobowych:**

    UPDATE people
    SET Name = new_name, Surname = new_surname
    WHERE (Name = old_name AND Surname = old_surname);

**12. Zaktualizowanie ilości punktów uczestnika:**

    UPDATE (points LEFT JOIN artists on artists.ID_points = points.ID_points)
    SET points.Score = new_score
    WHERE artists.Name = artistName;

**13. Usunięcie wykonawcy/zespołu:**

    DECLARE song_id_rm INT;
    DECLARE points_id_rm INT;
    
    set song_id_rm = (Select artists.ID_song from artists Where artists.Name = artist_name);
    set points_id_rm = (Select artists.ID_points from artists Where artists.Name = artist_name);
    
    DELETE FROM artists
    WHERE artists.Name = artist_name;
    
    DELETE FROM songs
    WHERE songs.ID_song = song_id_rm;
    
    DELETE FROM points
    WHERE points.ID_points = points_id_rm;

**14. Usunięcie państwa:**

    DELETE FROM countries
    WHERE countries.Name = country_name;

**15. Usunięcie pojedyńczej osoby:**

    DELETE FROM people
    WHERE people.Name = name_to_del AND people.Surname = sur_to_del;

**16. Uzupełnienie punktów dla każdego wykonawcy losową wartością z przedziału(0-1000):**

    UPDATE points SET points.Score = FLOOR(RAND() * (1000 + 1));

Tutaj należy wylistować wszystkie funkcjonalności, wraz z odpowiednimi zapytaniami SQL. W tej sekcji należy zawrzeć wyłącznie zapytania z grupy DML oraz DQL.

## Aplikacja
Tutaj należy opisać aplikację, która wykorzystuje zapytania SQL z poprzedniego kroku. Można, jednak nie jest to konieczne, wrzucić tutaj istotne snippety z Waszych aplikacji.

## Dodatkowe uwagi
W tej sekcji możecie zawrzeć informacje, których nie jesteście w stanie przypisać do pozostałych. Mogą to być również jakieś komentarze, wolne uwagi, itp.

