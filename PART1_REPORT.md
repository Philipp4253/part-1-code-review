Часть 1. Проверка кода — Отчёт

Окружение:

Версия n8n: (localhost:5678)
ОС: Windows 11

Проблемы при импорте: Отсутствуют, все workflow импортировались корректно.



01-mini-register.json (найденные проблемы):

Проблема 1: SQL Injection

Тяжесть: Высокая

Где: нода `Insert User`, поле `Query`

Что не так: Прямая вставка данных через `{{ $json.body.email }}`

Как исправить: Заменить на параметризованный запрос `VALUES ($1, $2)` + `Query Parameters`
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 224939" src="https://github.com/user-attachments/assets/09a38d5c-33bf-4e80-8681-1b0ffe133493" />



Проблема 2: SQL Injection

Тяжесть: Высокая

Где: нода `Save Code`, поле `Query`

Что не так: Вставка `user\_id` и `code` через `{{ ... }}`

Как исправить: Использовать `$1, $2` + параметры в Options
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 220719" src="https://github.com/user-attachments/assets/7c1ad9cd-b9e3-4467-a5a6-5f5f5b4d2b64" />




Проблема 3: Hardcoded Secret

Тяжесть: Высокая

Где: нода `Send Email`, поле Headers → Authorization

Что не так: API-ключ SendGrid вшит в текст ноды

Как исправить: Использовать `Generic Credential Type` → `Header Auth` → выбрать `test\_sendgrid`
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 220825" src="https://github.com/user-attachments/assets/df4b86b1-5ad6-406d-a538-609cb914d922" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 220848" src="https://github.com/user-attachments/assets/617b4b79-1125-4c70-8279-eb81ca3732f3" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 223721" src="https://github.com/user-attachments/assets/e1ccfd59-6b94-4df3-99af-3b2f271dbca5" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 223739" src="https://github.com/user-attachments/assets/2e4a0bf3-6a0e-458f-8528-82982644bdb7" />





02-job-poller.json (найденные проблемы):

Проблема 1: Агрессивный Cron

Тяжесть: Средняя

Где: нода `Schedule Trigger`, Expression

Что не так: (каждую минуту) создаёт излишнюю нагрузку

Как исправить:\*\* `0 \*/5 \* \* \* \*` (раз в 5 минут)
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 224412" src="https://github.com/user-attachments/assets/e89b2c04-a239-462d-8633-451887437405" />





Проблема 2: SQL Injection

Тяжесть: Высокая

Где: нода `Update Job`, поле `Query`

Что не так: `SET status = '{{ $json.new\_status }}' WHERE id = '{{ $json.id }}'`

Как исправить: Параметризация `SET status = $1 WHERE id = $2`
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 201349" src="https://github.com/user-attachments/assets/e7762b1c-702a-45f3-b698-842ee769db1b" />




Проблема 3: Логическая ошибка (дублирование обновления)

Тяжесть: Высокая

Где: нода `Map Status`, JS-код

Что не так: `const id = $('Get Pending Jobs').first().json.id` всегда берёт ID первой задачи, даже при обработке пачки

Как исправить: Добавить ноду `Edit Fields` перед API-запросом, сохранять `id`, в коде использовать `$json.id`
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 225004" src="https://github.com/user-attachments/assets/34977a3d-14fa-4bdb-a6a7-4021610a2df6" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 230358" src="https://github.com/user-attachments/assets/af40129b-7c41-423c-a97c-16be5aff4d7e" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 230423" src="https://github.com/user-attachments/assets/adf3d7bc-d2e4-410e-a950-c4212bde2806" />



Проблема 4: Отсутствие LIMIT

Тяжесть: Средняя

Где: нода `Get Pending Jobs`

Что не так: Запрос тянет все pending-задачи без ограничения

Как исправить: Добавить `ORDER BY id ASC LIMIT 5`
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 200727" src="https://github.com/user-attachments/assets/6532fc1f-522e-43b8-a8f6-6aa0e6531f14" />






03-file-uploader.json (найденные проблемы):

Проблема 1: Path Traversal

Тяжесть: Высокая

Где: нода `Upload to S3`, поле `File Name`

Что не так: Имя файла не очищается, возможна запись за пределы директории

Как исправить: Санитизация `.replace(/\[^a-zA-Z0-9.\_-]/g, '\_')`
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 201939" src="https://github.com/user-attachments/assets/7eb9298a-94a6-4120-a08e-f01dc96a7197" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 202221" src="https://github.com/user-attachments/assets/f8760642-c096-4fd0-8c46-bf042d3f9970" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 202613" src="https://github.com/user-attachments/assets/c668e532-333b-4b39-8663-31fdb63c5978" />





Проблема 2: SQL Injection

Тяжесть: Высокая

Где: нода `Save File Record`

Что не так: Прямая конкатенация значений в `INSERT`

Как исправить: `VALUES ($1, $2, $3, $4)` + `Query Parameters`
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 203928" src="https://github.com/user-attachments/assets/6f0c1d7b-ef38-44a0-87fd-1bfcfa389ca1" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 204514" src="https://github.com/user-attachments/assets/e0830e9d-fce3-4baa-b7ec-dc60dd68614c" />





Проблема 3: Открытый Webhook

Тяжесть: Высокая

Где: нода `Webhook1`, Authentication

Что не так: `None` — доступ без ограничений

Как исправить: `Header Auth` → `test\_sendgrid`
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 203324" src="https://github.com/user-attachments/assets/01ccf7be-21dd-4ab4-9c66-2ddc2242ba09" />




Проблема 4: Case-sensitive headers

Тяжесть: Средняя

Где: нода `Check Size`

Что не так: `headers\['content-length']` не найдёт заголовок при `Content-Length`

Как исправить: Добавить `|| headers\['Content-Length']`
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 202932" src="https://github.com/user-attachments/assets/0ec11eea-5f17-4862-85d9-9eb827918932" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 203212" src="https://github.com/user-attachments/assets/a1a61d7a-e785-4cee-891f-6065ce6c5c48" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 231309" src="https://github.com/user-attachments/assets/6d277125-6d4d-4970-a6f0-33eab126e5ce" />








Топ-3 по приоритету:

1. `[ALL] SQL Injection` — позволяет выполнить произвольные запросы к БД, критично для любого продакшена.

2. `[01, 03] Утечка секретов / Открытый доступ` — API-ключи в коде и Webhook без Auth позволяют перехватить управление.

3. `[02] Логическая ошибка Map Status` — при параллельной обработке все задачи обновляют одну запись в БД.



Реализованные исправления:

`01-mini-register-fixed.json` — параметризованы SQL-запросы, секрет вынесен в Credential.

`02-job-poller-fixed.json` — исправлен Cron, добавлен LIMIT, исправлена логика ID через `Edit Fields`.

`03-file-uploader-fixed.json` — добавлена санитизация имён, параметризация SQL, включён Header Auth.



Не сделано, но стоит!

Асинхронная отправка писем (разветвление потока после `Save Code`);

Глобальный Error Trigger workflow для алертов при падении;

Кэширование статусов внешних задач (чтобы не опрашивать API дважды за 5 мин);

Валидация MIME-type загружаемых файлов;




Общее впечатление:

Воркфлоу логично разделены на этапы, есть базовая валидация и хеширование паролей. Однако уровень безопасности на старте был низким: SQL-инъекции почти в каждом запросе, секреты в тексте, отсутствие лимитов. Это типично для прототипов, но требует обязательного аудита перед выкаткой. Если бы проектировал с нуля, сразу заложил бы Credentials, Error Handling и разделение "тяжёлых" операций (отправка писем) в фоновые ветки.

