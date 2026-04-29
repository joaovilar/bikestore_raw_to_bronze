#  Raw to Bronze Volumes - Pipeline BikeStore

Pipeline de ingestão de dados da camada **RAW** para **BRONZE** utilizando **Volumes do Unity Catalog** no **Databricks**.

---

##  Visão Geral

Este projeto implementa um pipeline de dados para o dataset **BikeStore**, realizando a ingestão de arquivos CSV (camada RAW) para o formato **Delta Lake** (camada BRONZE) no Databricks com Unity Catalog.

---

##  Estatísticas do Pipeline

| Tabela | Linhas | Colunas |
|--------|--------|---------|
| brands | 9 | 4 |
| categories | 7 | 4 |
| customers | 1,445 | 11 |
| order_items | 4,722 | 8 |
| orders | 1,615 | 10 |
| products | 321 | 8 |
| staffs | 10 | 10 |
| stocks | 939 | 5 |
| stores | 3 | 10 |
| **TOTAL** | **~9,071** | - |

---

##  Arquitetura de Armazenamento

###  Storage Account: `stgdatalakebr01`

raw/ # Dados crus (CSV originais)
└── *.csv

bronze/ # Dados tratados em Delta Lake
└── [tabelas]/



---

## 🗂️ Unity Catalog: `bikesales`

```
bikesales/
├── raw
│   └── Volume (EXTERNAL): files
│       └── LOCATION: abfss://raw@stgdatalakebr01.dfs.core.windows.net/
│           ├── brands.csv
│           ├── customers.csv
│           ├── orders.csv
│           └── ...
│
├── bronze
│   └── Dados armazenados em Delta via Volume
│
└── views
    └── Views persistentes para consumo
        ├── vw_brands
        ├── vw_customers
        ├── vw_orders
        └── ...
```


---

##  Limitações do Unity Catalog

###  O que NÃO é permitido

- `CREATE TABLE` com `LOCATION` apontando para `/Volumes/...`
- `CREATE TABLE` utilizando External Volume diretamente
- Operação `PATH_CREATE_TABLE on volume`

---

##  Soluções implementadas

| Opção | Descrição | Status |
|-------|-----------|--------|
| **Views** | `CREATE VIEW ... AS SELECT * FROM delta.\`/caminho/\`` |  Implementado |
| **Managed Tables** | `df.write.saveAsTable("schema.tabela")` |  Alternativa recomendada |
---
<img width="842" height="420" alt="image" src="https://github.com/user-attachments/assets/e695b1f9-f340-46ad-a134-7a45effc0e5d" />

---
<img width="730" height="406" alt="image" src="https://github.com/user-attachments/assets/62b485f8-409f-42fe-8f87-7ea5743bd380" />

---
<img width="599" height="325" alt="image" src="https://github.com/user-attachments/assets/445bff70-b050-4292-abb9-98e601860e25" />
---

<img width="456" height="258" alt="image" src="https://github.com/user-attachments/assets/ed7a7fec-3428-4838-9b53-c820d4515574" />



