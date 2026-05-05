Часть 2. Рабочий комбинат — Отчёт



1. Разбор пайплайна

Группа 1: Триггеры

Ноды: `Schedule Trigger`, `Webhook`, `Webhook1`

Что делает: Запуск по расписанию или внешнему HTTP-запросу.

Интересное: Критично правильно настраивать Cron, чтобы не создавать race conditions.
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 200333" src="https://github.com/user-attachments/assets/289c0dec-8a6c-4334-9684-0e99bae50d72" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 174223" src="https://github.com/user-attachments/assets/0d903a46-a731-43a7-a724-1cb3b38adb3d" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 191158" src="https://github.com/user-attachments/assets/8aeeb866-3efc-4a35-b451-11973c7cdf87" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 203324" src="https://github.com/user-attachments/assets/2cae43a7-1271-4f09-8df2-f08dbbe36c75" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 224412" src="https://github.com/user-attachments/assets/d6b9e795-8e14-4ad4-a437-ed99a5672ba4" />




Группа 2: Валидация

Ноды: `Validate Input`, `Check Size`, `Check Email Exists`

Что делает: Фильтрация входящих данных перед записью в БД.

Интересное: Проверка размера идёт через `content-length`, важно учитывать регистр заголовков.
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 231309" src="https://github.com/user-attachments/assets/b3248823-1ed9-4d86-bce7-0e9b53084265" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 203212" src="https://github.com/user-attachments/assets/9b48e230-4cd3-40b0-b06d-3c5505f9889b" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 202932" src="https://github.com/user-attachments/assets/9fe05c64-a995-40c1-b874-9175af2e309d" />





Группа 3: База данных (Postgres)

Ноды: `Get Pending Jobs`, `Insert User`, `Save Code`, `Update Job`, `Save File Record`

Что делает: CRUD-операции.

Интересное: Ручные `Execute Query` дают гибкость, но требуют строгой параметризации.
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 200727" src="https://github.com/user-attachments/assets/bb5ce0b6-bb1e-43e4-bf22-fda01fe15d72" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 220058" src="https://github.com/user-attachments/assets/4239e315-47b7-47b2-a480-4bac5918442b" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 220444" src="https://github.com/user-attachments/assets/2af7a0e5-5f1b-45cf-a962-162ced0420a5" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 220719" src="https://github.com/user-attachments/assets/b5b03e57-c383-43ce-a0dd-4e2323facd1b" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 203928" src="https://github.com/user-attachments/assets/59356983-0b94-48bb-89b3-6e9696c9e3f1" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 204514" src="https://github.com/user-attachments/assets/5373be43-3a73-4850-a22b-f4f3429f086e" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 201349" src="https://github.com/user-attachments/assets/482c37be-0d82-47f5-b97b-73ae9ef93d90" />





Группа 4: Внешние API

Ноды: `Check External Status`, `Send Email`

Что делает: Опрос сторонних сервисов и отправка уведомлений.

Интересное: Без настроенного Retry падение внешнего API останавливает весь пайплайн.
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 220825" src="https://github.com/user-attachments/assets/4117f315-36cf-46a6-aea6-9ca3c9a2ecc2" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 220848" src="https://github.com/user-attachments/assets/5443a3f4-1c0a-4aac-bedb-de16ed9decb7" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 223721" src="https://github.com/user-attachments/assets/30452353-9278-4c32-b5cb-8946c03751fd" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 223739" src="https://github.com/user-attachments/assets/1283352d-30d3-42ea-a8c6-7559a5c09f22" />





Группа 5: Трансформация данных

Ноды: `Hash Password`, `Map Status`, `Edit Fields`

Что делает: Подготовка payload для следующих шагов.

Интересное: `Edit Fields` спасает от потери данных при прохождении через HTTP-ноды.
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 225004" src="https://github.com/user-attachments/assets/c59774f5-a8d1-4ce5-86fb-cce7ab487692" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 230226" src="https://github.com/user-attachments/assets/437f4af0-8e06-4106-91de-b262afd5a643" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 230358" src="https://github.com/user-attachments/assets/633bb343-a50f-47bd-b97e-c83623fd7e6f" />





Группа 6: Хранилище

Ноды: `Upload to S3`

Что делает: Загрузка бинарных данных в MinIO.

