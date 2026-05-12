# Лабораторна робота №6

**З дисципліни:** «WEB-орієнтовані технології. Backend розробки»  
**Тема:** «Документування API за допомогою Swagger. Деплой Node.js-додатку. Підсумковий проєкт: REST API з MySQL»

## Мета роботи

Метою лабораторної роботи є вивчення принципів документування REST API та практичне використання Swagger/OpenAPI у Node.js + Express застосунку. У межах роботи необхідно інтегрувати Swagger UI, описати основні endpoint-и, параметри запитів, тіла запитів, схеми відповідей і перевірити роботу API через веб-інтерфейс документації.

Також метою роботи є підготовка підсумкового backend-проєкту на основі вже створеного застосунку `rentBikeApi`, який працює з базою даних MySQL, реалізує CRUD-операції та може бути підготовлений до деплою на хмарну платформу або VPS.

## Короткі теоретичні відомості

API — це інтерфейс взаємодії між різними програмними компонентами. У backend-розробці API дозволяє frontend-додатку, мобільному застосунку або сторонньому сервісу надсилати запити до сервера й отримувати дані у структурованому форматі.

REST API — це архітектурний підхід до побудови веб-сервісів, у якому дані представлені як ресурси. Для роботи з ресурсами використовуються стандартні HTTP-методи: `GET`, `POST`, `PUT`, `PATCH` і `DELETE`. Наприклад, `GET` використовується для отримання даних, `POST` — для створення, `PUT` — для повного оновлення, а `DELETE` — для видалення ресурсу.

Swagger — це набір інструментів для опису, перегляду й тестування REST API. Він базується на специфікації OpenAPI. Завдяки Swagger UI розробник може переглядати всі маршрути API, параметри запитів, схеми JSON-даних і виконувати HTTP-запити без використання Postman або окремого frontend-додатку.

OpenAPI Specification — це стандарт опису REST API. У ньому можна описати доступні endpoint-и, HTTP-методи, параметри, request body, формати відповідей, коди статусів і правила авторизації. У Node.js застосунках для інтеграції Swagger часто використовуються пакети `swagger-ui-express` і `swagger-jsdoc`.

Express.js — це фреймворк для Node.js, який використовується для створення серверних застосунків і REST API. Він дозволяє швидко налаштовувати маршрути, middleware, обробку JSON-запитів, підключення документації та взаємодію з базою даних.

MySQL — це реляційна система керування базами даних. У проєкті вона використовується для збереження користувачів, велосипедів, станцій прокату, бронювань та інших пов’язаних даних.

## Постановка задачі

У межах лабораторної роботи необхідно вдосконалити існуючий Node.js-проєкт `rentBikeApi` і підготувати його як підсумковий REST API з MySQL та Swagger-документацією.

Необхідно реалізувати:

- REST API на Node.js + Express;
- підключення до MySQL;
- CRUD-операції для основних сутностей;
- інтеграцію Swagger UI;
- документацію всіх основних endpoint-ів;
- опис request body, параметрів і відповідей;
- можливість тестування API через Swagger UI;
- підготовку проєкту до деплою;
- файл `.env.example` для змінних середовища;
- Docker Compose для запуску API разом із MySQL;
- README з інструкцією запуску.

## Опис предметної області

Проєкт `rentBikeApi` реалізує backend для системи оренди велосипедів. Користувач може реєструватися, входити в систему, переглядати доступні велосипеди та створювати бронювання. Адміністратор може керувати велосипедами, станціями прокату й бронюваннями.

Основні сутності системи:

- `User` — користувач системи;
- `Bike` — велосипед, який можна орендувати;
- `Station` — станція прокату велосипедів;
- `Booking` — бронювання велосипеда користувачем;
- `Payment` — платіж або інформація про оплату.

## Структура проєкту

Після оновлення проєкт має таку основну структуру:

