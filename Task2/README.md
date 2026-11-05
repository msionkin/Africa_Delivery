# Task2 — Архитектурное видение и выбор архитектурного стиля

| Файл | Содержание |
|---|---|
| [options-comparison.md](options-comparison.md) | Три варианта (модульный монолит, SOA, микросервисы CTO) по единым критериям, вывод |
| [tco-model.xlsx](tco-model.xlsx) | TCO на 5 лет: «Допущения», листы TCO-A / TCO-B / TCO-C по вариантам, «Сравнение» |
| [price-list.md](supporting/price-list.md) | Цены и тарифы для TCO-модели (P-01…P-46) и список источников |
| [adr-001-architecture-style.md](adr-001-architecture-style.md) | ADR-001: модульный монолит на Go и managed-сервисах, late binding и триггеры пересмотра |
| [c4-context.puml](c4-context.puml) · [c4-context.png](c4-context.png) | C4 Context: система, роли, M-Pesa, Flutterwave, Paystack, Sendbox, Sendy, WhatsApp Business API, SMS/USSD-шлюз |
| [use-cases.puml](use-cases.puml) · [use-cases.png](use-cases.png) | Use Case: ключевые сценарии MVP |
| [architecture-vision.md](architecture-vision.md) | Architecture Vision: цели, целевое состояние, принципы, НФТ, roadmap, что откладываем |
| [nairobi-slide.pdf](nairobi-slide.pdf) · [nairobi-slide.html](supporting/nairobi-slide.html) | Слайд для встречи в Найроби и его исходник |
| [nairobi-script.md](nairobi-script.md) | Транскрипт выступления на 10 минут |

**Коротко:** выбран модульный монолит. Это единственный вариант, который успевает к 15.10, укладывается в \$5k/мес (\$3,0k) и в команду из 6 человек. За 5 лет он дешевле предложения CTO на \$8,0M без учёта транзакционных расходов. Стоимость заказа — \$3,54 на запуске и \$1,12 через 12 месяцев. Цена ошибки «построили под 10M, пришло 50 тыс.» — \$1,7M за первый год.
