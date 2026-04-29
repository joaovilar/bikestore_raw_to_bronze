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
<img width="842" height="420" alt="image" src="https://github.com/user-attachments/assets/e695b1f9-f340-46ad-a134-7a45effc0e5d" />

---
<img width="730" height="406" alt="image" src="https://github.com/user-attachments/assets/62b485f8-409f-42fe-8f87-7ea5743bd380" />

---
<img width="599" height="325" alt="image" src="https://github.com/user-attachments/assets/445bff70-b050-4292-abb9-98e601860e25" />

---

<img width="456" height="258" alt="image" src="https://github.com/user-attachments/assets/ed7a7fec-3428-4838-9b53-c820d4515574" />

---

<img width="272" height="253" alt="image" src="https://github.com/user-attachments/assets/c7519ecb-2e88-4de8-8462-28e11e77bcc1" />

---
<img width="537" height="484" alt="image" src="https://github.com/user-attachments/assets/583a098f-2426-482a-89bf-906521a2d945" />

