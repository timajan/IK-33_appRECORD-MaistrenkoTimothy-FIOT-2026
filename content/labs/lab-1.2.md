# Лабораторна робота №1
## Частина 2. Основи роботи з Express.js

### Мета роботи
Ознайомитися з Node.js, Express.js та навчитися створювати простий REST API.

## Короткі теоретичні відомості
**HTTP-сервер** — це програма, яка приймає HTTP-запити і повертає відповіді.  
**Node.js** — середовище виконання JavaScript на стороні сервера.  
**Express.js** — фреймворк для Node.js, який спрощує створення серверів і маршрутів.  
**REST API** — підхід до побудови вебсервісів через стандартні HTTP-методи: `GET`, `POST`, `PUT`, `DELETE`.

## Хід виконання роботи
1. Створено Node.js проєкт.
2. Встановлено бібліотеку `express`.
3. Створено файл `server.js`.
4. Реалізовано маршрути:
   - `GET /`
   - `GET /students`
   - `POST /students`
   - `PUT /students/:id`
   - `DELETE /students/:id`

## Мінімальний код програми
```javascript
const express = require("express");
const app = express();

app.use(express.json());

let students = [
  { id: 1, name: "Іван", group: "ІП-21" }
];

app.get("/", (req, res) => {
  res.send("Hello from Node.js server");
});

app.get("/students", (req, res) => {
  res.json(students);
});

app.post("/students", (req, res) => {
  students.push(req.body);
  res.status(201).json(req.body);
});

app.put("/students/:id", (req, res) => {
  const student = students.find(s => s.id == req.params.id);
  if (!student) return res.status(404).json({ message: "Not found" });

  student.name = req.body.name ?? student.name;
  student.group = req.body.group ?? student.group;

  res.json(student);
});

app.delete("/students/:id", (req, res) => {
  students = students.filter(s => s.id != req.params.id);
  res.json({ message: "Student deleted" });
});

app.listen(3000, () => {
  console.log("Server started on port 3000");
});
```

## Запуск програми
```bash
npm init -y
npm install express
node server.js
```

## Результат
Після запуску сервер працює на адресі `http://localhost:3000`.  
Маршрут `/` повертає текстове повідомлення, а маршрут `/students` дозволяє працювати зі списком студентів у форматі JSON.

## Висновок
У ході лабораторної роботи було створено найпростіший серверний застосунок на Node.js та Express.js. Було освоєно створення маршрутів і базові операції з даними через REST API.
