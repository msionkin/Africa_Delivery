# Task5 — MVP Roadmap и Build/Buy/Partner решения

| Файл | Содержание |
|---|---|
| [capability-map.md](capability-map.md) · [capability-map.puml](diagrams/capability-map.puml) · [capability-map.png](diagrams/capability-map.png) | Иерархия возможностей L1/L2 (диаграмма) и таблица: capability, решение Build/Buy/Partner, обоснование, стоимость, риск |
| [value-stream.puml](diagrams/value-stream.puml) · [value-stream.png](diagrams/value-stream.png) | Поток ценности с нанесёнными возможностями и решениями Build/Buy/Partner |
| [services-per-capability.md](services-per-capability.md) | Возможность → модуль или сервис → владелец → внешняя зависимость; нагрузка на владельцев |
| [roadmap.md](roadmap.md) · [roadmap.mmd](diagrams/roadmap.mmd) · [roadmap.png](diagrams/roadmap.png) | Gantt, контрольные точки M1, M2 (MVP), M6, M12: scope, команда, стоимость; что стало с 29 функциями PM и по какому критерию |
| [adr-004-payment-provider.md](adr-004-payment-provider.md) | ADR-004: провайдеры по странам за собственным фасадом, география провайдеров, ≈ 1,97% GMV |
| [transition-architecture.md](transition-architecture.md) | Состояния «сейчас → MVP → 6 мес. → 12 мес.», что меняется на каждом шаге |
| [risk-register.md](risk-register.md) | Риски R-01…R-15: вероятность, влияние, митигация, владелец, триггер |
| [team-structure.md](team-structure.md) | Структура команды, удалённая работа против офиса в Найроби, дежурство 24/7 follow-the-sun |

**Коротко:** из 29 функций PM в MVP целиком входят 2, в урезанном виде — 12; ещё 2 перенесены на M6/M12, 7 — на период после 12 месяцев, 6 вычеркнуты. Строим то, что отличает нас от конкурентов или хранит ПДн. Покупаем типовые SaaS. Платежи и доставку берём у партнёров. Собственная логистика CTO к 15.10 невозможна: по оценке самого CTO она требует 24 чел.-мес., а ёмкость до запуска — 48 чел.-нед. ≈ 11 чел.-мес., считая с 18.08 ([C-04](../Task1/contradictions.md) — 12 чел.-мес., считая с 12.08); к ней вернёмся по триггеру. Собственная платёжная оркестрация без лицензии незаконна, поэтому M-Pesa в Кении подключаем напрямую, а остальное — через Flutterwave и Paystack: ≈ 1,97% GMV при потолке 2,5%.
