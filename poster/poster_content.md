# Conteúdo do Banner/Pôster Científico

> **Instruções**: Copie e cole os textos abaixo nas seções correspondentes do template no Canva. Os gráficos sugeridos estão na pasta `graphs/`.

---

## Título

**Evidência Estatística de Confiabilidade em Sistemas de Foguetes Reutilizáveis: Um Estudo com Dados da SpaceX**

**Autores**: Lucas Ferreira, Gabriel Garcia, Kaio Ribeiro, Izac Regis, Jonatas Pereira

**Instituição**: Jala University

---

## Introdução

A reutilização de foguetes é uma das inovações mais disruptivas da indústria aeroespacial moderna. A SpaceX, pioneira nessa abordagem, realiza rotineiramente o reuso de seus propulsores orbitais. Este estudo investiga se a reutilização de boosters afeta a confiabilidade das missões, analisando dados históricos de lançamentos da SpaceX entre 2006 e 2022.

**Pergunta de Pesquisa**: A reutilização de foguetes afeta a confiabilidade das missões?

**Hipótese**: Propulsores reutilizáveis mantêm ou melhoram a confiabilidade ao longo de múltiplos lançamentos.

---

## Fonte de Dados

- **API**: SpaceX API v4 (endpoints: launches, rockets, cores)
- **Dataset**: 192 registros | 9 variáveis | Período: 2006–2022
- **Unidade de observação**: Booster (propulsor) por missão
- **Qualidade**: 100% completo (0 valores nulos, 0 duplicatas)

---

## Metodologia

1. **Coleta**: Extração automatizada via pipeline ETL (Python + Pandas)
2. **Limpeza**: Remoção de 23 missões não executadas; validação de schema
3. **Análise**: Estatística descritiva, intervalos de confiança (IC 95%), tabulação cruzada, curva dose-resposta
4. **Controles**: Análise temporal ano a ano para identificação de confundidores

---

## Resultados Principais

### 1. Evolução Temporal

A taxa de sucesso evoluiu de **0% (2006)** para **100% sustentado (2017-2022)**, com crescimento de 1 para 43 lançamentos/ano.

> 📊 **Gráfico sugerido**: `gabriel_success_rate_per_year.png`

### 2. Reutilização vs Confiabilidade

Boosters reutilizados: **100% de sucesso** (149/149). Boosters virgens: **88,4%** (38/43). Diferença: **+11,6 pontos percentuais**.

> 📊 **Gráfico sugerido**: `kaio_overall_success_rate.png`

### 3. Experiência do Booster

Após o primeiro voo bem-sucedido, boosters mantêm **100% de sucesso** independentemente do número de reutilizações (até 13 voos). Sem evidência de desgaste.

> 📊 **Gráfico sugerido**: `izac_experience_vs_success.png`

### 4. Famílias de Foguetes

Falcon 1: **40%** (n=5) | Falcon 9: **98,9%** (n=178) | Falcon Heavy: **100%** (n=9). Evolução geracional clara na confiabilidade.

> 📊 **Gráfico sugerido**: `jonatas_taxa_sucesso_familia.png`

---

## Conclusão

Os dados indicam que a **reutilização de foguetes não degrada a confiabilidade das missões**. Boosters reutilizados apresentaram taxa de sucesso superior (100%) em comparação com virgens (88,4%), sem evidência de efeito de desgaste ao longo de até 13 reutilizações. Contudo, os resultados devem ser interpretados com cautela devido ao **viés temporal** (reutilização coincide com maturidade operacional) e ao **viés de seleção** (boosters defeituosos não são reutilizados).

---

## Referências

- SpaceX API v4. Disponível em: https://github.com/r-spacex/SpaceX-API
- Python Software Foundation. Python 3. https://www.python.org
- McKinney, W. pandas: Python Data Analysis Library. https://pandas.pydata.org
- Hunter, J. D. Matplotlib: A 2D Graphics Environment. https://matplotlib.org
