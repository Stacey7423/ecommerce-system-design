# Спецификация REST API для сценария просмотра витрины товаров и добавления в корзину

## Задача:
спроектировать REST API следующего сценария для приложения интернет-магазина: отобразить витрину товаров (список товаров с кратким описанием), перейти с витрины на страницу конкретного товара с детальным описанием, добавить товар в корзину. Решение должно включать описание запросов и описание и/или пример ответа для каждого из запросов в формате JSON. По составу полей товара ориентироваться на любой известный интернет-магазин.

## Описание:
в основном ориентировалась на интернет-магазины вроде Ozon, также указан защищённый протокол HTTPS. Для запросов 1, 2 предполагается, что авторизация не обязательна, т.к. это публичные данные. Для запроса 3 авторизация показана через передачу клиентом серверу токена авторизации.

## Запрос 1: Отображение витрины товаров

### Описание запроса: 
GET /products - получить список товаров с кратким описанием в формате JSON (задан с помощью Content-Type), используется метод GET, 
URL-адрес ресурса: https://www.online-store.ru/products
URI: https://www.online-store.ru
URN: /products

Возможен вариант с пагинацией:
GET /products?page=<page_number>&size=<size>

### Пример ошибки при выполнении неправильного запроса:
GET https://www.online-store.ru/products?page=abc&size=10


**Статус:** 400 Bad Request
```json
{
    "message": "Invalid pagination parameters"
}
```
### Внутренняя ошибка сервера:
**Статус:** 500 Internal Server Error

### Пример ответа при успешном выполнении запроса:
GET https://www.online-store.ru/products
Content-Type: application/json; charset = UTF-8

Ответ сервера:
**Статус:** 200 OK
```json
{
"products": [
    {
        "id": 1,
        "images": [
            "https://www.online-store.ru/product-images/pharmacy/optics/alcon/alcon-dailies-total1/alcon-dailies-total1-30/1.webp",
            "https://www.online-store.ru/add-images/sale-image-red.webp"
        ],
        "title":" Alcon Контактные линзы DAILIES TOTAL1, 30 шт., -3.75 / 8.5/ 1 день, однодневные",
        "price":3060,
        "discountPrice":2145,
        "discountPercentage":29.9,
        "brand": "Alcon",
        "original": true,
        "averageRating":4.9,
        "countReviews":38488
        }
],
"totalPages": 5,
"currentPage": 1,
"totalProducts": 100
}
```
## Запрос 2: Отображение страницы товара с детальным описанием

### Описание запроса: 
GET /products/<product-id> - получить информацию конкретного товара с детальным описанием в формате JSON (задан с помощью Content-Type), метод: GET,
URL: https://www.online-store.ru/products/<product-id>
URI: https://www.online-store.ru
URN: /products/<product-id>

### Пример ошибки при выполнении неправильного запроса:
GET https://www.online-store.ru/products/5%7
Content-Type: application/json; charset = UTF-8

**Статус:** 400 Bad Request
```json
{
    "message": "Invalid product id '5%7'"
}
```
### Пример ошибки при отсутствии ресурса, указанного в запросе:
GET https://www.online-store.ru/products/9205
Content-Type: application/json; charset = UTF-8

**Статус:** 404 Not Found
```json
{
    "message": "Product with id '9205' not found"
}
```
### Внутренняя ошибка сервера:
**Статус:** 500 Internal Server Error

### Пример ответа при успешном выполнении запроса:
GET https://www.online-store.ru/products/1
Content-Type: application/json; charset = UTF-8

