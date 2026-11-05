# Vulnerability Tracker API

REST API сервис на .NET 10 для управления уязвимостями ПО и эмуляции асинхронных задач сканирования репозиториев.

## 📌 API Endpoints Quick Reference

- **`POST /api/vulnerabilities`** — создание уязвимости
- **`GET /api/vulnerabilities/{id}`** — получение уязвимости по ID
- **`POST /api/scantasks`** — запуск сканирования репозитория
- **`GET /api/scantasks/{id}`** — получение статуса задачи сканирования

> 📖 **[Подробное описание](#-api-endpoints)**

## 🚀 Quick start

Запуск проекта локально через .NET CLI:

1. Склонируйте репозиторий и перейдите в рабочую директорию:

```bash
git clone https://github.com/arist0crab/KasperskyIntershipTask
cd VulnerabilityTrackerApi
```

2. Соберите и запустите приложение:

```bash
dotnet run
```

3. Для запуска тестов выполните:

```bash
dotnet test
```

4. Пример взаимодейтвия:

```bash

```

## 📌 API Endpoints

### 📍 1. Управление уязвимостями (`/api/vulnerabilities`)

- **`POST /api/vulnerabilities`** - добавление новой записи об уязвимости.

  - 📥 **Request Body (`application/json`):**
    ```json
    {
      "name": "SQL Injection",
      "severity": "High"
    }
    ```

  - ✅ **Успешный ответ (`201 Created`):**

    Возвращает сгенерированный ID уязвимости.

    ```json
    {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
    }
    ```

  - ❌ **Ошибка (`500 Internal Server Error`):** 
    
    Возникает, если не удалось сохранить уязвимость.

    ```json
    {
      "message": "An error occurred while saving vulnerability"
    }
    ```

- **`GET /api/vulnerabilities/{id}`** - получение полной информации об уязвимости по её ID.

  - ✅ **Успешный ответ (`200 OK`):**

    Возвращает информацию о уязвимости, такую как имя, уровень критичности и время создания.

    ```json
    {
      "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
      "name": "SQL Injection",
      "severity": "High",
      "created_at": "2026-03-30T12:00:00Z"
    }
    ```

  - ❌ **Ошибка (`404 Not Found`):** 

    Уязвимость с соответствующим ID не была найдена.

    ```json
    {
      "message": "No vulnerability with such id"
    }
    ```

### 📍 2. Фоновое сканирование (`/api/scantasks`)

- **`POST /api/scantasks`** - запуск фоновой задачи по сканированию кода.

  - 📥 **Request Body (`application/json`):**

    ```json
    {
      "repository_url": "https://github.com/example/repo"
    }
    ```

  - ✅ **Успешный ответ (`201 Created`):**

    Возвращает ID созданной задачи сканирования.

    ```json
    {
      "id": "7bc91e42-1234-5678-abcd-1234567890ab"
    }
    ```

  - ❌ **Ошибка (`500 Internal Server Error`):** 
  
    Возникает, если не удалось сохранить задачу.

    ```json
    {
      "message": "An error occurred while saving scan task"
    }
    ```

- **`GET /api/scantasks/{id}`** - получение статуса выполнения задачи сканирования и результат сканирования в случае успешного завершения заадчи.

  - ✅ **Успешный ответ (`200 OK`):**

    ```json
    {
      "scan_task_id": "7bc91e42-1234-5678-abcd-1234567890ab",
      "scan_task_status": "Completed",
      "scan_task_updated_at": "2026-03-30T12:05:00Z",
      "found_vulnerabilities": [
        {
          "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
          "name": "SQL Injection",
          "severity": "High",
          "created_at": "2026-03-30T12:00:00Z"
        }
      ]
    }
    ```

  - ❌ **Ошибка (`404 Not Found`):** 
  
    Задача сканирования с соответствующим ID не была найдена.

    ```json
    {
      "message": "No scan task with such id"
    }
    ```