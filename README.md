#  Raw to Bronze Volumes - Pipeline BikeStore

Pipeline de ingestão de dados da camada **RAW** para **BRONZE** utilizando **Volumes do Unity Catalog** no **Databricks**.

---

### 🏗️ Estrutura de Armazenamento e Governança

Este projeto utiliza uma abordagem de **Tabelas Externas** e **Volumes**, separando logicamente a computação do armazenamento:

* **Volumes Externos (Unity Catalog):** Utilizados para o mapeamento da camada `raw`. Isso permite o acesso direto aos arquivos CSV armazenados no Azure Data Lake Gen2 sem a necessidade de montagens (mounts) legadas
* **External Tables (Bronze):** As tabelas da camada Bronze foram criadas apontando para localizações externas (`abfss://`). Essa estratégia garante a persistência dos dados brutos processados em formato Delta

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

---
<img width="631" height="584" alt="image" src="https://github.com/user-attachments/assets/4222bcdb-a65d-4adc-85ab-6a1ddea6c3cb" />

---
<img width="427" height="411" alt="image" src="https://github.com/user-attachments/assets/90af314b-1c0d-4b2b-83d3-8d5e428bdad6" />

---