Ответ сервера:
**Статус:** 200 OK
Поля:
- `videos` - список ссылок на видео
- `answers` - список объектов-ответов с указанием автора, даты публикации и текста
```json
{
    "id": 1,
    "images": [
    "https://www.online-store.ru/product-images/pharmacy/optics/alcon/alcon-dailies-total1/alcon-dailies-total1-30/1.webp",
    "https://www.online-store.ru/add-images/sale-image-red.webp"
],
"title": "Alcon Контактные линзы DAILIES TOTAL1, 30 шт., -3.75 / 8.5/ 1 день, однодневные",
"price":3060,
"discountPrice": 2145,
"discountPercentage": 29.9,
"brandImage": "https://www.online-store.ru/brand-images/alcon/1.webp",
"brand": "Alcon",
"original": true,
"averageRating": 4.9,
"countReviews": 38488,
"countQuestions": 469,
"characteristics": {
    "title": "О товаре",
    "articul": 137764410,
    "type": "Контактные линзы",
    "brand": "Alcon",
    "opticalForce": -1.75,
    "curvatureRadius": 8.5,
    "diameter": 14.1,
    "moistureContent": 0.8,
    "oxygenTransmission": 156,
    "material": "Делефилкон А",
    "quantityPerPack": 30,
    "features": ["3D-увлажнение"],
    "shelfLife": 1080,
    "wearingMode": 24,
    "manufacturerCountry": "США"
},
"description": {
    "images": [
        "https://www.online-store.ru/product-images/pharmacy/optics/alcon/alcon-dailies-total1/alcon-dailies-total1-30/1.webp",
        "https://www.online-store.ru/product-images/pharmacy/optics/alcon/alcon-dailies-total1/alcon-dailies-total1-30/2.webp",
        "https://www.online-store.ru/product-images/pharmacy/optics/alcon/alcon-dailies-total1/alcon-dailies-total1-30/3.webp"
    ],
        "descriptionText": "…",
        "tags": [
            "#линзы",
            "#однодневные_линзы",
            "#линзы_контактные_однодневные",
            "#линзы_alcon",
            "#dailies_total1"
        ]
},
    "reviews": [
    {
        "author": "Ксения Б.",
        "datePublished": "2025-10-12T04:35:12.072Z",
        "rating": 5,
        "images": ["https://online-store.ru/reviews/1/images/1.webp"],
        "videos": [],
        "reviewText": "Брала для пробы. Норм",
        "answers": [],
        "reviewHelpfulCount": 0,
        "reviewUnhelpfulCount": 0,
        }
],
"questions": [
    {
        "author": "Анна К.",
        "publishedDate": "2025-03-20T11:02:14.403Z",
        "questionText": " Здравствуйте. Подскажите пожалуйста, у линз есть понятие левая и правая? Как это указано? И Подскажите, необходимо ли в течение дня увлажнять глаз вместе с линзой каплями?",
        "likesCount": 1,
        "answerText": "Добрый день, Анна! Нет, линзы Alcon Dailies Total 1 не имеют отличий для левого и правого глаза. Увлажнять глаз вместе с линзой каплями не требуется, можно носить до 16 часов в сутки без потери комфорта.",
        "answerHelpfulCount": 16,
        "answerUnhelpfulCount":0
    }
]
}
```
## Запрос 3: Добавление товара в корзину

### Описание запроса:
POST /cart/items 
Content-Type: application/json; charset = UTF-8
Authorization: Bearer <token>
```json
{
"items": [
    {
        "id": "<product_id>",
        "count": "<count>",
    }
]
}
```
– добавить позицию заказа в корзину с указанием токена авторизации пользователя, товара для добавления (идентификатор товара, количество) в формате JSON (указан в Content-Type), метод: POST
URL: https://www.online-store.ru/cart/items
URI: https://www.online-store.ru
URN: /cart/items

### Пример ошибки при некорректных данных пользователя:
POST https://www.online-store.ru/cart/items
Content-Type: application/json; charset = UTF-8
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```json
{
"items": [
    {
        "id": 1,
        "count": 4,
    }
]
}
```
### Пример ошибки авторизации
**Статус:** 401 Unauthorized
```json
{
"message": "Invalid or expired token"
}
```
### Пример ошибки при неправильных данных в теле запроса:
POST https://www.online-store.ru/cart/items
Content-Type: application/json; charset = UTF-8
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```json
{
"items": [
    {
        "id": 1,
        "count": -10,
    }
]
}
```
**Статус:** 400 Bad Request
```json
{
"message": "Invalid count parameter"
}
```
### Пример ошибки при отсутствии ресурса, указанного в запросе:
POST https://www.online-store.ru/cart/items
Content-Type: application/json; charset = UTF-8
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```json
{
"items": [
    {
        "id": 9205,
        "count": 4,
    }
]
}
```
**Статус:** 404 Not Found
```json
{
"message": "Product with id '9205' is not found"
}
```
### Внутренняя ошибка сервера:
**Статус:** 500 Internal Server Error

### Пример ответа при успешном выполнении запроса:
POST https://www.online-store.ru/cart/items
Content-Type: application/json; charset = UTF-8
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```json
{
"items": [
    {
        "id": 1,
        "count": 4,
    }
]
}
```
Ответ сервера:
**Статус:** 201 Created
```json
{
"items": [
    {
        "id": 1,
        "title": "Alcon Контактные линзы DAILIES TOTAL1, 30 шт., -3.75 / 8.5/ 1 день, однодневные",
        "price": 3060,
        "count": 4,
        "totalPrice": 12240,
        "discountPercentage": 29.9,
        "discountedTotalPrice": 8580
    }
],
"totalCount": 4,
"totalOrderPrice": 12240,
"discountOrder": 3660,
"discountedOrderPrice": 8580
}
```
