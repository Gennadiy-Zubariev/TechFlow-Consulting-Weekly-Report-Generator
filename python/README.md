# TechFlow Consulting — Weekly Report Generator



## Що робить скрипт

При запуску скрипт виконує такі кроки:

1. **Підключається до Airtable** — через API отримує всі записи з таблиці `Requests`
2. **Розраховує метрики** — аналізує дані за останні 7 днів
3. **Виводить результати** — показує метрики в консолі
4. **Зберігає файли** — створює `reports/techflow_report_YYYY-MM-DD.json`,`.csv`, та файл HTML з графіками
5. **Надсилає email** — HTML-лист менеджеру з прикріпленими файлами

## Метрики

- Кількість нових запитів за тиждень
- Кількість закритих запитів за тиждень
- Середній час обробки запиту (від створення до закриття)
- Середній час реакції (від створення до взяття в роботу)
- Кількість запитів у роботі
- Кількість прострочених запитів (>24 годин без обробки)
- Загальна кількість запитів
- Топ-3 консультанти за кількістю закритих запитів
- Розподіл запитів по типах послуг (послуга, кількість, частка)
- Навантаження на консультантів (відкриті запити)
- Нові запити за тиждень
- Закриті запити за тиждень


## Встановлення та запуск

```bash
# 1. Клонуйте репозиторій
https://github.com/Gennadiy-Zubariev/TechFlow-Consulting-Weekly-Report-Generator/tree/main/n8n
cd techflow-report

# 2. Створіть віртуальне середовище
python -m venv venv
source venv/bin/activate        # Linux/Mac
# venv\Scripts\activate         # Windows

# 3. Встановіть залежності
pip install -r requirements.txt

# 4. Створіть .env файл
cp .env.example .env
# Відредагуйте .env — вставте свій Airtable API key та SMTP дані

# 5. Запустіть
python techflow_report.py
```

## Що буде при успішному запуску

```
============================================================
  TechFlow Consulting — Weekly Report Generator
============================================================

📥 Отримання даних з Airtable...
✅ Отримано N записів з Airtable
📊 Розрахунок метрик...

  📅 Період: XXXX--XX — XXXX-XX-XX
  📬 Нових запитів за тиждень:    N
  ✅ Закритих запитів за тиждень:  N
  ⏱️  Середній час обробки:         xx.xx годин
  ⚡ Середній час реакції:          xx.xx годин
  🔄 У роботі зараз:               N
  ⚠️  Прострочених (>24г):          N
  📋 Загальна кількість:            N

  🏆 Топ-3 консультанти:
     1. Name — N закритих

📄 JSON звіт збережено: reports/techflow_report_XXXX-XX-XX.json
📊 CSV звіт збережено: reports/techflow_report_XXXX-XX-XX.csv
📊 Dashboard збережено: reports/techflow_report_XXXX-XX-XX.html

📧 Надсилання звіту на email...
📧 Звіт надіслано на Your@emeil.com

✅ Готово!
```

## Можливі помилки

| Помилка | Причина | Рішення |
|---------|---------|---------|
| `Відсутні змінні середовища` | Немає .env файлу | Створіть .env за прикладом .env.example |
| `Помилка підключення до Airtable` | Невірний API key або Base ID | Перевірте токен на airtable.com/create/tokens |
| `Помилка відправки email` | Невірні SMTP дані | Для Gmail використовуйте App Password |
| `Email не налаштований` | SMTP змінні не заповнені | Звіт збережеться у файли без email |

## Налаштування

### Airtable API Key
1. Перейдіть на https://airtable.com/create/tokens
2. Натисніть "Create new token"
3. Додайте scope: `data.records:read`
4. Додайте доступ до вашої бази TechFlow Base
5. Скопіюйте токен (починається з `pat_...`)

### Gmail App Password
1. Увімкніть 2FA: Google Account → Security → 2-Step Verification
2. Перейдіть: Google Account → Security → App passwords
3. Створіть новий App Password для "Mail"
4. Використовуйте 16-символьний пароль у SMTP_PASSWORD


## Структура проєкту

```
techflow-report/
├── techflow_report.py       # Основний скрипт
├── requirements.txt         # Залежності (pyairtable, python-dotenv)
├── .env                     # Конфігурація
├── .env.example             # Приклад конфігурації
├── README.md                # Документація
├── reports
    └── XXXX-XX-XX
        ├── techflow_report_XXXX-XX-XX.csv
        └── techflow_report_XXXX-XX-XX.json
        └── techflow_report_XXXX-XX-XX.html
```

## Залежності

- **pyairtable** — офіційна Python-бібліотека для Airtable API
- **python-dotenv** — завантаження змінних середовища з .env файлу