```text
rentBikeApi/
├── app.js
├── server.js
├── package.json
├── Dockerfile
├── docker-compose.yml
├── render.yaml
├── .env.example
├── config/
├── middleware/
├── models/
├── routes/
├── scripts/
│   └── seed.js
├── sql/
│   └── schema.sql
├── tests/
└── utils/
```

Основні файли проєкту:

| Файл | Призначення |
|---|---|
| `app.js` | Налаштування Express-застосунку, middleware та маршрутів |
| `server.js` | Запуск сервера та підключення додаткових сервісів |
| `config/database.js` | Підключення до MySQL через Sequelize |
| `models/` | Опис моделей бази даних |
| `routes/` | REST-маршрути API |
| `routes/swagger.js` | Swagger/OpenAPI документація |
| `.env.example` | Приклад змінних середовища |
| `docker-compose.yml` | Запуск API, MySQL та Redis у контейнерах |
| `scripts/seed.js` | Заповнення бази тестовими даними |
| `render.yaml` | Приклад конфігурації для деплою |

![Структура проєкту](/assets/labs/lab-6/screen-1.png)  
Рис. 1 Структура оновленого проєкту `rentBikeApi`

## Підключення залежностей

У файлі `package.json` були використані основні бібліотеки для роботи API:

```json
{
  "dependencies": {
    "express": "^4.22.1",
    "mysql2": "^3.14.0",
    "sequelize": "^6.37.3",
    "swagger-jsdoc": "^6.2.8",
    "swagger-ui-express": "^5.0.1",
    "jsonwebtoken": "^9.0.3",
    "bcryptjs": "^3.0.3",
    "express-validator": "^7.3.2",
    "dotenv": "^16.6.1"
  }
}
```

Для запуску сервера використовується script:

```json
{
  "scripts": {
    "start": "node server.js",
    "dev": "nodemon server.js",
    "db:seed": "node scripts/seed.js",
    "test": "jest --runInBand"
  }
}
```

## Налаштування змінних середовища

Для безпечного зберігання налаштувань використовується файл `.env`. У репозиторії не зберігаються реальні паролі, замість цього додано файл `.env.example`.

Приклад змінних середовища:

```env
PORT=3000
NODE_ENV=development

DB_HOST=127.0.0.1
DB_PORT=3307
DB_NAME=bike_rental_db
DB_USER=root
DB_PASSWORD=root

JWT_ACCESS_SECRET=change_me_access_secret
JWT_REFRESH_SECRET=change_me_refresh_secret

CACHE_DRIVER=memory
REDIS_URL=redis://localhost:6379
```

Змінна `PORT` використовується для запуску застосунку локально або на сервері. Це важливо для деплою, оскільки хмарні платформи часто передають порт через `process.env.PORT`.

## Реалізація REST API

У проєкті реалізовано основні REST endpoint-и для роботи з системою оренди велосипедів.

Основні маршрути:

| Метод | Endpoint | Опис |
|---|---|---|
| `GET` | `/` | Перевірка роботи API |
| `GET` | `/health` | Health check для деплою |
| `GET` | `/status` | Інформація про стан сервера |
| `POST` | `/auth/register` | Реєстрація користувача |
| `POST` | `/auth/login` | Авторизація користувача |
| `GET` | `/auth/profile` | Отримання профілю користувача |
| `GET` | `/bikes` | Отримання списку велосипедів |
| `GET` | `/bikes/:id` | Отримання одного велосипеда |
| `POST` | `/bikes` | Створення велосипеда |
| `PUT` | `/bikes/:id` | Оновлення велосипеда |
| `DELETE` | `/bikes/:id` | Видалення велосипеда |
| `GET` | `/stations` | Отримання списку станцій |
| `POST` | `/stations` | Створення станції |
| `PUT` | `/stations/:id` | Оновлення станції |
| `DELETE` | `/stations/:id` | Видалення станції |
| `GET` | `/bookings` | Отримання списку бронювань |
| `POST` | `/bookings` | Створення бронювання |
| `DELETE` | `/bookings/:id` | Видалення бронювання |

