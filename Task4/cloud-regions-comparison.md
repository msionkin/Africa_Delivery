# Сравнение облачных регионов

**Дата:** 2026-08-28 · **Сверка цен и задержек:** 2026-11-07 · **Курс:** 1 USD = ₦1 346 ([price-list.md](../Task2/supporting/price-list.md))

> Цены — on-demand list prices без скидок и НДС, потому что CFO запрещает обязательства (А6 п. 3).  
> [n] — источники в конце документа.  
> **Базовый контур MVP** — 1 управляющий слой K8s, 3 узла по 2 vCPU/8 ГБ, PostgreSQL HA 2 vCPU/8 ГБ со 100 ГБ, кеш ~6 ГБ с репликой и 5 ТБ трафика через CDN в месяц (как в [TCO-модели](../Task2/tco-model.xlsx)).

## Сравнение

| Провайдер / регион | Доступные сервисы | Latency, RTT (мс) [10] | Цена базового контура, \$/мес | SLA | Вывод |
|---|---|---|---|---|---|
| **AWS af-south-1**, Кейптаун, 3 зоны | EKS, RDS PostgreSQL Multi-AZ (без db.m7g), ElastiCache Valkey, SQS, KMS, CloudHSM. CloudFront: PoP в Лагосе и Найроби | Лагос 240 (маршрут через Европу), Найроби 67 | **≈ 1 450** (+100–250 прочее) [1] | EC2 в нескольких зонах 99,99%; EKS 99,95%; RDS Multi-AZ 99,95% [7] | **DR-площадка.** Другой провайдер и другой город относительно основного региона. На части маршрутов из Лагоса задержка втрое выше, поэтому как основной регион не подходит. Второго региона в Африке нет. Оплата по счёту — через партнёра AWS [11] |
| **GCP africa-south1**, Йоханнесбург, 3 зоны | GKE, Cloud SQL PostgreSQL HA, Memorystore Valkey, Pub/Sub, Cloud KMS, Cloud HSM (только multi-tenant). Cloud CDN: PoP в Лагосе; в Кении только Момбаса | Лагос 79, Найроби 55 | **≈ 1 160** (+100–200 прочее) [2] | Compute в нескольких зонах 99,99%; GKE regional 99,95%; Cloud SQL HA 99,95% [8] | **Основной регион.** Самый дешёвый и самый близкий к обоим рынкам запуска. Второго региона в Африке нет, поэтому DR — у другого провайдера. Оплата по счёту — через реселлера: напрямую нужен год истории и \$3,5k/мес [11] |
| **Azure South Africa North**, Йоханнесбург, 3 зоны (+ South Africa West, Кейптаун) | AKS, PostgreSQL Flexible Server с зонным HA, Managed Redis, Service Bus, Key Vault / Managed HSM. Front Door: PoP в Лагосе и Найроби. **SA West:** без зон, доступ только по заявке для DR внутри страны | Лагос 79, Найроби 55 | **≈ 1 490** (+100–250 прочее) [3] | VM в нескольких зонах 99,99%; AKS Standard 99,95% (Free — без SLA); PostgreSQL с зонным HA 99,99% [9] | **Резервный вариант.** Единственный провайдер с двумя регионами в ЮАР, но доступ к SA West не гарантирован, а managed PostgreSQL там не подтверждён. Оплата по счёту — через Microsoft CSP [11] |
| **Layer3**, Лагос + Абуджа (Нигерия) | ВМ, Kubernetes, managed PostgreSQL, S3-совместимое хранилище. Опубликованного KMS нет | Пользователи в Лагосе — < 10; до ядра в Йоханнесбурге — 79 | ВМ 2 vCPU/8 ГБ ≈ \$49 → узел NG **≈ 320** [4] | 99,9% [4] | **Площадка для ПДн нигерийцев.** Единственное облако в Нигерии с managed PostgreSQL, SLA и публичными ценами; остальные операторы ЦОД в Лагосе (Rack Centre, Equinix, OADC, Digital Realty) сдают только место в стойке под собственные серверы (колокация), облачных сервисов у них нет [5]. Счёт в NGN напрямую |
| AWS Local Zone, Лагос | Только EC2 (t3, m5, c5, r5, g4dn), gp2, ALB. Нет RDS, ElastiCache и S3 | Пользователи в Лагосе — < 10 | PostgreSQL на 2× m5.xlarge **≈ 426** [6] | Одна зона, отдельного SLA нет | **Отклонён.** Одна площадка без managed БД: реплика уходит из Нигерии, а это нарушает и наше допущение, и руководство NITDA |

