# SpaceX Reliability Study

## Research Question

Does rocket reusability affect mission reliability?

## Team

- Lucas
- Gabriel
- Kaio
- Izac
- Jonatas

## Data Source

SpaceX API v4

## Repository Structure

Aqui está a organização do nosso repositório e o propósito de cada diretório:

* **`data/`**: Armazenamento de dados do projeto.
  * `raw/`: Dados brutos extraídos diretamente da API da SpaceX (backup sem alterações).
  * `processed/`: Dados limpos, transformados e validados (`processed_dataset_v1.csv`). **Atenção:** Esta é a "fonte da verdade" e estes arquivos não devem ser alterados.
* **`docs/`**: Documentação de governança, planejamento (`ROADMAP.md`), dicionário de dados e guias de análise individuais.
* **`analyses/`**: Ambiente de trabalho individual. Os analistas (Gabriel, Kaio, Izac, Jonatas) devem criar e manter seus notebooks Jupyter exploratórios aqui.
* **`notebooks/`**: Scripts centrais do projeto, como o pipeline de extração (ETL) e o `master_analysis.ipynb` (que consolidará apenas os resultados finais validados na Fase 3).
* **`graphs/`**: Imagens e visualizações finais (ex: `.png`, `.svg`) geradas a partir das análises, que serão utilizadas no pôster e relatório.
* **`poster/`**: Materiais visuais, recursos e exportações para a apresentação e pôster científico final.
* **`report/`**: Arquivos do documento conclusivo da pesquisa (relatório final unificado contendo introdução, metodologia, resultados e conclusões).