## CRUD-операції для велосипедів

Для сутності `Bike` реалізовано повний набір CRUD-операцій.

Приклад отримання списку велосипедів:

```js
router.get('/', async (req, res, next) => {
  try {
    const bikes = await Bike.findAll({
      include: [{ model: Station }],
      order: [['id', 'DESC']],
    });

    res.json(bikes);
  } catch (error) {
    next(error);
  }
});
```

Приклад створення велосипеда:

```js
router.post('/', authMiddleware, roleMiddleware('admin'), async (req, res, next) => {
  try {
    const bike = await Bike.create(req.body);
    res.status(201).json(bike);
  } catch (error) {
    next(error);
  }
});
```

Для створення, редагування та видалення велосипеда використовується JWT-авторизація та перевірка ролі адміністратора.

## CRUD-операції для станцій

Для станцій прокату також реалізовано повний CRUD. Станція містить назву, адресу, координати та опис.

Основні endpoint-и:

```text
GET /stations
GET /stations/:id
POST /stations
PUT /stations/:id
DELETE /stations/:id
```

Ці маршрути дозволяють адміністратору додавати нові станції, редагувати інформацію про них і видаляти непотрібні записи.

## Реалізація бронювань

Для бронювань реалізовано створення, перегляд, зміну статусу та видалення.

Основні endpoint-и:

```text
GET /bookings
GET /bookings/:id
POST /bookings
PUT /bookings/:id/status
DELETE /bookings/:id
```

Під час створення бронювання користувач вказує велосипед, дату початку та завершення оренди. Сервер зберігає бронювання у базі даних MySQL і повертає JSON-відповідь.

## Інтеграція Swagger UI

Для документування API було використано пакети `swagger-ui-express` та `swagger-jsdoc`.

Приклад базового налаштування Swagger:

```js
const swaggerUi = require('swagger-ui-express');
const swaggerJsdoc = require('swagger-jsdoc');

const options = {
  definition: {
    openapi: '3.0.0',
    info: {
      title: 'Rent Bike API',
      version: '1.0.0',
      description: 'REST API documentation for bike rental project',
    },
  },
  apis: ['./routes/*.js'],
};

const swaggerSpec = swaggerJsdoc(options);

router.use('/api-docs', swaggerUi.serve, swaggerUi.setup(swaggerSpec));
```

У проєкті документація доступна за адресою:

```text
http://localhost:3000/api-docs
```

Також було додано endpoint для отримання OpenAPI JSON:

```text
http://localhost:3000/api-docs/json
```

![Swagger UI](/assets/labs/lab-6/screen-2.png)  
Рис. 2 Swagger UI з описом endpoint-ів API

## Документування endpoint-ів

У Swagger-документації описано основні маршрути API, параметри, тіла запитів та відповіді сервера. Для захищених endpoint-ів додано Bearer Token авторизацію.

Приклад опису endpoint-а для отримання велосипедів:

```js
/**
 * @swagger
 * /bikes:
 *   get:
 *     summary: Отримати список велосипедів
 *     tags: [Bikes]
 *     responses:
 *       200:
 *         description: Список велосипедів
 */
```

Приклад опису POST-запиту:

```js
/**
 * @swagger
 * /bikes:
 *   post:
 *     summary: Створити велосипед
 *     tags: [Bikes]
 *     security:
 *       - bearerAuth: []
 *     requestBody:
 *       required: true
 *       content:
 *         application/json:
 *           schema:
 *             type: object
 *             properties:
 *               title:
 *                 type: string
 *               type:
 *                 type: string
 *               price_per_hour:
 *                 type: number
 *               status:
 *                 type: string
 *               station_id:
 *                 type: integer
 *     responses:
 *       201:
 *         description: Велосипед створено
 */
```

Завдяки такому опису Swagger UI показує поля, які потрібно передати в запиті, і дозволяє виконати запит безпосередньо з браузера.

