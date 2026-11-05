# Price list — цены и тарифы для TCO-модели

**Дата сверки:** 2026-11-04 · **Курсы:** 1 USD = ₦1 346,07 = KES 129,98; 1 EUR = \$1,119 [62]

Цены, на которых построена [tco-model.xlsx](../tco-model.xlsx). В колонке «Комментарий» каждой статьи модели стоит номер пункта P-xx. Облако — on-demand list prices AWS af-south-1 (Кейптаун) без скидок и налогов; AWS взят как основа для цен, окончательный провайдер ещё не выбран. «Оценка» — публичной цены нет, значение экспертное.

## 1. Облако: AWS af-south-1

| ID | Сервис | Цена | Как используется в модели | Источник |
|---|---|---|---|---|
| P-01 | EKS, управляющий слой кластера | \$0,10/кластер-ч (\$73/мес) | 1 кластер в A и B, 2 в C | [1][2] |
| P-02 | EC2 m7g.large / m7g.xlarge | \$0,108 / \$0,2159 за ч | Узлы K8s: A — 3–6, B — 8, C — ~28 узлов xlarge | [1][3] |
| P-03 | EC2 m6i.large | \$0,127/ч (\$93/мес) | Брокеры Kafka в C (6 шт.) | [1][3] |
| P-04 | EC2 r6i.2xlarge | \$0,672/ч (\$491/мес) | ВМ под Oracle в C (2 шт.) | [1][3] |
| P-05 | NAT Gateway; ALB | \$0,057/ч + \$0,057/ГБ; \$0,030/ч + \$0,0095/LCU-ч | По 2 NAT и 1 ALB на кластер | [1][4][5] |
| P-06 | RDS PostgreSQL db.m6g.large | Single-AZ \$0,209/ч; Multi-AZ \$0,417/ч (\$304/мес) | Основная БД Multi-AZ; DR-реплика Single-AZ. db.m7g в регионе нет | [1][6] |
| P-07 | RDS gp3 | \$0,151 (Single-AZ) / \$0,302 (Multi-AZ) за ГБ-мес | 100 ГБ на БД | [1][6] |
| P-08 | ElastiCache Valkey cache.m7g.large | \$0,166/ч (\$121/мес) | Кеш каталога и сессий | [1][7] |
| P-09 | S3 Standard | \$0,0274/ГБ-мес; PUT \$0,006 / GET \$0,0004 за 1 000 | ~1 ТБ фото и бэкапов | [1][8] |
| P-10 | CloudFront (Южная Африка, Кения, Нигерия) | первый 1 ТБ/мес бесплатно; \$0,110/ГБ до 10 ТБ; \$0,105/ГБ до 50 ТБ | ~5 ТБ/мес в 1-й год | [9] |
| P-11 | SQS Standard | \$0,476 за млн запросов; 1 млн/мес бесплатно | ~25 млн сообщений в год | [1][10] |
| P-12 | MSK Serverless | \$0,992/кластер-ч (\$724/мес) + \$0,00198/партиция-ч + \$0,13/ГБ входящего трафика | Шина событий в B | [1][11][12] |
| P-13 | Передача данных из af-south-1 в другой регион | \$0,147/ГБ | DR-репликация (A, B), active-active (C, ~5 ТБ/мес) | [1][3] |
| P-14 | AWS WAF | \$5/web ACL + \$1/правило в мес + \$0,60 за млн запросов | Защита API и веба | [1][13] |
| P-15 | Secrets Manager | \$0,40/секрет-мес + \$0,05 за 10 тыс. вызовов | Ключи партнёров | [1][14] |
| P-16 | ECR | \$0,10/ГБ-мес | Реестр образов | [1][15] |
| P-28 | Исходящий трафик из af-south-1 в интернет | \$0,154/ГБ до 10 ТБ | Справочно: основной трафик идёт через CDN | [1][3] |
| P-29 | AWS Local Zone Lagos | EC2 T3/C5/M5/R5 на ~15% дороже Кейптауна; RDS, ElastiCache, S3 и NAT нет | В модели не используется: управляемой БД в Лагосе нет | [16] |

