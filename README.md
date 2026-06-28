# `gs-neuromorphic-cluster: NeuroSpace Alert`

> Redes neurais Leaky Integrate-and-Fire (LIF) com memristores sinteticos para deteccao de anomalias em telemetria de rovers lunares. Processamento de eventos esparsos com consumo energetico sub-mW. GS 2026.1, FIAP.

---

## `Tecnologias`

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![NumPy](https://img.shields.io/badge/NumPy-neuromorfico-013243)
![Pandas](https://img.shields.io/badge/pandas-telemetria-green)
![Plotly](https://img.shields.io/badge/Plotly-visualizacao-purple)
![Colab](https://img.shields.io/badge/Google%20Colab-notebook-F9AB00)
![License](https://img.shields.io/badge/license-MIT-green)

---

## `O que faz`

Simula um sensor neuromórfico de baixo consumo para monitoramento de telemetria de rover lunar:

- **Modelo LIF**: neuronios Leaky Integrate-and-Fire com memristores sinteticos como sinapses adaptativos
- **Codificacao esparsa**: converte leituras de sensores em trens de spikes temporais (rate-coding)
- **Deteccao de anomalias**: classifica estados NOMINAL/ATENCAO/CRITICO em telemetria multimodal
- **Auditoria de dados**: `data_lineage_audit.json` rastreia toda transformacao na pipeline
- **Model registry**: `model_registry.json` versiona snapshots do modelo LIF

---

## `Notebooks`

| Arquivo | Descricao |
|---------|---------|
| `Colab_NeuroSpace_Alert.ipynb` | Rede LIF completa com 50 neuronios e 200 passos temporais |
| `GS_1º_(SEM)_Cluster_Computing,...ipynb` | Cluster computing e supercomputadores (notebook expandido) |
| `telemetria_processada_Ajuste_B_equilibrado.csv` | Dataset de telemetria lunar processado |

---

## `Arquitetura LIF`

```
Sensores (tensao, corrente, temperatura, vibracao)
    |
    Codificacao rate-coding: leitura → frequencia de spikes
    |
    Camada LIF (50 neuronios)
        V(t+1) = V(t) * decay + I(t) * w - V_reset * spike(t)
        spike(t): V(t) > threshold → emite spike, reseta V
    |
    Taxa de spikes → limiar de classificacao
    |
    Saida: NOMINAL · ATENCAO · CRITICO
```

---

## `Auditoria`

```json
{
  "pipeline_id": "neuro_pipeline_v1",
  "transformations": [
    {"step": "raw_telemetry", "records": 200},
    {"step": "normalized", "scaling": "MinMaxScaler"},
    {"step": "spike_encoded", "method": "rate_coding"}
  ]
}
```

---

## `Como executar`

```python
# Abra Colab_NeuroSpace_Alert.ipynb no Google Colab
# Runtime > Run all
# Dependencias: apenas NumPy, Pandas, Plotly
```

---

## `Licenca`

Distribuido sob a licenca MIT.

---

## `Autor`

**Arthur Baptista dos Santos**
RM 565346 · Inteligencia Artificial · FIAP 2025-2026

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Arthur%20Baptista-0077B5?logo=linkedin)](https://linkedin.com/in/arthur-baptista-dos-santos)
[![GitHub](https://img.shields.io/badge/GitHub-Arthur--Baptista--dos--Santos-181717?logo=github)](https://github.com/Arthur-Baptista-dos-Santos)