## Итог

- **Выбрано:** ядро — GCP africa-south1, DR — AWS af-south-1, ПДн нигерийцев — Layer3 (Лагос, реплика в Абудже). Обоснование — [ADR-003](adr-003-data-residency.md).
- **Облако:** GCP ≈ \$1 310, HA VPN до Layer3 \$73, Layer3 ≈ \$320, DR в AWS ≈ \$190, трафик репликации ≈ \$10 — **≈ \$1 900/мес**, в пределах потолка \$5k.
- **Согласование с CFO** (А6, так как эта позиция дороже \$500/мес): к предложению использования GCP прикладывается эта таблица. Рассмотренные альтернативы — AWS af-south-1 (≈ \$1 450/мес) и Azure South Africa North (≈ \$1 490/мес).

## Assumptions

- Задержки — RTT между городами определены по WonderNetwork, а не до конкретного облачного эндпоинта. Значение 240 мс Лагос–Кейптаун зависит от маршрута провайдера связи: перед подписанием договора замеряем его синтетикой из Лагоса.
- Цену managed PostgreSQL у Layer3 оцениваем как цену ВМ того же размера: она не опубликована.

## Источники

1. AWS Price List API, af-south-1 — https://pricing.us-east-1.amazonaws.com/offers/v1.0/aws/index.json
2. GCP: Compute Engine, Cloud SQL, Memorystore for Valkey, Cloud CDN pricing — https://cloud.google.com/products/compute/pricing/general-purpose ; https://cloud.google.com/sql/pricing ; https://cloud.google.com/memorystore/docs/valkey/pricing ; https://cloud.google.com/cdn/pricing
3. Azure Retail Prices API (southafricanorth, southafricawest) — https://prices.azure.com/api/retail/prices ; Azure restricted regions — https://learn.microsoft.com/azure/reliability/regions-list
4. Layer3 Cloud pricing и PaaS — https://layer3.cloud/pricing ; https://www.layer3.cloud/platform-as-a-service
5. Rack Centre — https://rack-centre.com/rack-centres-lgs2-12mw-data-centre-commissioned/ ; Equinix Lagos — https://www.equinix.com/data-centers/europe-colocation/nigeria-colocation/lagos-data-centers ; OADC Lagos LOS1 — https://baxtel.com/data-center/oadc-lagos-los1 ; Digital Realty LKK2 (Лагос) — https://baxtel.com/news/digital-realty-launches-2mw-data-center-in-nigeria
6. AWS Local Zones features — https://aws.amazon.com/about-aws/global-infrastructure/localzones/features/
7. AWS SLA — https://aws.amazon.com/compute/sla/ ; https://aws.amazon.com/eks/sla/ ; https://aws.amazon.com/rds/sla/
8. GCP SLA — https://cloud.google.com/compute/sla ; https://cloud.google.com/kubernetes-engine/sla ; https://cloud.google.com/sql/sla
9. Azure SLA; AKS uptime SLA — https://www.microsoft.com/licensing/docs/view/Service-Level-Agreements-SLA-for-Online-Services ; https://learn.microsoft.com/azure/aks/uptime-sla
10. WonderNetwork, задержки из Лагоса и Найроби — https://wondernetwork.com/pings/Lagos ; https://wondernetwork.com/pings/Nairobi
11. Оплата по счёту: AWS — https://www.repost.aws/questions/QUibKJdC08QSCljMi6oTzU-A/switch-from-credit-card-payment-to-invoice ; GCP — https://docs.cloud.google.com/billing/docs/how-to/invoiced-billing ; Azure — https://learn.microsoft.com/en-us/azure/cost-management-billing/manage/pay-by-invoice