## Авторизація через JWT

У проєкті використовується JWT-авторизація. Після успішного входу користувач отримує `accessToken`, який потрібно передавати в заголовку `Authorization`.

Приклад заголовка:

```text
Authorization: Bearer YOUR_ACCESS_TOKEN
```

У Swagger UI для цього використовується кнопка **Authorize**. Після введення токена можна виконувати захищені запити, наприклад створення велосипеда або станції.

![Авторизація Swagger](/assets/labs/lab-6/screen-3.png)  
Рис. 3 Введення Bearer Token у Swagger UI

## Підключення MySQL

Підключення до бази даних виконується через Sequelize ORM. Параметри підключення зберігаються у змінних середовища.

Приклад конфігурації:

```js
const { Sequelize } = require('sequelize');

const sequelize = new Sequelize(
  process.env.DB_NAME,
  process.env.DB_USER,
  process.env.DB_PASSWORD,
  {
    host: process.env.DB_HOST,
    port: process.env.DB_PORT,
    dialect: 'mysql',
  }
);

module.exports = sequelize;
```

У проєкті також є SQL-файл `sql/schema.sql`, який описує структуру бази даних. Для швидкого заповнення тестовими даними створено script:

```bash
npm run db:seed
```

## Docker Compose

Для зручного запуску застосунку було підготовлено `docker-compose.yml`. Він запускає API, MySQL і Redis.

Приклад запуску:

```bash
docker compose up --build
```

Після запуску застосунок доступний за адресою:

```text
http://localhost:3000
```

Swagger-документація доступна за адресою:

```text
http://localhost:3000/api-docs
```

![Docker Compose](/assets/labs/lab-6/screen-4.png)  
Рис. 4 Запуск API та MySQL через Docker Compose

## Підготовка до деплою

Для деплою Node.js-застосунку потрібно:

1. Завантажити проєкт на GitHub.
2. Створити сервіс на Render або іншій платформі.
3. Вказати команду встановлення залежностей.
4. Вказати команду запуску.
5. Додати змінні середовища.
6. Перевірити `/health` після запуску.

Команда встановлення:

```bash
npm ci
```

Команда запуску:

```bash
npm start
```

Health check endpoint:

```text
/health
```

У проєкт додано файл `render.yaml`, який містить приклад конфігурації для Render.

## Тестування API через Swagger UI

Після запуску проєкту Swagger UI дозволяє перевірити всі основні дії:

1. Відкрити `http://localhost:3000/api-docs`.
2. Виконати `POST /auth/login`.
3. Скопіювати `accessToken`.
4. Натиснути **Authorize**.
5. Передати токен у форматі `Bearer <accessToken>`.
6. Виконати CRUD-запити для велосипедів, станцій або бронювань.

Під час тестування було перевірено:

- отримання списку велосипедів;
- створення нового велосипеда;
- отримання велосипеда за `id`;
- оновлення велосипеда;
- видалення велосипеда;
- створення та перегляд станцій;
- створення й видалення бронювання;
- роботу JWT-авторизації.

![Тестування endpoint-а](/assets/labs/lab-6/screen-5.png)  
Рис. 5 Виконання запиту через Swagger UI

## Приклади cURL-запитів

Перевірка роботи API:

```bash
curl http://localhost:3000/
```

Health check:

```bash
curl http://localhost:3000/health
```

Авторизація адміністратора:

```bash
curl -X POST http://localhost:3000/auth/login ^
  -H "Content-Type: application/json" ^
  -d "{\"email\":\"admin@example.com\",\"password\":\"admin123\"}"
```

Отримання списку велосипедів:

```bash
curl http://localhost:3000/bikes
```

Створення велосипеда:

```bash
curl -X POST http://localhost:3000/bikes ^
  -H "Content-Type: application/json" ^
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" ^
  -d "{\"title\":\"Road Bike\",\"type\":\"road\",\"price_per_hour\":100,\"status\":\"available\",\"station_id\":1}"
```

