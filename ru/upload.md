# Загрузка и распознавание файлов
<a name="parsing"></a>

`POST /account/{account_id}/upload` 

Для загрузки файла необходимо отправить запрос `multipart/form-data` c файлом в параметре `file`.

Для того, чтобы файл также был обработан системой распознавания полей, необходимо передать заголовок `X-File-Parse: true`. В этом случае, в ответе будут присутствовать поля `text`, `photo`, `fields`. 

```json
{
    "id": 1329115,
    "name": "Глибин Виталий Николаевич.pdf",
    "content_type": "application/pdf",
    "url": "https://store.huntflow.ru/uploads/t/f/9/tf96318hrjj5w9o5m9xuvwwwzbgbgave.pdf",
    "text": "Текст резюме",
    "photo": {
        "id": 1352599,
        "name": "image1.png",
        "url": "https://store.huntflow.ru/uploads/6/p/p/6ppzo93d8ic8slkzy7flrabqemnwtzpq.png",
        "content_type": "image/png"
    },
    "fields": {
        "position": "Ведущий frontend разработчик (team lead)",
        "email": "glibin.v@gmail.com",
        "salary": 200000,
        "name": {
            "middle": "Николаевич",
            "last": "Глибин",
            "first": "Виталий"
        },
        "phones": [
            "+7 (926) 073-17-78"
        ],
        "birthdate": {
            "month": 7,
            "day": 20,
            "precision": "day",
            "year": 1984
        },
        "experience": [
            {
                "position": "Team lead команды мобильного сайта",
                "company": "HeadHunter"
            },
            {
                "position": "Senior frontend developer",
                "company": "HeadHunter"
            }
        ]
    }
}
```


Имя | Тип | Описание
--- | --- | ---
id | число | Идентификатор загруженного файла
name | строка | Название загруженного файла
content_type | строка | Тип файла
url | строка | Прямая ссылка на файл в Хантфлоу
text | строка | Текстовое представление файла (если резюме имеет текстовый формат) 
photo | объект | Объект с файлом-фотографией, если такая нашлась в распознаваемом файле иначе `null` 
fields | объект | Объект с распознаными данными (описание полей ниже) 

 
### Поля из системы распознавания

Любое из указанных полей может отсутствовать
 
Имя | Тип | Описание
--- | --- | ---
name | объект | Объект с полями ФИО
position | строка | Желаемая должность
email | строка | Адрес электронной почты
salary | число | Размер заработной платы
phones | массив | Массив телефонов
birthdate | объект | Объект даты рождения. В поле `precision` указывается точность определения возраста.
experience | массив | Массив с опытом работы кандидата
