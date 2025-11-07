# XGBoost_version_3.0
O XGboost (Extreme Gradient Boosting) é um modelo de `ensembled learning` baseado em árvores de decisão, ou seja, as árvores sucessoras tentam corrigir os erros das predecessoras. Nesse caso utilizei técnicas como:

`Target Encoding, One hot encoding, Cross Validation, Feature Engineering.`


## Técnicas Utilizadas

**1. Target Encoding**
- Converte categorias em números baseados na média do valor alvo (target). Reduz dimensionalidade transformando múltiplas categorias em uma única coluna numérica que captura a relação direta da categoria com o target. Evita criar dezenas de colunas como no One-Hot Encoding.

**2. One-Hot Encoding**
- Transforma cada categoria em uma coluna binária (0 ou 1). Permite que o modelo entenda categorias sem assumir ordem artificial entre elas. Funciona bem para variáveis com poucas categorias.

**3. Cross Validation**
- Divide os dados em K partes (folds) e treina/testa K vezes, cada vez usando uma parte diferente para teste. Fornece avaliação mais robusta do modelo, detecta overfitting comparando performance entre folds, e garante que todos os dados sejam usados tanto para treino quanto para validação.

**4. Feature Engineering**
- Criação de novas variáveis a partir das existentes para capturar padrões que o modelo sozinho não enxergaria facilmente. Combina features existentes em métricas mais informativas (como poder_compra e razao_valor_renda), ajudando o modelo a aprender relações complexas de forma mais eficiente. Geralmente é a técnica que mais impacta a performance final.

## Vantagens:

-Excelente para dados tabulares
-Robusto a outliers
-Captura bem relações não lineares
-Possui regularização implementada
-Extremamente rápido e eficiente


## Obtenção de melhora:
O modelo apresentava overfitting em profissões específicas, por exemplo profissao_empresario representando 11.2% da importância total, indicando memorização de padrões ao invés de generalização adequada (overfitting).
## Técnicas Aplicadas

**1. Pré-processamento**
- Limpeza de dados (lowercase, strip de espaços)
- Agrupamento de categorias raras (< 2% dos dados) em "outros"

**2. Target Encoding**
Aplicado exclusivamente na feature `profissao`, convertendo 30 categorias em uma única coluna numérica baseada na média do target. Isso reduziu a importância de profissões de 25.7% para 5.1%, eliminando overfitting.

**3. One-Hot Encoding**
Utilizado nas demais features categóricas (`gênero`, `estado_civil`, `cidade`, `urgência`, `forma_pagamento`, `objetivo`) com `drop_first=True` para evitar multicolinearidade.

**4. Feature Engineering**
Criação de features derivadas para capturar relações complexas:
- `poder_compra = score_credito × renda_mensal / 1000`: tornou-se a feature mais importante (36.6%)
- `razao_valor_renda = valor_imovel / renda_mensal`: mede affordability

**5. Normalização**
StandardScaler aplicado em features numéricas (`renda_mensal`, `score_credito`, `idade`, `valor_imovel`, `composicao_familiar`) após o split treino-teste para evitar `data leakage`.

**6. Cross-Validation (5-folds)**
Validação cruzada com re-encoding em cada fold garantiu avaliação robusta. CV RMSE (0.46) próximo ao teste (0.44) confirma boa generalização.

**7. Regularização XGBoost**
Hiperparâmetros ajustados (`max_depth=4`, `learning_rate=0.02`) para balancear performance e prevenção de overfitting.

## Resultados

O modelo final apresentou **melhoria de 53%** no RMSE comparado à versão inicial que não possuia feature engineering e aplicava label encoding ao invés de target encoding . Features numéricas e engenhadas dominam 62.8% da importância, garantindo predições baseadas em capacidade financeira real.
