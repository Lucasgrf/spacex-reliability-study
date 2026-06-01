# 📊 Instruções de Análise - Jonatas

## Seu Foco: Comparação de Famílias de Foguetes

**Pesquisa**: Qual família de foguete (Falcon 1, 9, Heavy) tem a maior confiabilidade?

---

## Dataset Pronto

✅ **Arquivo**: `data/processed/processed_dataset_v1.csv`  
✅ **Status**: Aprovado para análise  
✅ **Linhas**: 192 (100% completo)  
✅ **Famílias**: 3 (Falcon 1, Falcon 9, Falcon Heavy)  

---

## Variáveis Disponíveis

| Coluna | Tipo | Descrição | Use? |
|--------|------|-----------|------|
| `rocket_name` | object | Tipo de foguete | ✅ **PRIMARY** |
| `success` | bool | Sucesso da missão | ✅ **PRIMARY** |
| `launch_year` | int64 | Ano do lançamento | ✅ Para análise temporal |
| `is_reused` | bool | Booster reutilizado? | ✅ Para comparar efeitos |
| `reuse_count` | int64 | Vezes reutilizado | ✅ Para controle |
| `flight_number` | int64 | Número sequencial | ℹ️ Ordem |
| `launch_date` | str | Data ISO 8601 | ℹ️ Timeline |
| `launch_name` | str | Nome da missão | ℹ️ Identificação |
| `core_id` | str | ID do booster | ℹ️ Rastreamento |

---

## Características Esperadas das Famílias

### Falcon 1
- **Período**: 2006-2009 (5 missões)
- **Status**: Descontinuado
- **Características**: Design experimental, motor Merlin original, cargas científicas pequenas
- **Expectativa**: Taxa de sucesso baixa (4/5 = 80%?)

### Falcon 9
- **Período**: 2010-2022 (168 lançamentos)
- **Status**: Operacional, principal veículo SpaceX
- **Características**: Motor Merlin aprimorado, propulsão em clusters, diversos payloads
- **Expectativa**: Taxa de sucesso média-alta (170+/172)

### Falcon Heavy
- **Período**: 2018-2022 (5 lançamentos)
- **Status**: Operacional, heavylift especializado
- **Características**: Três cores Falcon 9 em cluster, tecnologia mais recente
- **Expectativa**: Taxa de sucesso alta (devido idade/maturidade)

---

## Questões de Pesquisa Recomendadas

### Q1: Confiabilidade Bruta por Família
```
Qual é a taxa de sucesso de cada família de foguete?
```

**Análise Básica**:
```python
df.groupby('rocket_name')['success'].agg(['count', 'sum', 'mean']).round(4)

# Esperado (aproximado):
#              count  sum  mean
# Falcon 1         5    4  0.80
# Falcon 9       172  169  0.98
# Falcon Heavy    15   15  1.00
```

**Visualização**: Gráfico de barras com taxa de sucesso por rocket

---

### Q2: Evolução Temporal de Cada Família
```
Como a confiabilidade de cada família evoluiu ao longo do tempo?
```

**Análise com Estratificação**:
```python
for rocket in sorted(df['rocket_name'].unique()):
    subset = df[df['rocket_name'] == rocket]
    yearly = subset.groupby('launch_year')['success'].agg(['count', 'mean'])
    print(f"\n{rocket}:")
    print(yearly)
```

**Por quê**: Falcon 1 foi em 2006-2009 (tecnologia antiga vs Falcon Heavy 2018+).

---

### Q3: Controle por Reusabilidade
```
Removendo o efeito de reutilização, qual família é melhor?
```

**Análise Estratificada por is_reused**:
```python
for is_reused_val in [False, True]:
    print(f"\n{'Virgin' if not is_reused_val else 'Reused'} boosters:")
    subset = df[df['is_reused'] == is_reused_val]
    result = subset.groupby('rocket_name')['success'].agg(['count', 'mean'])
    print(result)
```

**Interpretação**:
- Se Falcon 9 melhora dramaticamente com reutilização, efeito real
- Se Falcon Heavy é alto em ambas, design intrinsecamente melhor

---

### Q4: Teste Chi-Square (Independência)
```
A taxa de sucesso DEPENDE do tipo de foguete,
ou são independentes?
```

