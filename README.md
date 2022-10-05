# CRUD: Склады и товары

# Сборка контейнера
В файл проекта .env необходимо указать свой SECRET_KEY.

# Команды
Сборка контейнера:
docker build -t stocks_crud ./

Монтируем volume для хранения БД:
docker volume create stocksdb

Запускаем контейнер с сервисом:
docker run --publish 8000:8000 --detach --name stocks_crud --mount source=stocksdb,target=/code/db stocks_crud

Создаем учетную запись admin:
docker exec -it stocks_crud python manage.py createsuperuser


# примеры API-запросов

@baseUrl = http://localhost:8000/api/v1

# создание продукта
POST {{baseUrl}}/products/
Content-Type: application/json

{
  "title": "Помидор",
  "description": "Лучшие помидоры на рынке"
}

###

# получение продуктов
GET {{baseUrl}}/products/
Content-Type: application/json

###

# обновление продукта
PATCH {{baseUrl}}/products/1/
Content-Type: application/json

{
  "description": "Самые сочные и ароматные помидорки"
}

###

# удаление продукта
DELETE {{baseUrl}}/products/1/
Content-Type: application/json

###

# поиск продуктов по названию и описанию
GET {{baseUrl}}/products/?search=помидор
Content-Type: application/json

###

# создание склада
POST {{baseUrl}}/stocks/
Content-Type: application/json

{
  "address": "мой адрес не дом и не улица, мой адрес сегодня такой: www.ленинград-спб.ru3",
  "positions": [
    {
      "product": 2,
      "quantity": 250,
      "price": 120.50
    },
    {
      "product": 3,
      "quantity": 100,
      "price": 180
    }
  ]
}

###

# обновляем записи на складе
PATCH {{baseUrl}}/stocks/4/
Content-Type: application/json

{
  "positions": [
    {
      "product": 2,
      "quantity": 100,
      "price": 130.80
    },
    {
      "product": 3,
      "quantity": 243,
      "price": 145
    }
  ]
}

###

# поиск складов, где есть определенный продукт
GET {{baseUrl}}/stocks/?products=2
Content-Type: application/json
