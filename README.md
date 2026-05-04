# Анализ оттока клиентов Telco_Customer_Churn

## Обзор проекта

Аналитический проект по снижению оттока клиентов телеком-оператора на основе датасета **Telco Customer Churn** (IBM Sample Dataset). 

**Ключевой результат:** потенциальное сохранение до **$637K годовой выручки** (37,5% от текущих потерь в $1,7M).

---

## Данные

| Параметр | Значение |
|----------|----------|
| Датасет | Telco Customer Churn (IBM Sample) |
| Клиентов | 7 043 |
| Признаков | 21 (3 числовых, 18 категориальных) |
| Целевая переменная | Churn (Yes / No) |
| Пропуски | Нет |
| Баланс классов | Несбалансирован (26,5% / 73,5%) |

**Категории признаков:**
- **Демография:** Gender, SeniorCitizen, Partner, Dependents
- **Услуги:** PhoneService, MultipleLines, InternetService, OnlineSecurity, OnlineBackup, DeviceProtection, TechSupport, StreamingTV, StreamingMovies
- **Аккаунт:** Tenure, Contract, PaperlessBilling, PaymentMethod
- **Финансы:** MonthlyCharges, TotalCharges

---

## Методология

| Этап | Описание |
|------|----------|
| **1. EDA** | Распределение признаков, корреляции с оттоком, выявление ключевых драйверов |
| **2. Сегментация** | Выделение риск-групп по Contract + Tenure + Protection count |
| **3. Моделирование** | Logistic Regression (baseline) → Random Forest (ROC-AUC **0,839**) |
| **4. Экономика** | Расчёт ROI для двух сегментов удержания |

---

## Ключевые инсайты

**Топ-3 драйвера оттока:**

1. **Тип контракта** — Month-to-month даёт отток 42,7% против 2,8% у Two year (разница в 15×)
2. **Срок (Tenure)** — первый год критичен: отток 52,9% (1–14 мес.) против 6,6% (60–72 мес.), разница в 8×
3. **Защитные услуги** — клиенты без защиты уходят в 10 раз чаще (56,7% → 5,3% при 4 услугах)

**Главный рычаг:** защитные услуги снижают отток с **57% до 5%** при полном наборе.

---

## Модель

| Метрика | Значение |
|---------|----------|
| ROC-AUC | **0,839** |
| Precision | 0,55 |
| Recall | 0,72 |
| F1-score | 0,62 |

**Топ-5 признаков по важности (Random Forest):**
1. `tenure` (0,127) — стаж клиента
2. `contract_Month-to-month` (0,106) — тип контракта
3. `contract_Two_year` (0,059)
4. `totalcharges` / `monthlycharges` — финансовые метрики
5. `onlinesecurity_No` (0,050) — отсутствие защиты

---

## Бизнес-рекомендации

| Мера | Целевой сегмент | Ожидаемый эффект | Сохранённая выручка |
|------|-----------------|------------------|---------------------|
| **Пакет «Защита клиента»** (OnlineSecurity + TechSupport, скидка 50% первый месяц) | Клиенты с интернетом, но без защиты (2 734 чел.) | Отток с 57% → 15% | **$631K** |
| **Перевод на годовые контракты** (бонус 1 месяц за переход, таргетинг на 10-й месяц) | Month-to-month клиенты (1 994 чел.) | Отток с 51% → 25% | **$367K** |
| **Перевод на автоплатёж** (Electronic check → автоплатёж, бонусные баллы) | Клиенты на electronic check | Отток с 45% → 15% | Дополнительно |

>**Итоговый эффект:** до **$637K** сохранённой годовой выручки (сегменты пересекаются, эффект неаддитивный).

---

## Технический стек

- Python (pandas, numpy, scikit-learn, matplotlib, seaborn)
- Jupyter Notebook
- Модели: Logistic Regression, Random Forest
- Метрики: ROC-AUC, Precision, Recall, F1-score

---

## Структура репозитория
```text
├── data/
│   └── Telco-Customer-Churn.csv
├── notebooks/
│   ├── Telco_Customer_Churn.ipynb
├── presentation/
│   └── Churn.pptx
├── README.md
└── requirements.txt 
```

---

## Установка и запуск

```bash
# Клонировать репозиторий
git clone https://github.com/YanaLavr/Telco_Customer_Churn_ML.git
cd Telco_Customer_Churn_ML

# Установить зависимости
pip install -r requirements.txt

# Запустить Jupyter
jupyter notebook