**Teste Estatístico**:
```python
from scipy.stats import chi2_contingency

contingency = pd.crosstab(df['rocket_name'], df['success'])
chi2, pval, dof, expected = chi2_contingency(contingency)

print(f"Chi-square: {chi2:.4f}")
print(f"P-value: {pval:.6f}")
print(f"Significant? {pval < 0.05}")
```

**Interpreta**:
- p < 0.05: Diferenças são significantes (não apenas acaso)
- p ≥ 0.05: Diferenças podem ser acaso

---

### Q5: Análise de Risco Relativo
```
Qual é o risco de falha para cada família?
```

**Cálculo de Risco Relativo**:
```python
# Risk = 1 - success_rate

for rocket in sorted(df['rocket_name'].unique()):
    subset = df[df['rocket_name'] == rocket]
    success_rate = subset['success'].mean()
    risk = 1 - success_rate
    n = len(subset)
    print(f"{rocket}: Success={success_rate*100:.2f}%, Risk={risk*100:.2f}% (n={n})")
```

**Ou comparativo**:
```python
# Risco Relativo: Falcon 1 vs Falcon 9
falcon1_risk = 1 - df[df['rocket_name'] == 'Falcon 1']['success'].mean()
falcon9_risk = 1 - df[df['rocket_name'] == 'Falcon 9']['success'].mean()

rr = falcon1_risk / falcon9_risk
print(f"Falcon 1 tem {rr:.1f}x o risco do Falcon 9")
```

---

### Q6: Análise de Payload/Missão
```
Diferentes tipos de missão têm diferentes taxas de sucesso?
```

**Análise por Família + Nome da Missão** (se houver padrão):
```python
# Verificar nomes comuns
df[df['rocket_name'] == 'Falcon 9']['launch_name'].value_counts().head(10)

# Há padrão de missão? CRS? Starlink? Comercial?
# Isso pode confundir a análise.
```

---

## Cuidados Críticos

⚠️ **1. Tamanho de Amostra Muito Desigual**

- Falcon 1: n=5 (muito pequeno!)
- Falcon 9: n=172 (grande)
- Falcon Heavy: n=15 (moderado)

**Impacto**: Comparar Falcon 1 vs Falcon 9 é injusto (n tão diferente).

**Solução**: Use intervalos de confiança, não apenas taxas.

⚠️ **2. Confusão Temporal**

Falcon 1 foi 2006-2009 (tecnologia velha). Falcon Heavy é 2018-2022 (tecnologia nova).
Melhoria pode ser apenas pelo tempo, não pela qualidade do design.

**Solução**: Estratifique por ano ao comparar.

⚠️ **3. Possível Survivorship Bias**

Falcon 1 foi descontinuado (supostamente por baixa performance).
Falcon 9 continua operacional (porque funciona).

Isso não é viés no dataset, é realidade! Mas note na interpretação.

⚠️ **4. Número Pequeno de Falhas**

Total de falhas no dataset: 5 (apenas 2.6%).
Falcon Heavy: 0 falhas em 15 lançamentos (100%).

Com tão poucas falhas, poder estatístico é limitado.

---

## Testes Estatísticos

### Teste 1: Chi-Square
```python
from scipy.stats import chi2_contingency

contingency = pd.crosstab(df['rocket_name'], df['success'])
chi2, pval, dof, expected = chi2_contingency(contingency)
print(f"Chi-square={chi2:.4f}, p={pval:.6f}")
```

### Teste 2: Intervalos de Confiança (Binomial)
```python
from statsmodels.stats.proportion import proportion_confint

for rocket in sorted(df['rocket_name'].unique()):
    subset = df[df['rocket_name'] == rocket]
    n_success = subset['success'].sum()
    n_total = len(subset)
    
    lower, upper = proportion_confint(n_success, n_total, alpha=0.05)
    
    print(f"{rocket}:")
    print(f"  Success: {n_success}/{n_total} = {n_success/n_total*100:.2f}%")
    print(f"  IC95%: [{lower*100:.2f}%, {upper*100:.2f}%]")
```

