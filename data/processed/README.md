# Dataset Versions

## `processed_dataset_v1.csv` ⭐ (RECOMENDADO)

**Status**: 🟢 **APPROVED FOR RELEASE**  
**Date**: 2024-01 / Junho 2026  
**Records**: 192 rows × 9 columns  
**Quality**: 100% complete (0 null values)  

### O que é?
Versão congelada e oficialmente aprovada do dataset SpaceX para análise estatística. Esta é a versão que deve ser usada para:
- Análises estatísticas da equipe
- Compartilhamento no GitHub
- Relatórios finais
- Reprodutibilidade de resultados

### QA Status
```
✅ 192 linhas
✅ 0 nulos
✅ 0 duplicatas
✅ Schema validado
✅ Tipos validados
✅ Multi-booster validado
```

### Evidência Preliminar
| Booster Type | Taxa Sucesso |
|--------------|-------------|
| Virgem (n=43) | 88.37% |
| Reutilizado (n=149) | 100.00% |
| **Diferença** | **+11.63 pp** |

---

## `processed_dataset.csv` (Descontinuado)

**Status**: 🟡 **DEPRECATED - Não use para análises**

Esta é a versão intermediária de desenvolvimento. Mantém-se aqui apenas por referência histórica do notebook, mas deve ser ignorada em favor de `processed_dataset_v1.csv`.

---

## Instruções de Uso

### Para Análises
```python
import pandas as pd

# ✅ CORRETO - use v1
df = pd.read_csv('data/processed/processed_dataset_v1.csv')

# ❌ ERRADO - não use deprecated
# df = pd.read_csv('data/processed/processed_dataset.csv')
```

### Para Git Commits
```bash
git add data/processed/processed_dataset_v1.csv
git commit -m "Release dataset v1.0 - QA approved (192 rows, 0 nulls)"
```

---

## Schema

| Column | Type | Nulls | Description |
|--------|------|-------|-------------|
| `launch_year` | int64 | 0 | Year of launch (2006-2022) |
| `launch_name` | object | 0 | Mission name |
| `flight_number` | int64 | 0 | Sequential mission number |
| `success` | bool | 0 | Mission success (True/False) |
| `rocket_name` | object | 0 | Rocket family (Falcon 1/9/Heavy) |
| `core_id` | object | 0 | Booster ID |
| `reuse_count` | int64 | 0 | Previous launches of this booster |
| `is_reused` | bool | 0 | Whether booster was reused (True/False) |
| `launch_date` | object | 0 | ISO 8601 timestamp (UTC) |

---

## Origem dos Dados

- **Source**: SpaceX API v4 (https://api.spacexdata.com/v4)
- **Extraction Date**: 2024 / Junho 2026
- **Launches Included**: All executed missions through 2022
- **Removed Records**: 23 unexecuted planned launches (2022)
- **Orientation**: 1 row per booster (not per launch)

---

## Próximas Ações

1. **GitHub Push**
   ```bash
   git add data/processed/processed_dataset_v1.csv
   git commit -m "Release dataset v1.0 - 192 rows, 0 nulls, QA approved"
   git push
   ```

2. **Team Distribution**
   - Share `processed_dataset_v1.csv` with Gabriel, Kaio, Izac, Jonatas
   - Share `docs/data_dictionary.md` for column definitions
   - Reference `docs/data_quality_report.md` for QA details

3. **Statistical Analysis Phase**
   - Gabriel: Temporal trends
   - Kaio: Reusability impact
   - Izac: Booster experience
   - Jonatas: Rocket family comparison

---

**Dataset Manager**: Lucas  
**Approval Date**: 2024-01  
**Status**: ✅ Ready for Production Use