Створення станції:

```bash
curl -X POST http://localhost:3000/stations ^
  -H "Content-Type: application/json" ^
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN" ^
  -d "{\"name\":\"Campus Station\",\"address\":\"KPI, Kyiv\",\"latitude\":50.449,\"longitude\":30.457}"
```

## Результати роботи

У результаті виконання лабораторної роботи було оновлено backend-проєкт `rentBikeApi` і підготовлено його як підсумковий REST API з MySQL та Swagger-документацією.

Було реалізовано:

- REST API на Node.js + Express;
- підключення MySQL через Sequelize;
- моделі користувачів, велосипедів, станцій, бронювань і платежів;
- CRUD-операції для велосипедів;
- CRUD-операції для станцій;
- створення, перегляд, оновлення статусу й видалення бронювань;
- JWT-авторизацію;
- перевірку ролей для адміністративних маршрутів;
- Swagger UI за маршрутом `/api-docs`;
- OpenAPI JSON за маршрутом `/api-docs/json`;
- файл `.env.example`;
- Docker Compose для запуску API та MySQL;
- `render.yaml` для прикладу деплою;
- README з інструкціями запуску та тестування.

Кількість ілюстрацій у звіті зменшено: залишено тільки основні скріншоти, які демонструють структуру проєкту, Swagger UI, авторизацію, запуск Docker Compose та тестування endpoint-а.

## Контрольні запитання

**1. Що таке REST API?**  
REST API — це інтерфейс взаємодії між клієнтом і сервером, побудований за принципами REST. Він використовує HTTP-методи для роботи з ресурсами.

**2. Для чого використовується Swagger?**  
Swagger використовується для документування, перегляду й тестування REST API через зручний веб-інтерфейс.

**3. Що таке OpenAPI?**  
OpenAPI — це стандарт опису REST API, який визначає маршрути, методи, параметри, тіла запитів, відповіді та авторизацію.

**4. Які HTTP-методи використовуються у REST?**  
Найчастіше використовуються `GET`, `POST`, `PUT`, `PATCH` і `DELETE`.

**5. Для чого потрібен swagger-jsdoc?**  
`swagger-jsdoc` потрібен для генерації OpenAPI-документації на основі JSDoc-коментарів у коді.

**6. Що таке endpoint?**  
Endpoint — це конкретний URL API, до якого клієнт надсилає HTTP-запит для виконання певної дії.

**7. Які переваги Swagger UI?**  
Swagger UI дозволяє переглядати всі маршрути API, бачити схеми запитів і відповідей, а також тестувати endpoint-и прямо з браузера.

**8. Для чого використовується Express.js?**  
Express.js використовується для створення серверних застосунків і REST API на Node.js.

**9. Що таке деплой?**  
Деплой — це процес публікації застосунку на сервері або хмарній платформі, щоб він став доступним для користувачів через Інтернет.

**10. Для чого потрібен GitHub під час деплою?**  
GitHub використовується для зберігання коду, контролю версій і підключення репозиторію до платформ автоматичного деплою, наприклад Render або Railway.

## Висновок

У ході виконання лабораторної роботи було досягнуто поставленої мети. Було розглянуто принципи документування REST API та реалізовано Swagger/OpenAPI документацію для Node.js + Express застосунку.

На основі існуючого проєкту `rentBikeApi` було підготовлено підсумковий REST API з MySQL. У застосунку реалізовано CRUD-операції для велосипедів, станцій і бронювань, JWT-авторизацію, перевірку ролей, Swagger UI, Docker Compose та конфігурацію для деплою.

Swagger UI значно спрощує перевірку API, оскільки дозволяє бачити всі доступні endpoint-и, структуру запитів і відповідей, а також виконувати тестові запити без додаткових інструментів. У результаті проєкт став більш зрозумілим, структурованим і готовим до демонстрації як підсумковий REST API з MySQL та Swagger-документацією.
