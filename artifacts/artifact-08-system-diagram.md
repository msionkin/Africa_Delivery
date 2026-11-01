# Артефакт 8. Схема системы

Схему приложения СТО первоначально отрисовал буквально на салфетке.

![artifact-08.1-cto-scheme.png](artifact-08.1-cto-scheme.png)

Однако позже он всё-таки передал её в более читаемом виде.

![artifact-08.2-cto-scheme-slide.png](artifact-08.2-cto-scheme-slide.png)

Та же схема в виде Mermaid-диаграммы:

```mermaid
block-beta
    columns 6
    Title["Africa Delivery - <u>Nigeria</u>, Kenya, SA, 15 countries"]:6
    space:3 CoreLabel["Core"] space:2
    Sellers["Sellers"] space:2 Catalog["Catalog"] space User1(("☺"))
    space:2 Inventories["Inventories"] Ordering["Ordering"] App["Delivery<br/>App"] User2(("☺"))
    MarketShare["<u>40% market share</u>"] space Reports["Reports"] Notifications["Notifications"] space User3(("☺"))
    TCO["TCO- ??"] space Payments["Payments"] Fulfillment["Fulfillment"] Platforms["Apple/google/Iphone/<br/>Android"] space
    space:2 PaymentProviders(["Payment providers<br/>(M-PESA)"]) LogisticProviders(["Logistic<br/>providers"]) Features["Features from PM"] space
    CloudNative["CTO: Cloud Native !!!"]:2 space Couriers["<u>couriers</u>"] GDPR["GDPR data protection - Minister"]:2

    Sellers -- "API" --> Catalog
    Payments -- "API" --> Sellers
    Catalog --> App
    App --> Ordering
    Notifications --> App
    Payments --> PaymentProviders
    Fulfillment --> LogisticProviders
    App --- Platforms
    Platforms --- Features

    classDef box fill:#efefef,stroke:#555555,color:#000000
    classDef provider fill:#f4cccc,stroke:#555555,color:#000000
    classDef user fill:#efefef,stroke:#888888,color:#000000
    classDef note fill:none,stroke:none,color:#808080
    classDef alert fill:none,stroke:none,color:#ff0000

    class Sellers,Inventories,Reports,Payments,Catalog,Ordering,Notifications,Fulfillment,App box
    class PaymentProviders,LogisticProviders provider
    class User1,User2,User3 user
    class Title,CoreLabel,MarketShare,TCO,Couriers,Platforms,Features note
    class CloudNative,GDPR alert
```
