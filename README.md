## Zespół:

### Kamil Kurowski
### Mateusz Pieróg



# Projekt z baz danych


## Ogólny zarys

* Mamy w planie stworzyć serwis z filmami
* coś w stylu “Filmweb”
* Mamy bazę filmów, osób kina oraz serwisów streamingowych
* Każdy użytkownik ma swoją bibliotekę, może oznaczać jako obejrzane itp
* Dane filmów najprawdopodobniej pochodzące z zewnętrznego API
* Sprawdzać informacje na temat filmów, osób itp.
* Wyszukiwanie w zależności od filtrów (rok, gatunek, platforma)


## Technologie

* Dokumentowa baza danych: MongoDB
* Frontend: React.js
* Node.js
* Express.js


## Dokumentacja

[Link do google docs](https://docs.google.com/document/d/1IDP3PLPie59oKxTlVEqZKJXBfDBSobYQ6-YQ4tkv9cs/edit)


## API

Do częsci endpointów wymagany token.


### `/movie/`

` GET /movie/ ` zwraca wszystkie filmy

` GET /movie/id/{ id } ` zwraca film o podanym id

` GET /movie/count/ ` zwraca liczbe filmów

` POST /movie/add/ ` dodaje nowy film

` POST /movie/id/{ id }/rate ` dodaje rate filmu o podanym id

` GET /movie/id/{ id }/title ` zwracatytul filmu o podanym id

` DELETE /movie/if/{ id }/ ` pobiera wszystkie filmy

` GET /movie/filters/{ atribute }` zwraca wszystkie możliwe wartości atrybutu, możliwe atrybuty to: `runtime, year, genre, platform`

` POST /movie/filtered/ ` zwraca filmy z uwzględnieniem filtrów przesłanych jako ciało zapytania, uwzględniając którą strone aktualnie przegląda użytkownik

` POST /movie/filtered/count/ ` zwraca liczbe filmow z względnieniem filtrów przesłanych jako ciało zapytania



### `/people/`

` GET /poeple/ ` zwraca wszystkich ludzi

` GET /poeple/page/{ number }/ ` zwraca ludzi ze strony number

` GET /poeple/count/ ` zwraca liczbe ludzi

` GET /poeple/id/{ id } ` zwraca osobe o podanym id

` POST /poeple/add/ ` dodaje osobę do bazy

` POST /filtered ` zwraca osoby z uwzględnieniem filtrów przesłanych jako ciało zapytania, uwzględniając którą strone aktualnie przegląda użytkownik

` POST /people/filtered/count/ ` zwraca liczbe osob z względnieniem filtrów przesłanych jako ciało zapytania



### `/users/`

` GET /users/ ` zwraca wszystkich użytkowników

` POST /users/add/ ` dodaje nowego użytkownika

` GET /users/username/ ` zwraca nazwę aktualnie zalogowanego użytkownika

` GET /users/user/ ` zwraca aktualnie zalogowanego użytkownika

` GET /user/profile/ ` zwraca profil użytkownika

` POST /user/profile/{ atribute }/ ` aktualizuje podany atrybut z profilu użytkownika, możliwe atrybuty to: ` firstname, lastname, description, avatar `



### `/auth/`

` POST /auth/login ` logowanie

` POST /auth/register ` rejestracja



### `/library/`

` GET /library/ ` zwraca biblioteke zalogowanego użytkownika

` GET /library/{ section }/ ` zwraca sekcję biblioteki zalogowanego użytkownika, możlwie sekcje to ` towatch, favourites, seen`



### `/libmodifying/`

Poniżej dentyfikator filmu oraz użytkownika przesyłane jako ciało zapytania.

` POST /libmodifying/{ section }/checkstate/ ` zwraca informacje czy podany film znajduję się w sekcji danego użytkownika

` POST /libmodifying/{ section }/add/ ` dodaje film do biblioteki użytkownika

` POST /libmodifying/{ section }/remove/ ` usuwa film z bilioteki użytkownika

` POST /libmodifying/rate/add/ ` dodaje ocene filmu

` POST /libmodifying/rate/value/ ` zwraca wartość oceny










## Struktura bazy danych (v.0.2 stan z dnia 27.05.2021)
### Wypełniona przykładowymi danymi

### Tabela przetrzymująca dane Użytkownika

```
"User":
{
    "_id": "60a3bcf3f5cf182ca3bd05f1",
    "email": "asdfg@gmail.com",
    "password": "<ENCODED PASSWORD",
    "firstName": "Jan Julian",
    "lastName": "Kowalski",
    "createdAt": "2021-05-18T13:11:15.924Z",
    "updatedAt": "2021-05-18T13:11:15.924Z",
    "__v": 0
    "library": {
    "toWatch": [
    {
      "movieID": "235235",
      "title": "to watch 1"
    }
  ],
  "favourites": [
    {
      "movieID": "235235",
      "title": "fav 1"
    }
  ],
  "seen": [
    {
      "movieID": "235235",
      "title": "seen 1"
    },
    {
      "movieID": "3453",
      "title": "seen 2",
      "rate": 4
    },
    {
      "movieID": "326",
      "title": "seen 3",
      "rate": 2
    }
  ]
}
    
```

### Tabela przetrzymująca dane dotyczace filmu

```
"Movie":
 {
    "_id": "60a3ab2ebf147569236e2566",
    "title": "First Movie",
    "date": "2020-02-11T00:00:00.000Z",
    "runtime": 3,
    "rate": {
      "sum": 0,
      "amount": 0
    },
    "genre": [
      "Action"
    ],
    "plot": "Movie about this project",
    "cover": "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTFlOaeron7sa8fd9Xlv5GZZ3JOkd6piSEMpw&usqp=CAU",
    "directors": [
      {
        "directorName": "Dir Name",
        "directorID": "456456"
      }
    ],
    "writers": [
      {
        "writerName": "Writ Name",
        "writerID": "456456"
      },
      {
        "writerName": "Writ name 2",
        "writerID": "456456"
      }
    ],
    "actors": [
      {
        "actorName": "Anonymous actor"
      }
    ],
    "platforms": [
      {
        "name": "Netflix",
        "URL": "random"
      }
    ],
    "createdAt": "2021-05-18T11:55:26.019Z",
    "updatedAt": "2021-05-18T11:55:26.019Z",
    "__v": 0
  }


```
### Tabela przetrzymująca dane dotyczace osób związanych z filmami

```
"People":
 {
     "_id": "60a3c0b793fa99389e728b5b",
    "firstName": "Jan Julian",
    "lastName": "Kowalski",
    "born": "2001-05-01T00:00:00.000Z",
    "__v": 0
    "functions": {
      "director": [
        {
          "title": "gsfhsgd",
          "movieID": "235235"
        }
      ],
      "actor": [
        {
          "title": "fav1",
          "movieID": "235235"
        }
      ],
      "writer": [
        {
          "title": "seem1",
          "movieID": "235235"
        },
        {
          "title": "seen2",
          "movieID": "3453"
        },
        {
          "title": "seen3",
          "movieID": "326"
        }
      ]
    },
    "photo": "https://image.slidesharecdn.com/funnycatvideosdownload-150901043709-lva1-app6892/95/funny-cat-videos-download-1-638.jpg?cb=1441082273",
 }

```