Интересное: Требует строгой санитизации имён файлов (Path Traversal).
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 201939" src="https://github.com/user-attachments/assets/95653a1d-329e-49f8-b300-1ee432b0da55" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 202221" src="https://github.com/user-attachments/assets/1740e7e6-86f5-4167-be8d-ae1e96dd9b3c" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 202613" src="https://github.com/user-attachments/assets/b983995b-06b8-4ad5-997f-c116f8fa0a58" />





Группа 7: Ответ клиенту

Ноды: `Respond OK`, `Respond OK1`

Что делает: Возврат HTTP-статуса и JSON.

Интересное: Сейчас ответ синхронный, пользователь ждёт завершения всего цикла.





2. Три слабых места

Слабое место 1: Синхронная отправка писем

Где: `01-mini-register` (ветка `Send Email` → `Respond OK`)

В чём проблема: Клиент ждёт ответа от SMTP/API пока письмо не уйдёт.

Когда выстрелит: При скачке регистраций время ответа вырастет до 5-10 сек, возможны таймауты.

Оценка: Средняя
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 232854" src="https://github.com/user-attachments/assets/e8554916-6e3b-4bab-8125-b31e2375d272" />



Слабое место 2: Отсутствие дедупликации поллера

Где: `02-job-poller` (`Schedule Trigger`)

В чём проблема: Если обработка пачки займёт >5 мин, запустится второй экземпляр.

Когда выстрелит: Накопление задач → наложение запусков → блокировки в БД.

Оценка: Средняя
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 224412" src="https://github.com/user-attachments/assets/f87fa2f0-01df-4a2b-a4e1-28a6e83bf3aa" />




Слабое место 3: Нет проверки MIME-type

Где: `03-file-uploader` (`Check Size`)

В чём проблема: Проверяется только расширение, но не реальное содержимое.

Когда выстрелит: Загрузка маскированного исполняемого файла (`virus.jpg.exe`).

Оценка: Высокая
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 203212" src="https://github.com/user-attachments/assets/ac6a7e3c-0900-4e5b-a41c-6c71bdc525aa" />



3. Два рефакторинга:

Рефакторинг 1: Асинхронное ветвление регистрации

Что изменить: После `Save Code` разорвать связь с `Send Email`. Подключить `Respond OK` напрямую к `Save Code`. `Send Email` оставить в параллельной ветке без выхода на ответ.

Зачем: Снизить latency ответа пользователю с \~5с до \~200мс.

Риски: Ошибка отправки письма останется незамеченной клиентом (нужен лог/Error Trigger).

Время: \~30 мин
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 205010" src="https://github.com/user-attachments/assets/b1c5741f-7ba1-4941-93b5-c9dca69166f0" />



Рефакторинг 2: Вынос валидации файлов в микросервис

Что изменить: Создать отдельный workflow `validate-file`, проверять magic numbers / MIME, вызывать через `Execute Workflow`.

Зачем: Переиспользование, централизованная безопасность, разгрузка основного пайплайна.

Риски: Доп. задержка на вызов, сложность поддержки.

Время: \~2-3 часа



4. Реализованное изменение:

Выбрал вариант: Добавление ноды `Edit Fields` для сохранения контекста ID.

Что сделал: Вставил ноду между `Get Pending Jobs` и `Check External Status`. Настроил маппинг `id = {{ $json.id }}`. Обновил код `Map Status` на `$json.id`.

Где именно: Workflow `02-job-poller`, центральная секция обработки джобов.

Как тестировал: Имитировал пачку из 3 задач. Убедился, что в Execution Log обновляются 3 разные записи, а не одна и та же.
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 230226" src="https://github.com/user-attachments/assets/d04c2ad9-74d6-4894-8ed8-2a50d725e7af" />
<img width="1920" height="1200" alt="Снимок экрана 2026-05-04 230423" src="https://github.com/user-attachments/assets/9f3b534d-ac7f-47fb-85da-8d1cb8247eca" />



Общее впечатление:

Пайплайны покрывают типовые сценарии автоматизации, но собраны в стиле "быстрого прототипа". Основная боль — отсутствие отказоустойчивости (Retry, Error Handling) и синхронные блокирующие операции. Для MVP это допустимо, но для продакшена я бы вынес тяжёлые операции (отправка писем, проверка файлов) в фоновые воркфлоу, добавил бы глобальный Error Trigger и Redis для кэширования состояний. n8n отлично подходит для оркестрации, но бизнес-логику с высоким риском лучше изолировать.

