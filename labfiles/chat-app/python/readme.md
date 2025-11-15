# How to Run Azure AI Foundry SDK Chat App on Linux (WSL)

## ✅ English Version

### 1. Prerequisites
- Python 3.8+
- Azure CLI installed (`az --version`)
- Azure subscription with an Azure AI Foundry **Project** and a deployed model (e.g., `gpt-4o`)

### 2. Clone or Create Project Folder
```bash
cd /path/to/project
```

### 3. Create and Activate Virtual Environment
```bash
python3 -m venv labenv
source labenv/bin/activate
```

### 4. Install Dependencies
```bash
pip install azure-identity azure-ai-projects openai python-dotenv
```

### 5. Configure Environment Variables
Create a `.env` file:
```
PROJECT_ENDPOINT="https://<your-project-endpoint-or-target-uri>"
MODEL_DEPLOYMENT="<your-deployment-name>"
```
- **Project endpoint**: Foundry → **Project** → **Overview** → **Endpoints and keys** (select *Azure AI Foundry*).
- **Target URI** (only if the model was deployed in a different region): Foundry → **Models + Endpoints** → **Deployments** → *your gpt‑4o* → **Target URI**.
- **Deployment name**: Foundry → **Deployments** → **Name** (e.g., `gpt-4o`).

### 6. Authenticate
```bash
az login
# If you have multiple subscriptions:
az account set --subscription "<your-subscription-name-or-id>"
```

### 7. Run the App
```bash
python chat-app.py
```

### 8. Troubleshooting (Quick)
- **401/403 Unauthorized**: run `az login`; ensure role **Cognitive Services User**; verify `PROJECT_ENDPOINT` is correct (Project endpoint vs Target URI).
- **Deployment not found**: check `MODEL_DEPLOYMENT` matches the **Deployment name** exactly (not the model name).
- **429/Rate limit**: you hit TPM/RPM; try again later or reduce frequency.

### 9. Portal Paths (Exact)
- Project endpoint: **Project → Overview → Endpoints and keys → Azure AI Foundry**
- Target URI (cross‑region only): **Models + Endpoints → Deployments → [gpt‑4o] → Target URI**
- Deployment name: **Models + Endpoints → Deployments → Name**

### 10. Cost Guardrails
- Billing is **per token** (system + history + user + output). Keep chat history short.
- Remove unused cross‑region deployments to avoid quota fragmentation and unexpected costs.

---

## ✅ Русская версия

### 1. Предварительные требования
- Python 3.8+
- Установленный Azure CLI (`az --version`)
- Подписка Azure с проектом **Azure AI Foundry** и задеплоенной моделью (например, `gpt-4o`)

### 2. Перейдите в папку проекта
```bash
cd /путь/к/проекту
```

### 3. Создайте и активируйте виртуальную среду
```bash
python3 -m venv labenv
source labenv/bin/activate
```

### 4. Установите зависимости
```bash
pip install azure-identity azure-ai-projects openai python-dotenv
```

### 5. Настройте переменные окружения
Создайте файл `.env`:
```
PROJECT_ENDPOINT="https://<ваш-project-endpoint-или-target-uri>"
MODEL_DEPLOYMENT="<имя-деплоя>"
```
- **Project endpoint**: Foundry → **Project** → **Overview** → **Endpoints and keys** (выбран *Azure AI Foundry*).
- **Target URI** (только если деплой в другом регионе): Foundry → **Models + Endpoints** → **Deployments** → *ваш gpt‑4o* → **Target URI**.
- **Имя деплоя**: Foundry → **Deployments** → **Name** (например, `gpt-4o`).

### 6. Авторизация
```bash
az login
# Если несколько подписок:
az account set --subscription "<имя-или-ID-подписки>"
```

### 7. Запуск приложения
```bash
python chat-app.py
```

### 8. Быстрый разбор проблем
- **401/403 Unauthorized**: выполните `az login`; проверьте роль **Cognitive Services User**; убедитесь, что `PROJECT_ENDPOINT` корректен (Project endpoint vs Target URI).
- **Deployment not found**: проверьте, что `MODEL_DEPLOYMENT` точно совпадает с **именем деплоя** (а не названием модели).
- **429/Rate limit**: достигнуты лимиты TPM/RPM; снизьте частоту запросов.

### 9. Пути в портале
- Project endpoint: **Project → Overview → Endpoints and keys → Azure AI Foundry**
- Target URI (только при другом регионе): **Models + Endpoints → Deployments → [gpt‑4o] → Target URI**
- Имя деплоя: **Models + Endpoints → Deployments → Name**

### 10. Контроль стоимости
- Биллинг **по токенам** (system + history + user + output). Держите историю короткой.
- Удаляйте неиспользуемые деплои в других регионах, чтобы избежать лишних расходов и дробления квот.

---

**Auth sanity (SDK choices):**
- Local dev (recommended): `AzureCliCredential()` + `az login`.
- GUI alternative: `InteractiveBrowserCredential()` (opens a browser tab for login).
