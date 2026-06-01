# 📊 Instruções de Análise - Izac

## Seu Foco: Experiência de Booster e Confiabilidade

**Pesquisa**: Existe correlação entre experiência de um booster (reuse_count) e taxa de sucesso?

---

## Dataset Pronto

✅ **Arquivo**: `data/processed/processed_dataset_v1.csv`  
✅ **Status**: Aprovado para análise  
✅ **Linhas**: 192 (100% completo)  
✅ **Variável-chave**: `reuse_count` (0 a 13)  

---

## Variáveis Disponíveis

| Coluna | Tipo | Descrição | Use? |
|--------|------|-----------|------|
| `reuse_count` | int64 | Lançamentos anteriores (0-13) | ✅ **PRIMARY** |
| `success` | bool | Sucesso da missão | ✅ **PRIMARY** |
| `is_reused` | bool | Booster reutilizado? (simplificado) | ✅ Para comparar virgens vs experientes |
| `launch_year` | int64 | Ano (CUIDADO: confusão) | ⚠️ Variável de controle |
| `rocket_name` | str | Tipo de foguete | ✅ Para estratificar |
| `flight_number` | int64 | Número sequencial SpaceX | ℹ️ Ordem temporal |
| `launch_date` | str | Data ISO 8601 | ⚠️ Para timeline |
| `launch_name` | str | Nome da missão | ℹ️ Identificação |
| `core_id` | str | ID único do booster | ℹ️ Rastreamento |

---

## Questões de Pesquisa Recomendadas

### Q1: Distribuição de Experiência
```
Como se distribui a experiência dos boosters no dataset?
```

**Análise Básica**:
```python
df['reuse_count'].describe()
# Expected: mean ~4.6, min=0, max=13

df['reuse_count'].value_counts().sort_index()
# Veja: quantos boosters com 0 lançamentos? 1? 2? etc.
```

**Visualização**: Histograma ou gráfico de barras de reuse_count

---

### Q2: Correlação Simples
```
Conforme reuse_count aumenta, taxa de sucesso muda?
```

**Análise Quantitativa**:
```python
# Correlação de Pearson
corr = df['reuse_count'].corr(df['success'].astype(int))
print(f"Correlação: {corr:.4f}")

# Correlação de Spearman (não-linear)
from scipy.stats import spearmanr
rho, pval = spearmanr(df['reuse_count'], df['success'].astype(int))
print(f"Spearman rho: {rho:.4f}, p-value: {pval:.6f}")
```

**Interpretação**:
- r > 0.3: Correlação positiva (experiência → sucesso)
- r ≈ 0: Sem correlação
- r < -0.3: Correlação negativa (desgaste?)

---

### Q3: Análise por Faixas de Experiência
```
Como a taxa de sucesso varia para boosters com 0, 1-3, 4-6, 7+ lançamentos?
```

**Análise com Bins**:
```python
# Criar faixas
df['experience_level'] = pd.cut(
    df['reuse_count'],
    bins=[−1, 0, 3, 6, 13],
    labels=['Virgin (0)', 'Intermediate (1-3)', 'Experienced (4-6)', 'Veteran (7+)']
)

# Taxa de sucesso por faixa
experience_success = df.groupby('experience_level')['success'].agg(['count', 'sum', 'mean'])
print(experience_success)
```

**Esperado**: Se experiência importa, taxa aumenta monotonicamente.

---

### Q4: Regressão Linear
```
Qual é o efeito quantitativo de cada lançamento adicional?
```

**Regressão Simples**:
```python
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import r2_score

# Variável independente: reuse_count
X = df[['reuse_count']]

# Variável dependente: success (0/1)
y = df['success'].astype(int)

# Fit logistic regression (success é binária)
model = LogisticRegression()
model.fit(X, y)

# Coeficiente: efeito de +1 no reuse_count
coef = model.coef_[0][0]
print(f"Effect of +1 reuse: {coef:.4f} (log-odds)")

# Pseudo R-squared
score = model.score(X, y)
print(f"Accuracy: {score:.2%}")
```

---

### Q5: Controlando por Rocket Type
```
O efeito de reuse_count é igual para Falcon 1, 9 e Heavy?
```

**Análise Estratificada**:
```python
for rocket in df['rocket_name'].unique():
    subset = df[df['rocket_name'] == rocket]
    corr = subset['reuse_count'].corr(subset['success'].astype(int))
    print(f"\n{rocket} (n={len(subset)}):")
    
    # Taxa por faixa
    faixa = subset.groupby('reuse_count')['success'].agg(['count', 'mean'])
    print(faixa[faixa['count'] > 1])
```

---

### Q6: Possível "Desgaste" (Wear-Out)
```
Há um ponto em que reutilização excessiva prejudica?
```

**Análise Visual**:
```python
import matplotlib.pyplot as plt

# Scatter plot: reuse_count vs success
plt.scatter(df['reuse_count'], df['success'].astype(int), alpha=0.5)

# Adicionar tendência
z = np.polyfit(df['reuse_count'], df['success'].astype(int), 2)  # Polinômio grau 2
p = np.poly1d(z)
x_line = np.linspace(0, 13, 100)
plt.plot(x_line, p(x_line), 'r--', label='Trend (quadratic)')

plt.xlabel('Reuse Count')
plt.ylabel('Success (0/1)')
plt.title('Booster Experience vs Mission Success')
plt.legend()
plt.show()
```