## 2. Хранение данных в Нигерии

| ID | Позиция | Цена | Как используется в модели | Источник |
|---|---|---|---|---|
| P-37 | ВМ у локального провайдера (Layer3) | vCPU ₦3 267, RAM ₦7 201/ГБ, диск ₦35,65/ГБ в мес → ВМ 4 vCPU/16 ГБ ≈ \$97/мес | A: 2 ВМ + PostgreSQL с репликой и бэкапом ≈ \$450/мес. C: 5 ВМ 8 vCPU/32 ГБ ≈ \$1 100/мес. Цены Rack Centre, MainOne и Galaxy Backbone не публикуются | [44][45] |

## 3. SaaS и инструменты

| ID | Сервис | Цена | Как используется в модели | Источник |
|---|---|---|---|---|
| P-17 | Grafana Cloud Pro | \$19/мес + \$6,50 за 1k серий + ~\$0,55/ГБ логов и трейсов (10k серий и 50 ГБ включены) | ≈ \$230–250/мес в A | [17] |
| P-18 | GitHub Team + Actions | \$4/польз.-мес, 3 000 мин включено, Linux \$0,006/мин | ≈ \$110/мес с ECR | [18][19] |
| P-19 | Expo EAS Production | \$199/мес, 50 тыс. MAU OTA-обновлений | OTA-обновления React Native | [20] |
| P-20 | Sentry Team | \$26/мес | Ошибки мобильного приложения и API | [21] |
| P-21 | Linear Basic; Figma Professional | \$10/польз.-мес; \$16/место-мес | Трекер и дизайн | [22][23] |
| P-22 | Apple Developer; Google Play | \$99/год; \$25 разово | Публикация в сторах | [24][25] |
| P-23 | WhatsApp BSP (360dialog) | €49 ≈ \$59 за номер в мес | 2 номера (NG, KE) | [26] |

## 4. Сообщения

| ID | Позиция | Цена | Как используется в модели | Источник |
|---|---|---|---|---|
| P-24 | WhatsApp utility, по тарифам Meta | Нигерия \$0,0067; Кения \$0,0040 за сообщение | 2 сообщения на заказ | [27][28] |
| P-25 | SMS по локальному транзакционному маршруту | Нигерия ₦6–6,65 ≈ \$0,0045–0,0049; Кения ~KES 0,8 ≈ \$0,006 | 3 SMS на заказ × \$0,005. Международный маршрут в Нигерию стоит \$0,39, использовать его нельзя | [29][30][31][32] |
| P-26 | USSD-сессия | Кения KES 1,25 ≈ \$0,0096; в Нигерии с 06.2025 платит абонент (₦6,98 за 120 с) | Только Кения: 10% заказов через USSD × ~35% (доля Кении) × 3 сессии | [33][35] |
| P-27 | USSD-код, Кения | Общий код агрегатора с подкодом: ~KES 3 000/мес + 5 000 разово ≈ \$23 + \$38. Выделенный код: Safaricom KES 145k разово + 70k/мес; Airtel 116k + 46,4k/мес; Telkom 58k + 116k/мес | Год 1 — общий код. С Года 2, когда USSD-заказов больше ~18 тыс./мес и код стоит меньше \$0,10 на заказ, — выделенные коды у трёх операторов: ≈ \$1 790/мес и \$2 670 разово | [34][63] |

## 5. Платежи