### Teste 3: Fisher Exact Test (Alternativa ao Chi-Square)
```python
from scipy.stats import fisher_exact

# Para comparar 2 rockets por vez (Fisher é para tabelas 2x2)
falcon1_success = df[df['rocket_name'] == 'Falcon 1']['success'].sum()
falcon1_total = len(df[df['rocket_name'] == 'Falcon 1'])
falcon1_fail = falcon1_total - falcon1_success

falcon9_success = df[df['rocket_name'] == 'Falcon 9']['success'].sum()
falcon9_total = len(df[df['rocket_name'] == 'Falcon 9'])
falcon9_fail = falcon9_total - falcon9_success

# Tabela 2x2
table = [[falcon1_success, falcon1_fail],
         [falcon9_success, falcon9_fail]]

odds_ratio, pval = fisher_exact(table)
print(f"Odds Ratio: {odds_ratio:.4f}, p={pval:.6f}")
```

---

## Estrutura Sugerida do Notebook

```markdown
# Análise: Confiabilidade por Família de Foguete

## 1. Setup
   - Load dataset
   - Check distribution by rocket_name

## 2. Exploração Inicial
   - Tabela: rocket_name × success
   - Distribuição: n por rocket
   - Taxa bruta por rocket

## 3. Main Analysis
   ### 3.1 Confiabilidade Bruta
   ### 3.2 Evolução Temporal
   ### 3.3 Intervalos de Confiança
   ### 3.4 Efeito de Reutilização (Controle)
   ### 3.5 Análise de Risco Relativo

## 4. Testes Estatísticos
   - Chi-Square
   - Fisher Exact (pairwise)
   - Intervalos de Confiança

## 5. Contexto Histórico
   - Quando cada rocket foi usado?
   - Por que Falcon 1 foi descontinuado?
   - Maturidade relativa de cada design?

## 6. Conclusões
   - Ranking de confiabilidade
   - Diferenças significantes?
   - Possíveis explicações
```

---

## Exemplos de Código

### Exemplo 1: Taxa por Rocket
```python
import pandas as pd

df = pd.read_csv('data/processed/processed_dataset_v1.csv')

# Taxa de sucesso por rocket
rocket_stats = df.groupby('rocket_name')['success'].agg([
    'count',  # n
    'sum',    # sucessos
    'mean'    # taxa
]).round(4)

rocket_stats.columns = ['Total', 'Sucessos', 'Taxa Sucesso']
print(rocket_stats)
```

### Exemplo 2: Intervalos de Confiança
```python
from statsmodels.stats.proportion import proportion_confint

print("Intervalos de Confiança 95%:")
for rocket in sorted(df['rocket_name'].unique()):
    subset = df[df['rocket_name'] == rocket]
    n_success = subset['success'].sum()
    n_total = len(subset)
    
    lower, upper = proportion_confint(n_success, n_total, alpha=0.05)
    
    print(f"\n{rocket} (n={n_total}):")
    print(f"  Taxa: {n_success/n_total*100:.2f}%")
    print(f"  IC95%: [{lower*100:.2f}%, {upper*100:.2f}%]")
```

### Exemplo 3: Chi-Square
```python
from scipy.stats import chi2_contingency

contingency = pd.crosstab(df['rocket_name'], df['success'])
chi2, pval, dof, expected = chi2_contingency(contingency)

print(f"Chi-square Test:")
print(f"  χ² = {chi2:.4f}")
print(f"  p-value = {pval:.6f}")
print(f"  Significant? {pval < 0.05}")
```

---

## Deliverables

Quando terminar:
1. **Notebook** com análise completa
2. **Tabelas**:
   - Sucesso bruto por rocket
   - Intervalos de confiança
   - Teste chi-square
3. **Gráficos**:
   - Barras: taxa de sucesso por rocket
   - Linha: evolução temporal de cada rocket
   - Box plot: distribuição
4. **Conclusões** (1-2 parágrafos):
   - Qual rocket é mais confiável?
   - As diferenças são significantes?
   - Como interpretar considerando tamanho de amostra?

---

## Contato

Dúvidas? Veja:
- `docs/data_dictionary.md` — Campos
- `docs/data_quality_report.md` — QA
- `docs/MILESTONE_v1_COMPLETE.md` — Contexto

**Data Manager**: Lucas

---

**Boa análise!** 🚀
