# 📊 Instruções de Análise - Gabriel

## Seu Foco: Tendências Temporais

**Pesquisa**: Como a confiabilidade de missões SpaceX evoluiu ao longo do tempo?

---

## Dataset Pronto

✅ **Arquivo**: `data/processed/processed_dataset_v1.csv`  
✅ **Status**: Aprovado para análise  
✅ **Linhas**: 192 (100% completo)  
✅ **Período**: 2006-2022  

---

## Variáveis Disponíveis

| Coluna | Tipo | Descrição | Use? |
|--------|------|-----------|------|
| `launch_year` | int64 | Ano do lançamento | ✅ **PRIMARY** |
| `launch_date` | str | Data/hora ISO 8601 | ✅ Para análises mais precisas |
| `success` | bool | Sucesso da missão | ✅ **PRIMARY** |
| `rocket_name` | str | Tipo de foguete | ✅ Para comparar famílias |
| `reuse_count` | int64 | Lançamentos anteriores do booster | ℹ️ Controle/ajuste |
| `flight_number` | int64 | Número sequencial | ℹ️ Ordenação |
| `is_reused` | bool | Booster foi reutilizado? | ✅ Para segmentar análise |
| `launch_name` | str | Nome da missão | ℹ️ Identificação |
| `core_id` | str | ID do booster | ℹ️ Rastreamento |

---

## Questões de Pesquisa Recomendadas

### Q1: Evolução da Taxa de Sucesso
```
Como a taxa de sucesso mudou de 2006 a 2022?
```

**Análise Sugerida**:
```python
df.groupby('launch_year')['success'].agg(['count', 'sum', 'mean'])
# Resultado esperado: média aumentando ao longo dos anos
```

**Visualização**: Gráfico de linha: sucesso (%) vs. ano

---

### Q2: Mudança de Frequência de Lançamentos
```
SpaceX aumentou o ritmo de lançamentos ao longo do tempo?
```

**Análise Sugerida**:
```python
df.groupby('launch_year').size()
# ou
df['launch_year'].value_counts().sort_index()
```

**Visualização**: Gráfico de barras: quantidade de lançamentos por ano

---

### Q3: Correlação: Experiência da SpaceX vs Sucesso
```
Conforme a SpaceX ganhou experiência (flight_number aumentou),
os lançamentos ficaram mais confiáveis?
```

**Análise Sugerida**:
```python
df.groupby(pd.cut(df['flight_number'], bins=5))['success'].mean()
# Ou correlação direta
df['flight_number'].corr(df['success'].astype(int))
```

**Cuidado**: `flight_number` pode ser proxy para `launch_year`

---

### Q4: Mudança de Mix de Produtos
```
A distribuição entre Falcon 1, Falcon 9, Falcon Heavy mudou ao longo do tempo?
```

**Análise Sugerida**:
```python
pd.crosstab(df['launch_year'], df['rocket_name'], margins=True)
```

**Visualização**: Gráfico empilhado: rocket type por ano

---

## Avisos Importantes (Antes de Publicar Resultados)

⚠️ **1. Tamanho de Amostra Varia**
- 2006-2010: n=20 (muito pequeno, alta variância)
- 2015-2018: n=50-70 (bom tamanho)
- 2021-2022: n=37 (moderado)

Sempre reporte `n` e considere intervalos de confiança.

⚠️ **2. Possível Confusão com Rocket Type**
- Falcon 1 (2006-2009): Taxa de sucesso baixa (70%)
- Falcon 9 (2010+): Taxa de sucesso alta (97.7%)
- Falcon Heavy (2018+): Taxa de sucesso muito alta (100%)

A melhoria de 2006→2022 pode ser parcialmente devida a mudança de rocket type, não apenas aprendizado operacional.

**Solução**: Segmente por rocket_name e também agregado.

⚠️ **3. Early SpaceX Bias**
Os primeiros 5 lançamentos (2006-2009) foram da Falcon 1, que tinha design diferente.
Considere análises "com" e "sem" Falcon 1 para clareza.

---

## Estrutura Sugerida do Notebook

```markdown
# Análise: Tendências Temporais SpaceX

## 1. Setup
   - Load dataset
   - Check shape and types

## 2. Exploração Inicial
   - Year distribution
   - Success rate overall (97.40%)

## 3. Main Analysis
   ### 3.1 Success Rate Over Time (All Rockets)
   ### 3.2 Success Rate by Rocket Family + Year
   ### 3.3 Launch Frequency Over Time
   ### 3.4 Falcon 1 vs Falcon 9+ Trajectory

## 4. Statistical Tests
   - Trend test (mann-kendall or linear regression)
   - Test for break points (2010 = Falcon 9 intro)

## 5. Conclusions & Caveats
   - What improved?
   - What stayed same?
   - What might be confounded?
```

---

## Ferramentas Recomendadas

**Estatística**:
- `scipy.stats.mannkendalltrend` — Test for monotonic trend
- `numpy.polyfit` — Linear regression for trend line

**Visualização**:
- `matplotlib.pyplot` — Basic plots
- `seaborn` — Enhanced plots with confidence intervals

**Dados**:
- `pandas.groupby` — Aggregations
- `pandas.resample` — Time series (if using launch_date)

---

## Exemplos de Código

### Exemplo 1: Taxa de Sucesso por Ano
```python
import pandas as pd

df = pd.read_csv('data/processed/processed_dataset_v1.csv')

# Success rate by year
yearly_success = df.groupby('launch_year').agg({
    'success': ['count', 'sum', 'mean']
}).round(4)

yearly_success.columns = ['launches', 'successes', 'success_rate']
print(yearly_success)
```

### Exemplo 2: Comparação Falcon 1 vs Falcon 9+
```python
falcon1_success = df[df['rocket_name'] == 'Falcon 1']['success'].mean()
falcon_modern_success = df[df['rocket_name'] != 'Falcon 1']['success'].mean()

print(f"Falcon 1: {falcon1_success*100:.2f}%")
print(f"Falcon 9+: {falcon_modern_success*100:.2f}%")
print(f"Difference: {(falcon_modern_success - falcon1_success)*100:.2f} pp")
```

---

## Deliverables

Quando terminar sua análise, compartilhe:
1. **Notebook** com código reproduzível
2. **Gráficos** em alta resolução (PNG/PDF)
3. **Tabela de Resultados** em CSV ou markdown
4. **Conclusões** em 1-2 parágrafos

---

## Contato

Dúvidas sobre o dataset? Veja:
- `docs/data_dictionary.md` — Definição de campos
- `docs/data_quality_report.md` — QA details
- `notebooks/lucas_data_extraction.ipynb` — ETL source code

**Data Manager**: Lucas

---

**Boa sorte!** 🚀