| ID | Провайдер | Тариф | Источник |
|---|---|---|---|
| P-30 | Flutterwave, Нигерия | локальные карты и переводы 2,0% + 7,5% VAT = 2,15%; международные карты 4,8%; выплаты ₦10–50 | [36] |
| P-31 | Flutterwave, Кения | mobile money (M-Pesa) 2,9%; карты 3,2%; выплаты KES 100 | [37] |
| P-32 | Paystack | Нигерия 1,5% + ₦100, не более ₦2 000; Кения M-Pesa 1,5%; выплаты M-Pesa KES 20–60 | [38][39] |
| P-33 | M-Pesa напрямую (Daraja, Buy Goods) | 0% до KES 500; 0,55% до KES 200 за транзакцию (с 07.08.2026); доступ к API бесплатный | [40][41] |
| P-34 | Выплаты продавцам | NG ₦10–50 (+₦50 гербовый сбор от ₦10 000); KE KES 20–100 | [36][37][39] |

**Смешанная ставка 2,3% GMV (оценка смеси):**
- Нигерия, ~60% GMV, по ~2,15%.
- Кения, ~35% GMV, M-Pesa по ~1,5%.
- Международные карты, ~5% GMV, по 4,8%.
- Выплаты продавцам — около +0,15%.
- Резерв на маршрутизацию через резервного провайдера — около +0,1%.

Итого ≈ 2,3% при потолке 2,5% (А6 п. 6). Если в Кении основным провайдером будет Flutterwave (2,9%), потолок будет превышен.

## 6. Логистика

| ID | Позиция | Цена | Источник |
|---|---|---|---|
| P-35 | Сервисные сборы Sendbox / Sendy | Не публикуются: Sendbox зарабатывает на наценке к тарифу курьера. В модели — оценка \$0,10 за заказ | [42] |
| P-36 | Статус Sendy | По публичным данным в 2023 году работа в Нигерии остановлена, компания продавала активы. Устойчивость партнёра в Кении не подтверждена; за адаптером его можно заменить | [43] |

## 7. Лицензии

| ID | Позиция | Цена | Источник |
|---|---|---|---|
| P-38 | Oracle Database EE; Active Data Guard; поддержка | \$47 500 и \$11 500 за процессорную лицензию; поддержка 22% в год → 4 лицензии = \$236k + \$51,9k/год (только вариант C) | [46][47] |
| P-39 | Лицензирование Oracle на AWS | 2 vCPU = 1 процессорная лицензия; 4 лицензии покрывают 8 vCPU | [48] |

## 8. Команда и услуги

| ID | Позиция | Цена | Источник |
|---|---|---|---|
| P-40 | Контрактор senior (Go, React Native, TypeScript) | Нигерия \$33/ч, Кения \$32/ч ≈ \$5 500–5 700/мес → в модели \$66k/год | [49][50][52] |
| P-41 | SRE / DevOps strong senior | \$6 000–6 900/мес → в модели \$78k/год | [49][50][51] |
| P-42 | Oracle DBA (контракт) | США ~\$57/ч ≈ \$118k/год; Великобритания \$112–130k → в модели \$118k | [53][54] |
| P-43 | Комиссия рекрутингового агентства | 15–25% годового оклада, медиана ~20% → ~\$13k за найм | [55][56] |
| P-44 | Пентест | веб + API + облако \$15–35k; мобильные платформы \$7–35k каждая → A \$25k, B \$28k, C \$35k в год | [57][58] |

## 9. Комплаенс

| ID | Позиция | Цена | Источник |
|---|---|---|---|
| P-45 | NDPC (Нигерия) | регистрация UHL ₦250k ≈ \$186; ежегодная подача аудита ₦500k–1m ≈ \$371–743 | [59][60] |
| P-46 | ODPC (Кения) | регистрация KES 4–40k ≈ \$31–308; продление каждые 24 мес. KES 2–25k | [61] |

Госпошлины составляют меньше \$1,1k в год. Остальной бюджет на комплаенс (\$30k в 1-й год, \$15k далее) — юристы, DPCO-аудит и DPO на аутсорсе: это оценка, публичных ставок нет.

## Источники

