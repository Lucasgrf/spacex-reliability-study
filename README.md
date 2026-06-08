# SpaceX Reliability Study

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Lucasgrf/spacex-reliability-study/blob/main/notebooks/master_analysis.ipynb)

Estudo estatístico empírico sobre o impacto da reutilização de propulsores orbitais (boosters) na confiabilidade das missões aeroespaciais da SpaceX.

---

## 🎯 Pergunta de Pesquisa

> **A reutilização de foguetes afeta a confiabilidade das missões?**

Investigamos se a fadiga mecânica ou outros fatores de desgaste decorrentes de múltiplos lançamentos comprometem a segurança dos voos, ou se os processos de recondicionamento e seleção natural mitigam esses riscos.

---

## 👥 Equipe do Projeto

* **Lucas Gabriel Rocha Ferreira** — Engenharia de Dados (ETL)
* **Gabriel Matheus de Souza Garcia** — Analista (Tendências Temporais)
* **Kaio Ribeiro de Sousa** — Analista (Impacto da Reutilização)
* **Izac Regis de Souza** — Analista (Curva de Experiência e Desgaste)
* **Jônatas Pereira** — Analista (Famílias de Foguetes)
* **Guilherme Muller de Souza** — Pesquisa e Suporte

---

## 📊 Síntese dos Resultados

O estudo analisou **192 registros de boosters individuais por missão** extraídos da **SpaceX API v4**, cobrindo o período de 2006 a 2022. Os resultados das quatro vertentes analíticas são resumidos na tabela a seguir:

| Eixo Analítico | Principal Achado | Evidência Estatística | Fator Confundidor / Viés |
| :--- | :--- | :--- | :--- |
| **Temporal** (Gabriel) | O sucesso operacional evoluiu de 0% (2006) para 100% (2017–2022). | Estabilização em 100% de sucesso concomitante ao crescimento operacional (43 lançamentos em 2022). | **Maturação Operacional**: Evolução de processos de qualidade e design ao longo do tempo. |
| **Reutilização** (Kaio) | Foguetes reutilizados apresentaram desempenho superior aos novos. | **100% de sucesso** em boosters reutilizados (n=149) vs. **88,4%** em novos (n=43). | **Efeito Temporal**: Os boosters reutilizados só surgiram após a SpaceX atingir maturidade técnica (a partir de 2016). |
| **Experiência** (Izac) | A confiabilidade se comporta como uma função degrau, sem sinal de desgaste. | 88,4% de sucesso no 1º voo (n=43) e 100% estável de 2 a 13 voos (n=149). | **Viés de Sobrevivência/Seleção**: Boosters com defeitos latentes falham ou são descartados na estreia. |
| **Famílias** (Jonatas) | A baixa confiabilidade inicial está concentrada nas primeiras gerações de hardware. | **Falcon 1**: 40% (n=5) \| **Falcon 9**: 98,9% (n=178) \| **Falcon Heavy**: 100% (n=9). | **Descontinuidade de Hardware**: A transição para a família Falcon 9 sanou as falhas estruturais iniciais. |

---

## 🛠️ Tecnologias e Bibliotecas Utilizadas

* **Linguagem**: Python 3.10+
* **Engenharia de Dados (ETL)**: Pandas, Jupyter Notebook
* **Análise Estatística**: SciPy (cálculo de intervalos de confiança binomiais com Wilson score interval)
* **Visualização**: Matplotlib
* **Consumo de APIs**: Requests (para extração JSON da SpaceX API v4)

---

## 📁 Estrutura do Repositório

```text
spacex-reliability-study/
├── data/
│   ├── raw/             # Dados brutos em JSON direto da SpaceX API v4 (backup).
│   └── processed/       # Dataset limpo e modelado (processed_dataset_v1.csv).
├── docs/                # ROADMAP.md, project-plan.md e guias individuais.
├── analyses/            # Jupyter Notebooks exploratórios individuais dos integrantes.
├── notebooks/           # Scripts centrais: lucas_data_extraction.ipynb e master_analysis.ipynb.
├── graphs/              # Gráficos e visualizações exportadas (.png).
├── poster/              # Conteúdo textual estruturado para montagem do banner científico.
└── report/              # Documento conclusivo do relatório do grupo.
```

---

## 🚀 Como Executar o Projeto

### 1. Configurar o Ambiente Virtual
Crie e ative um ambiente virtual local:
```bash
python -m venv .venv
# No Windows (PowerShell):
.venv\Scripts\activate
# No Linux/macOS:
source .venv/bin/activate
```

### 2. Instalar Dependências
Instale as bibliotecas necessárias para rodar o pipeline e os gráficos:
```bash
pip install pandas numpy scipy matplotlib notebook requests
```

### 3. Executar o Pipeline de Extração e Limpeza (ETL)
Abra e execute todas as células do notebook de extração:
```bash
jupyter notebook notebooks/lucas_data_extraction.ipynb
```
Isso fará o download das informações da API da SpaceX, filtrará missões pendentes, estruturará os propulsores secundários do Falcon Heavy e salvará a base em `data/processed/processed_dataset_v1.csv`.

### 4. Executar a Análise Consolidada
Abra e execute o notebook mestre para ver a análise cruzada dos resultados e gerar os gráficos finais:
```bash
jupyter notebook notebooks/master_analysis.ipynb
```