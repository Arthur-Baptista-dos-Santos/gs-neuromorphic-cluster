# 🛰️ NeuroSpace Alert

> **Sensor Neuromórfico de Baixo Consumo para Ambientes Espaciais**  
> Global Solution 2026 · FIAP · Cluster Computing, Computação Neuromórfica e Supercomputadores

---

## 📋 Descrição do Projeto

O **NeuroSpace Alert** propõe e simula um sensor neuromórfico embarcado em um **Rover Lunar**, capaz de detectar condições críticas de operação (temperatura elevada, radiação ionizante e acúmulo de poeira) sem transmitir continuamente todos os dados brutos à Terra.

O sensor implementa um modelo **LIF (Leaky Integrate-and-Fire)** com **memristor virtual**, inspirado em Strukov et al. (2008). Em vez de 61 transmissões contínuas (polling), o sistema emite apenas **3 tipos de eventos discretos** — redução de ~95% no volume de dados transmitidos.

---

## 🗂️ Estrutura do Repositório

```
neurospace-alert/
├── Colab_NeuroSpace_Alert.ipynb   # Notebook principal — simulador completo
├── docs/
│   └── Relatorio_NeuroSpace_Alert.docx  # Relatório técnico (4 páginas)
├── output/
│   ├── telemetria_processada_Ajuste_B_equilibrado.csv  # Saída Gold Layer
│   ├── model_registry.json                             # MLOps Model Registry
│   └── data_lineage_audit.json                         # Audit trail LGPD
└── README.md
```

> **Nota:** O dataset `dataset_neurosensor_espacial_5h.csv` deve ser carregado manualmente no Google Colab na Célula 1 (upload interativo).

---

## 🚀 Como Executar

1. Acesse [Google Colab](https://colab.research.google.com)
2. Faça upload do notebook `Colab_NeuroSpace_Alert.ipynb`
3. Execute a **Célula 1** e faça upload do arquivo `dataset_neurosensor_espacial_5h.csv` quando solicitado
4. Execute todas as células em ordem

---

## ⚙️ Ajustes do Sensor Testados

| Parâmetro | Ajuste A — Sensível | Ajuste B — Equilibrado | Ajuste C — Conservador |
|-----------|---------------------|------------------------|------------------------|
| `V_limiar` | 24 V | 28 V | 32 V |
| `taxa_chaveamento` | 0,007 | 0,005 | 0,003 |
| LED Amarelo | t = 170 min | t = 205 min | t = 255 min |
| LED Vermelho | t = 185 min | t = 225 min | t = 295 min |
| Lag vs. crítica real | −15 min (falso alerta) | +25 min (admissível) | +95 min (tardio) |

### ✅ Ajuste Recomendado: **Ajuste B — Equilibrado**

- `V_limiar = 28 V` · `taxa_chaveamento = 0.005`
- **Zero falso-positivos** durante a fase de ATENÇÃO
- Lag de +25 min dentro da tolerância dos materiais espaciais
- Estabilidade ALTA · Recall ALTO

---

## 🧠 Arquitetura Técnica

```
Sensores Físicos (Temp · Rad · Poeira)
        │
        ▼
  [Normalização Vetorial — NumPy SIMD]
        │
        ▼
  [Índice de Risco Ponderado → V_entrada]
  V = 15 + 25 × (0.45·temp + 0.35·rad + 0.20·poeira)
        │
        ▼
  [Integração Memristiva LIF — Strukov-Williams 2008]
  w += taxa × (V - V_limiar) × dt × (1 - w)   [se V > V_limiar]
  w -= λ × dt × w                               [decaimento Leaky]
        │
        ▼
  [Threshold Logic]
  w ≥ 0.40 → OBSERVACAO (LED Amarelo)
  w ≥ 0.65 → ALERTA_CRITICO (LED Vermelho)
        │
        ▼
  [Evento Discreto — Event-Driven Output]
  NORMAL · OBSERVACAO · ALERTA_CRITICO
```

---

## 📊 Pipeline de Dados (Arquitetura Medallion)

| Camada | Descrição |
|--------|-----------|
| **RAW** | CSV bruto do dataset de missão |
| **Bronze** | Validação Pydantic + Data Contract |
| **Silver** | Normalização vetorial + índice de risco |
| **Gold** | Telemetria enriquecida (V_entrada, estado_memristor, LED, diagnóstico) |

---

## 🏗️ Tecnologias Utilizadas

| Tecnologia | Uso |
|------------|-----|
| Python 3.10+ | Linguagem principal |
| Pandas / NumPy | Manipulação e normalização de dados |
| Pydantic | Validação de schema (Data Contract) |
| Loguru | Observabilidade enterprise |
| Rich | Saída formatada (tabelas, painéis) |
| concurrent.futures | Paralelismo (ThreadPoolExecutor) |
| scikit-learn | Benchmarking ML (Random Forest, Gradient Boosting) |
| SHAP | Explainability (TreeSHAP) |
| scipy | Detecção de drift (KS test) |

---

## 🎯 Conexão com ODS 9

**ODS 9 — Indústria, Inovação e Infraestrutura (ONU, Agenda 2030)**

- **Autonomia operacional:** sensor funciona sem uplink permanente
- **Eficiência energética:** redução de ~95% no consumo de RF
- **Infraestrutura resiliente:** operação em caso de falha do link Terra-Lua
- **Transferência tecnológica:** arquitetura aplicável a estações remotas de monitoramento ambiental no Brasil

---

## 📚 Referências

- STRUKOV, D. B. et al. *The missing memristor found*. Nature, v. 453, p. 80–83, 2008.
- MEAD, C. *Neuromorphic electronic systems*. Proceedings of the IEEE, v. 78, n. 10, 1990.
- DAVIES, M. et al. *Loihi: A Neuromorphic Manycore Processor with On-Chip Learning*. IEEE Micro, v. 38, n. 1, 2018.
- ONU. *Agenda 2030 — ODS 9: Indústria, Inovação e Infraestrutura*. 2015.

---

## 👤 Integrantes

| Nome | RM |
|------|----|
| Arthur Belo Santos | — |

---

*FIAP — Global Solution 2026 · 1º Semestre*
