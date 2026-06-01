# 📊 Instruções de Análise - Kaio

## Seu Foco: Impacto da Reutilização na Confiabilidade

**Pesquisa**: A reutilização de boosters afeta a confiabilidade de missões SpaceX?

---

## ⚠️ AVISO CRÍTICO

Você tem os resultados mais intrigantes:

| Tipo | N | Taxa Sucesso |
|------|---|--------------|
| Virgem | 43 | 88.37% |
| Reutilizado | 149 | 100.00% |

**Mas NÃO é conclusão ainda.** Precisa validar:
- ✅ Tamanho de amostra (149 > 30, OK)
- ❌ Intervalo de confiança (ainda não calculado)
- ❌ Teste estatístico de significância (ainda não feito)
- ❌ Possível viés temporal (boosters reutilizados são mais recentes?)

---

## Dataset Pronto

✅ **Arquivo**: `data/processed/processed_dataset_v1.csv`  
✅ **Status**: Aprovado para análise  
✅ **Linhas**: 192 (100% completo)  
✅ **Boosters virgens**: 43  
✅ **Boosters reutilizados**: 149  

---

## Variáveis Disponíveis

| Coluna | Tipo | Descrição | Use? |
|--------|------|-----------|------|
| `is_reused` | bool | Booster foi reutilizado? | ✅ **PRIMARY** |
| `reuse_count` | int64 | Quantas vezes? (0, 1, 2, ..., 13) | ✅ Para análise mais fina |
| `success` | bool | Sucesso da missão | ✅ **PRIMARY** |
| `launch_year` | int64 | Ano (CUIDADO: possível confusão) | ⚠️ Variável de controle |
| `rocket_name` | str | Tipo de foguete | ✅ Para estratificar análise |
| `launch_date` | str | Data ISO 8601 | ⚠️ Para detectar viés temporal |
| `flight_number` | int64 | Número sequencial | ℹ️ Para ordenação |
| `launch_name` | str | Nome da missão | ℹ️ Identificação |
| `core_id` | str | ID do booster | ℹ️ Rastreamento |

---

## Questões de Pesquisa Recomendadas

### Q1: Diferença Básica (Com Cuidado!)
```
Qual é a taxa de sucesso de boosters virgens vs reutilizados?
```

**Análise Básica**:
```python
df.groupby('is_reused')['success'].agg(['count', 'sum', 'mean'])
# Resultado esperado:
# False: n=43, sucesso=38, taxa=88.37%
# True:  n=149, sucesso=149, taxa=100.00%
```

**⚠️ CUIDADO**: O resultado é intrigante, mas pode haver confusão.

---

### Q2: Controlando por Rocket Type
```
A vantagem de reutilização existe em cada tipo de foguete?
```

**Análise com Estratificação**:
```python
for rocket in df['rocket_name'].unique():
    subset = df[df['rocket_name'] == rocket]
    print(f"\n{rocket}:")
    print(subset.groupby('is_reused')['success'].agg(['count', 'mean']))
```

**Por quê**: Alguns rockets podem ser mais novos/complexos = viés natural.

---

### Q3: Controlando por Ano (Temporal Bias)
```
Será que boosters reutilizados foram usados em anos recentes,
quando SpaceX era mais competente?
```

**Análise com Controle Temporal**:
```python
# Quando os boosters reutilizados foram lançados?
reused_years = df[df['is_reused'] == True]['launch_year'].value_counts()
virgin_years = df[df['is_reused'] == False]['launch_year'].value_counts()

print("Virgin boosters year distribution:")
print(virgin_years)
print("\nReused boosters year distribution:")
print(reused_years)
```

**Se resultado mostrar**:
- Virgens: principalmente 2006-2015
- Reutilizados: principalmente 2015-2022

Então há confusão! Boosters reutilizados não são melhores, SpaceX é melhor em 2022.

---

### Q4: Análise de Dose-Resposta
```
Quanto mais reutilizado, melhor o desempenho?
```

**Análise por Número de Reutilizações**:
```python
df.groupby('reuse_count')['success'].agg(['count', 'mean']).head(15)
```

**Possíveis padrões**:
- ✅ Correlação positiva: reuse_count ↑ → success ↑
- ⚠️ Correlação nula: reuse_count não importa
- ❌ Correlação negativa: boosters desgastam

---

## Testes Estatísticos Necessários

### Teste 1: Chi-Square (Independência)
```python
from scipy.stats import chi2_contingency

contingency = pd.crosstab(df['is_reused'], df['success'])
chi2, pval, dof, expected = chi2_contingency(contingency)

print(f"Chi-square: {chi2:.4f}")
print(f"P-value: {pval:.4f}")
print(f"Significant? {pval < 0.05}")
```

**Interpreta**:
- p < 0.05: Diferença é estatisticamente significante
- p ≥ 0.05: Diferença pode ser acaso (tamanho pequeno)