**Padrões esperados**:
- Linear crescente: Mais experiência = melhor
- Decrescente no final: Desgaste em altos reuse_count
- Flat: Experiência não importa

---

## Testes Estatísticos

### Teste 1: Correlação de Spearman
```python
from scipy.stats import spearmanr

rho, pval = spearmanr(df['reuse_count'], df['success'].astype(int))
print(f"Spearman's rho: {rho:.4f}")
print(f"P-value: {pval:.6f}")
print(f"Significant (α=0.05)? {pval < 0.05}")
```

### Teste 2: ANOVA (Diferenças entre Grupos)
```python
from scipy.stats import f_oneway

# Separar por faixa de experiência
virgin = df[df['reuse_count'] == 0]['success'].astype(int)
intermediate = df[(df['reuse_count'] >= 1) & (df['reuse_count'] <= 3)]['success'].astype(int)
experienced = df[df['reuse_count'] >= 4]['success'].astype(int)

# ANOVA
f_stat, pval = f_oneway(virgin, intermediate, experienced)
print(f"F-statistic: {f_stat:.4f}")
print(f"P-value: {pval:.6f}")
print(f"Significant? {pval < 0.05}")
```

### Teste 3: Regressão Logística (Formal)
```python
from statsmodels.formula.api import logit

# Fit model
model = logit('success ~ reuse_count', data=df).fit()
print(model.summary())

# Interpretação:
# Coef positivo: +reuse_count → +probabilidade sucesso
# P-value < 0.05: Efeito significante
```

---

## Avisos Importantes

⚠️ **1. Correlação ≠ Causalidade**

Possíveis explicações para correlação positiva:
- ✅ Boosters melhoram com idade (aprendizado)
- ⚠️ Boosters novos têm pequenas falhas (design immaturo)
- ⚠️ SpaceX aprendeu ao longo do tempo (confusão temporal)
- ⚠️ Boosters defeituosos são descartados (selection bias)

**Como testar**: Controle por launch_year.

⚠️ **2. Distribuição Desigual**

Muito poucos boosters com alto reuse_count:
```python
df['reuse_count'].value_counts().sort_index().tail()
# Esperado: muitos 0-3, poucos 10-13
```

Isso reduz o poder estatístico para valores altos.

⚠️ **3. Possível Ceiling Effect**

Se taxa de sucesso já é ~100%, não há espaço para melhoria!

---

## Estrutura Sugerida do Notebook

```markdown
# Análise: Experiência de Booster e Confiabilidade

## 1. Setup
   - Load dataset
   - Check reuse_count distribution

## 2. Exploração Inicial
   - Estatísticas descritivas
   - Histograma de reuse_count
   - Taxa de sucesso geral

## 3. Main Analysis
   ### 3.1 Correlação Simples
   ### 3.2 Taxa de Sucesso por Faixas
   ### 3.3 Gráfico Scatter + Tendência
   ### 3.4 Regressão Logística

## 4. Análise Estratificada
   ### 4.1 Por Rocket Type
   ### 4.2 Por Período Temporal

## 5. Testes Estatísticos
   - Spearman Correlation
   - ANOVA
   - Regressão com p-values

## 6. Conclusões
   - Existe efeito de experiência?
   - Qual é o tamanho?
   - Controlando por confusões?
```

---

## Exemplos de Código

### Exemplo 1: Correlação
```python
import pandas as pd
from scipy.stats import spearmanr

df = pd.read_csv('data/processed/processed_dataset_v1.csv')

# Pearson
pearson_r = df['reuse_count'].corr(df['success'].astype(int))
print(f"Pearson r: {pearson_r:.4f}")

# Spearman
spearman_rho, pval = spearmanr(df['reuse_count'], df['success'].astype(int))
print(f"Spearman rho: {spearman_rho:.4f}, p={pval:.6f}")
```

### Exemplo 2: Faixas de Experiência
```python
# Criar faixas
bins = pd.cut(df['reuse_count'], bins=[−1, 0, 3, 6, 13])
faixa_success = df.groupby(bins)['success'].agg(['count', 'mean'])
print(faixa_success)
```

### Exemplo 3: Regressão
```python
from statsmodels.formula.api import logit

model = logit('success ~ reuse_count', data=df).fit()
print(model.summary())
```

---

## Deliverables

Quando terminar:
1. **Notebook** reproduzível
2. **Tabelas** de correlações e testes
3. **Gráficos**:
   - Scatter plot (reuse_count vs success)
   - Taxa por faixa de experiência
   - Distribuição de reuse_count
4. **Conclusões** (1-2 parágrafos):
   - Existe correlação significante?
   - Qual o tamanho do efeito?
   - Possíveis confusões?

---

## Contato

Dúvidas? Veja:
- `docs/data_dictionary.md`
- `docs/data_quality_report.md`
- `docs/MILESTONE_v1_COMPLETE.md`

**Data Manager**: Lucas

---

**Boa análise!** 📈