1. AWS Price List API, af-south-1, публикация 2026-11-03 — https://pricing.us-east-1.amazonaws.com/offers/v1.0/aws/index.json
2. Amazon EKS pricing — https://aws.amazon.com/eks/pricing/
3. Amazon EC2 On-Demand pricing и Data Transfer — https://aws.amazon.com/ec2/pricing/on-demand/
4. Amazon VPC pricing (NAT Gateway) — https://aws.amazon.com/vpc/pricing/
5. Elastic Load Balancing pricing — https://aws.amazon.com/elasticloadbalancing/pricing/
6. Amazon RDS for PostgreSQL pricing — https://aws.amazon.com/rds/postgresql/pricing/
7. Amazon ElastiCache pricing — https://aws.amazon.com/elasticache/pricing/
8. Amazon S3 pricing — https://aws.amazon.com/s3/pricing/
9. Amazon CloudFront pay-as-you-go pricing — https://aws.amazon.com/cloudfront/pricing/pay-as-you-go/
10. Amazon SQS pricing — https://aws.amazon.com/sqs/pricing/
11. Amazon MSK pricing — https://aws.amazon.com/msk/pricing/
12. MSK Serverless в Кейптауне — https://aws.amazon.com/about-aws/whats-new/2026/03/amazon-msk-serverless-3-regions
13. AWS WAF pricing — https://aws.amazon.com/waf/pricing/
14. AWS Secrets Manager pricing — https://aws.amazon.com/secrets-manager/pricing/
15. Amazon ECR pricing — https://aws.amazon.com/ecr/pricing/
16. AWS Local Zones features — https://aws.amazon.com/about-aws/global-infrastructure/localzones/features/
17. Grafana Cloud pricing — https://grafana.com/pricing/
18. GitHub pricing — https://github.com/pricing
19. GitHub Actions runner pricing — https://docs.github.com/en/billing/reference/actions-runner-pricing
20. Expo pricing — https://expo.dev/pricing
21. Sentry pricing — https://sentry.io/pricing/
22. Linear pricing — https://linear.app/pricing
23. Figma pricing — https://www.figma.com/pricing/
24. Apple Developer Program — https://developer.apple.com/support/enrollment/
25. Google Play developer registration — https://support.google.com/googleplay/android-developer/answer/6112435
26. 360dialog prices and plans — https://docs.360dialog.com/docs/360dialog/prices-plans-and-payment-options
27. Meta WhatsApp Business Platform pricing — https://developers.facebook.com/docs/whatsapp/pricing/
28. Twilio WhatsApp rate card (тарифы Meta с 01.10.2026) — https://www.twilio.com/content/dam/twilio-com/pricing-data/en/WhatsAppPricing-pricing-details.csv
29. Nigeria SMS pricing — https://www.sent.dm/en/resources/sms-pricing/nigeria-sms-pricing
30. BulkSMS Nigeria pricing — https://www.bulksmsnigeria.com/pricing
31. Kenya SMS pricing — https://www.sent.dm/en/resources/sms-pricing/kenya-sms-pricing
32. Twilio SMS pricing, Nigeria — https://www.twilio.com/en-us/sms/pricing/ng
33. Africa's Talking USSD — https://africastalking.com/ussd?lang=en
34. Africa's Talking: стоимость выделенного USSD-кода — https://help.africastalking.com/en/articles/1163556-how-much-is-a-dedicated-ussd-code
35. Nigeria USSD end-user billing — https://nairametrics.com/2025/06/18/telecom-operators-to-begin-direct-airtime-charges-for-ussd-services-from-june-18
36. Flutterwave Nigeria pricing — https://flutterwave.com/ng/pricing
37. Flutterwave Kenya pricing — https://flutterwave.com/ke/pricing
38. Paystack transaction fees — https://support.paystack.com/en/articles/2130306
39. Paystack transfer fees — https://support.paystack.com/en/articles/2130370
40. Safaricom M-Pesa business charges (The Kenya Times) — https://thekenyatimes.com/business/safaricom-reduces-m-pesa-business-charges-list-of-new-charges/
41. Safaricom merchant fees (The Standard) — https://www.standardmedia.co.ke/business/article/2001554316/safaricom-halves-m-pesa-merchant-fees-in-cbk-led-move
42. Sendbox review — https://brands.ng/sendbox-nigeria-review-best-delivery-platform-for-small-businesses/
43. Sendy status (Techpoint Africa) — https://techpoint.africa/news/sendy-ceasing-operations-selling-assets
44. Layer3 Cloud pricing — https://www.layer3.cloud/pricing
45. Cloud providers in Nigeria (Lineserve) — https://www.lineserve.net/blog/cloud-providers-in-nigeria
46. Oracle price list 2026 (Redress Compliance) — https://redresscompliance.com/oracle-price-list-2026-key-changes-and-analysis
47. Oracle Database EE options pricing (Redress Compliance) — https://redresscompliance.com/oracle-database-ee-options-pricing-audit
48. Oracle Licensing in the Cloud Computing Environment — https://www.oracle.com/assets/cloud-licensing-070579.pdf
49. Lemon.io rate calculator, Nigeria — https://lemon.io/rate-calculator/nigeria/
50. Lemon.io rate calculator, Kenya — https://lemon.io/rate-calculator/kenya/
51. Lemon.io rate calculator, Ukraine — https://lemon.io/rate-calculator/ukraine/
52. Arc.dev salaries, Nigeria — https://www.arc.dev/salaries/software-engineers-in-nigeria
53. ZipRecruiter, Contract Oracle DBA — https://www.ziprecruiter.com/Jobs/Contract-Oracle-Dba/2
54. IT Jobs Watch, Oracle DBA contracts UK — https://www.itjobswatch.co.uk/contracts/uk/oracle%20dba.do
55. Recruitment agency costs (Underdog.io) — https://underdog.io/blog/recruitment-agency-costs
56. Tech recruiter fees (Dover) — https://dover.com/blog/tech-recruiter-fees-cost-guide
57. Penetration testing cost (Astra) — https://www.getastra.com/blog/penetration-testing/cost/
58. Mobile app pentest cost (Stingrai) — https://www.stingrai.io/blog/mobile-app-penetration-testing-cost-2026
59. NDPC Guidance Notice on registration (Templars) — https://www.templars-law.com/app/uploads/2024/02/CLIENT-ALERT-NDPC-ISSUES-GUIDANCE-NOTICE.pdf
60. NDPC Compliance Audit Return fees (Docufy) — https://docufy.ng/blog/ndpc-compliance-audit-return-deadline-and-fee
61. ODPC registration fees (CM Advocates) — https://cmadvocates.com/blog/registration-of-data-controllers-and-data-processors/
62. Курсы валют, ExchangeRate-API — https://open.er-api.com/v6/latest/USD
63. USSD codes in Kenya (Celcom Africa) — https://celcomafrica.com/blog/ussd-codes-in-kenya-guide/

## Assumptions

- Цены AWS — list prices на дату сверки; Savings Plans и скидки не учтены, потому что CFO запрещает обязательства (А6 п. 3).
- Структура GMV по странам (~60% Нигерия, ~35% Кения, ~5% международные карты) — допущение для смешанной комиссии; уточнится по факту первых месяцев.
- Источники [29], [31], [40], [41], [45], [46], [47], [49]–[58], [60], [61] и [63] — вторичные; цена общего USSD-кода [63] не подтверждена прайсом агрегатора (агрегаторы, СМИ, консалтинг). Тарифы Termii, Africa's Talking (SMS), Safaricom B2C, Sendbox, нигерийских ЦОД и DPCO не публикуются.
- Дежурства, менеджмент, self-hosted observability в C и расширение команды — экспертные оценки без внешнего источника.
- Тарифы SMS и USSD в Кении взяты из вторичных источников и сниппетов официальных страниц, потому что сами страницы были недоступны.
