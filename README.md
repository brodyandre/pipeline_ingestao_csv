# pipeline_ingestao_csv

# 📊 Pipelines de Dados — Transformando Dados Brutos em Insights Valiosos de forma profissional   

![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Processing-orange?logo=pandas)
![SQLite](https://img.shields.io/badge/SQLite-Database-lightgrey?logo=sqlite)
![Airflow](https://img.shields.io/badge/Airflow-Orchestration-red?logo=apacheairflow)
![Status](https://img.shields.io/badge/Status-Em%20Desenvolvimento-yellow)

> **Autor**: [Luiz André de Souza](https://github.com/brodyandre)  

---

## 🚀 O que é um Pipeline de Dados?

Um **pipeline de dados** é como uma *fábrica inteligente* que transforma dados brutos em informações confiáveis.  

Ele segue **cinco pilares principais**:

1. **Ingestão** → coleta de dados (APIs, CSV, bancos, streams).  
2. **Transformação** → limpeza, padronização e enriquecimento.  
3. **Validação** → checagem de integridade e consistência.  
4. **Persistência** → armazenamento em bancos de dados ou data lakes.  
5. **Entrega** → relatórios, dashboards, APIs e arquivos.  

---

## 🏗️ Arquitetura de Pipelines Robustos  

Além das etapas operacionais, um pipeline de dados **profissional** deve contemplar:  

- ⚡ **Orquestração** → Airflow, Prefect, Dagster, Cron Jobs  
- 📜 **Logging & Monitoramento** → execuções, volumetria, erros  
- 🌀 **Versionamento** → Git para código, schemas e regras  
- 🧩 **Modularização** → scripts independentes (`extract.py`, `transform.py`, `validate.py` etc.)  
- 🔐 **Segurança** → variáveis de ambiente, gestão de permissões  
- 🖥️ **Ambientes distintos** → DEV, Homologação e Produção  

> 💡 *Um pipeline que não possui governança é apenas um script frágil disfarçado de automação.*

---

## ⚙️ Exemplo Didático: Pipeline de Ingestão de dados com Validação de aquivo CSV  

### 📌 Contexto do Problema  
Uma aplicação recebe diariamente um **CSV com dados de usuários** contendo:  

- `id` → Identificador do usuário  
- `nome` → Nome completo  
- `email` → E-mail válido  
- `idade` → Idade declarada  

🎯 Objetivo:  
✔️ Validar os dados  
✔️ Descartar inconsistências  
✔️ Persistir apenas registros válidos em um banco SQL  

---

### 🔄 Fluxo da Solução  

1. 📥 **Leitura segura do CSV**  
2. 🧹 **Remoção de registros incompletos**  
3. 🔍 **Validação semântica** (idade numérica, e-mails válidos)  
4. 🔄 **Conversão de tipos**  
5. 🗄️ **Persistência em banco SQL**  
6. 📑 **Logs de execução**  

---

### 💻 Código Exemplo (Python + Pandas + SQLite)  

```python
import pandas as pd
import sqlite3

# 1. Leitura do CSV
df = pd.read_csv("usuarios.csv", sep=",", encoding="utf-8")

# 2. Remover registros incompletos
df = df.dropna(subset=["nome", "email", "idade"])

# 3. Validar idade como numérica
df = df[df["idade"].apply(lambda x: str(x).isdigit())]

# 4. Converter idade para inteiro
df["idade"] = df["idade"].astype(int)

# 5. Persistência em SQLite
conn = sqlite3.connect(":memory:")
df.to_sql("usuarios", conn, index=False, if_exists="replace")

# 6. Validação da persistência
print(pd.read_sql_query("SELECT * FROM usuarios", conn))
