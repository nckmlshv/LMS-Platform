# Создание пользователя

## Метод
POST /users

## Назначение
Метод предназначен для создания нового пользователя в системе LMS.

---

## Авторизация
Требуется Bearer token.

---

## Заголовки запроса

| Заголовок         | Тип данных | Обязательный | Описание                         |
|------------------|------------|--------------|----------------------------------|
| Authorization    | string     | Да           | Bearer JWT token                 |
| X-Idempotency-Key| string     | Да           | Ключ идемпотентности             |
| X-Trace-Id       | string     | Да           | Идентификатор трассировки        |

---

## Входные данные

### Тело запроса

| Поле                | Тип данных | Обязательное | Ограничения / Описание |
|---------------------|------------|--------------|------------------------|
| first_name          | string     | Да           | 1–24 символа |
| middle_name         | string     | Нет          | 1–24 символа |
| last_name           | string     | Да           | 1–24 символа |
| gender              | string     | Нет          | male / female |
| date_of_birth       | date       | Да           | YYYY-MM-DD |
| email               | string     | Да           | 5–64 символа, формат email |
| password_hash       | string     | Да           | 8–64 символа |
| phone_number        | string     | Нет          | 12 символов, формат +12345678910 |
| role                | string     | Да           | author / student / advisor |
| avatar_url          | string     | Нет          | URL |
| email_notifications | string     | Нет          | all / important / no |
| created_at          | date       | Нет          | YYYY-MM-DD |

### Пример запроса

    {
      "first_name": "Donald",
      "middle_name": "John",
      "last_name": "Trump",
      "gender": "male",
      "date_of_birth": "2000-01-01",
      "email": "student@lms.com",
      "password_hash": "qwerty123",
      "phone_number": "+12345678910",
      "role": "student",
      "avatar_url": "https://lms.com/avatars/1.png",
      "email_notifications": "all",
      "created_at": "2026-01-01"
    }

---

## Выходные данные

### Успешный ответ 201

| Поле          | Тип данных | Обязательное | Описание |
|---------------|------------|--------------|----------|
| id            | integer    | Да           | ID пользователя |
| first_name    | string     | Нет          | Имя |
| last_name     | string     | Нет          | Фамилия |
| middle_name   | string     | Нет          | Отчество |
| email         | string     | Да           | Email |
| phone_number  | string     | Нет          | Телефон |
| role          | string     | Да           | Роль |
| avatar_url    | string     | Нет          | Аватар |
| creation_date | datetime   | Нет          | Дата и время создания |
| created_at    | date       | Нет          | Дата создания |

### Пример ответа

    {
      "id": 123,
      "first_name": "Donald",
      "last_name": "Trump",
      "middle_name": "John",
      "email": "student@lms.com",
      "phone_number": "+71234567890",
      "role": "student",
      "avatar_url": "https://lms.com/avatars/1.png",
      "creation_date": "2025-08-30T14:30:00",
      "created_at": "2026-01-01"
    }

---

## Алгоритм работы

1. Проверить наличие заголовков:
   - Authorization
   - X-Idempotency-Key
   - X-Trace-Id

2. Провести валидацию тела запроса:
   - обязательные поля присутствуют
   - значения соответствуют ограничениям
   - email валиден
   - enum-поля содержат допустимые значения

3. Проверить идемпотентность по X-Idempotency-Key:
   - при повторе вернуть тот же результат

4. Создать пользователя в системе

5. Вернуть ответ 201 Created

---

## Ошибки

### Формат ошибки

| Поле         | Тип данных | Описание |
|--------------|------------|----------|
| status_code  | integer    | HTTP код |
| description  | string     | Описание ошибки |

### Пример ошибки

    {
      "status_code": 400,
      "description": "Invalid request data"
    }

---

## Возможные ошибки

| Код | Описание |
|-----|----------|
| 400 | Некорректные данные |
| 401 | Не авторизован |
| 403 | Доступ запрещён |
| 409 | Конфликт |
| 500 | Ошибка сервера |

---

## Примечания

- Пароль передаётся в виде password_hash
- Поле created_at передаётся клиентом
- Поля creation_date и created_at частично дублируют друг друга