### Teste 2: Intervalo de Confiança (Binomial)
```python
from scipy.stats import binom

# Bootstrapping or Wilson score interval
# Para n=149, sucesso=149: IC de 100%?
# Para n=43, sucesso=38: IC de 88.37%?

# Simples: usar statsmodels
from statsmodels.stats.proportion import proportion_confint

virgin_success = df[df['is_reused'] == False]['success'].sum()
virgin_total = len(df[df['is_reused'] == False])
lower, upper = proportion_confint(virgin_success, virgin_total, alpha=0.05)
print(f"Virgin IC95%: [{lower*100:.2f}%, {upper*100:.2f}%]")

reused_success = df[df['is_reused'] == True]['success'].sum()
reused_total = len(df[df['is_reused'] == True])
lower, upper = proportion_confint(reused_success, reused_total, alpha=0.05)
print(f"Reused IC95%: [{lower*100:.2f}%, {upper*100:.2f}%]")
```

---

## Avisos Críticos

⚠️ **1. Possível Causa de Confusão: Tempo**

**Hipótese Nula Alternativa**:
"Boosters reutilizados parecem melhores porque foram lançados em anos recentes (quando SpaceX aprendeu), não porque reutilização é intrinsecamente melhor."

**Como testar**:
```python
# Estratificado por ano
for year in sorted(df['launch_year'].unique()):
    subset = df[df['launch_year'] == year]
    if len(subset) > 5:  # Só se houver amostra
        result = subset.groupby('is_reused')['success'].mean()
        print(f"{year}: Virgin={result.get(False, np.nan):.2f}, Reused={result.get(True, np.nan):.2f}")
```

⚠️ **2. Possível Causa de Confusão: Rocket Type**

Falcon Heavy (mais novo, mais complexo?) pode ter outros padrões.

**Como testar**: Estratifique também por rocket_name.

⚠️ **3. Tamanho de Amostra Desbalanceado**

- Virgens: n=43
- Reutilizados: n=149

Razão 1:3.5. Isso é OK para chi-square, mas note.

⚠️ **4. Taxa Perfeita de 100%**

"Boosters reutilizados: 100% de sucesso" é suspeito.

**Possibilidades**:
1. Reutilização realmente melhora confiabilidade ✅
2. Boosters defeituosos nunca são reutilizados (selection bias) ⚠️
3. Boosters reutilizados são mais novos/modernos ⚠️
4. SpaceX é mais cuidadoso ao reutilizar (selection bias) ⚠️

---

## Estrutura Sugerida do Notebook

```markdown
# Análise: Reutilização e Confiabilidade

## 1. Setup
   - Load dataset
   - Check n for virgens/reutilizados

## 2. Exploração Inicial
   - Tabela: is_reused × success
   - Distribuição de launch_year por is_reused

## 3. Main Analysis
   ### 3.1 Taxa de Sucesso: Virgens vs Reutilizados
   ### 3.2 Teste Chi-Square
   ### 3.3 Intervalos de Confiança
   ### 3.4 Análise Estratificada por Ano
   ### 3.5 Análise Estratificada por Rocket Type
   ### 3.6 Dose-Resposta (reuse_count)

## 4. Investigação de Possíveis Confusões
   ### 4.1 Viés Temporal
   ### 4.2 Viés de Seleção (selection bias)
   ### 4.3 Variações por Rocket

## 5. Conclusões
   - What's the actual effect?
   - What could be confounders?
   - Need for follow-up?
```

---

## Exemplos de Código

### Exemplo 1: Tabela Cruzada
```python
import pandas as pd

df = pd.read_csv('data/processed/processed_dataset_v1.csv')

# Tabela cruzada
crosstab = pd.crosstab(df['is_reused'], df['success'], margins=True)
print(crosstab)

# Com percentuais
crosstab_pct = pd.crosstab(df['is_reused'], df['success'], normalize='index') * 100
print(crosstab_pct.round(2))
```

### Exemplo 2: Teste Chi-Square
```python
from scipy.stats import chi2_contingency

contingency = pd.crosstab(df['is_reused'], df['success'])
chi2, pval, dof, expected = chi2_contingency(contingency)

print(f"Chi-square: {chi2:.4f}")
print(f"P-value: {pval:.6f}")
print(f"Statistically significant? {pval < 0.05}")
```

### Exemplo 3: Controle por Ano
```python
# Sucesso por is_reused, para cada ano
for year in sorted(df['launch_year'].unique())[-10:]:
    subset = df[df['launch_year'] == year]
    result = subset.groupby('is_reused')['success'].agg(['count', 'sum', 'mean'])
    print(f"\n{year}:")
    print(result)
```

---

## Deliverables

Quando terminar, compartilhe:
1. **Notebook** com análise completa e cuidadosa
2. **Tabelas** de resultados (estratificado)
3. **Gráficos** mostrando:
   - Taxa de sucesso: virgens vs reutilizados
   - Distribuição temporal
   - Dose-resposta
4. **Conclusões** (1-2 parágrafos):
   - O efeito é real?
   - Qual é o tamanho?
   - Há confusões pendentes?

---

## Contato

Dúvidas? Veja:
- `docs/data_dictionary.md` — Definição de campos
- `docs/data_quality_report.md` — QA details
- `docs/MILESTONE_v1_COMPLETE.md` — Contexto do projeto

**Data Manager**: Lucas

---

**Boa análise!** 🔬
