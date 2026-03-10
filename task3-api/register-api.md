# REST API - Регистрация пользователя
## Общая информация
1. Http метод + URL: POST /api/users
2. Content-Type: application/json
3. Описание: Регистрация нового пользователя в системе Book Store
## Входные параметры 

| Наименование | Тип | Обязательный | Ограничения |
|------|------|------|------|
| firstName | string | да | minLength: 1, maxLength: 50, только буквы |
| lastName | string | да | minLength: 1, maxLength: 50, только буквы |
| username | string | да | minLength: 1, maxLength: 50, только латиница и цифры |
| password | string | да | minLength: 8, минимум: 1 цифра, 1 uppercase, 1 lowercase, 1 спецсимвол |
| recaptchaToken | string | да | Непустая строка — токен от Google reCAPTCHA |

## Выходные параметры при успешном ответе

| Наименование | Тип | Обязательный | Ограничения |
|------|------|------|------|
| id | string | да | Уникальный, генерируется сервером, формат UUID v4 |
| firstName | string | да | minLength: 1, maxLength: 50, только буквы |
| lastName | string | да | minLength: 1, maxLength: 50, только буквы |
| username | string | да | minLength: 1, maxLength: 50, только латиница и цифры |

## Выходные параметры при ответе с ошибкой

| Наименование | Тип | Обязательный | Ограничения |
|------|------|------|------|
| code | integer | да | Допустимые значения: 409, 422, 500 |
| message | string | да | maxLength: 255 |

## Описания ошибок + коды ответов

| Код | Тип | Сценарий | Сообщение |
|------|------|------|------|
| 201 | Успех | Пользователь создан | - |
| 409 | Клиентская ошибка | Пользователь уже существует | User already exists |
| 422 | Клиентская ошибка | Невалидный пароль | Password must have at least 8 characters, one uppercase, one lowercase, one digit and one special character |
| 422 | Клиентская ошибка | reCAPTGHA не пройдена | Please verify reCaptcha to register |
| 500 | Серверная ошибка | Внутренняя ошибка | Internal server error. Please try again later |

## Пример запроса
```
POST /api/users
Content-Type: application/json
{
  "firstName": "Ivan",
  "lastName": "Ivanov",
  "username": "ivan",
  "password": "Parol!123~",
  "recaptchaToken": "03AGdBq25xYz..."
}
```
## Пример успешного ответа
```
{
  "id": "111e0000-e11b-abc1-a000-110011000000",
  "firstName": "Ivan",
  "lastName": "Ivanov",
  "username": "ivan"
}
```
## Пример ответа с ошибкой 
```
{
  "code": 409,
  "message": "User already exists"
}
```
```
{
  "code": 422,
  "message": "Password must have at least 8 characters, one uppercase, one lowercase, one digit and one special character"
}
```
```
{
  "code": 422,
  "message": "Please verify reCaptcha to register"
}
```
```
{
  "code": 500,
  "message": "Internal server error. Please try again later"
}
```
