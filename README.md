# Checkpoint 02 — Regressão Linear

## Integrantes

* Felipe Mitsuo Takahashi Stephano — RM570692
* Laura Godoy Callegari — RM569181
* Letícia Araújo Espindola — RM569308
* Mariana Dreset Carbollan — RM569207
* Milena de Aguiar Lopes Cardoso — RM570599

## Sobre o projeto

Este projeto foi desenvolvido para aplicar conceitos de Machine Learning utilizando modelos de Regressão Linear em dois conjuntos de dados diferentes.

O trabalho foi dividido em duas partes:

* **Parte 1:** Regressão Linear com dados de Energia Solar.
* **Parte 2:** Regressão Linear utilizando o índice de volume do PIB e o Índice ABCR.

Em ambas as partes foram realizados processos de coleta e preparação dos dados, análise de correlação, treinamento dos modelos e avaliação dos resultados utilizando métricas como MAE, MSE e R².

---

# Parte 1 — Regressão Linear com Dados de Energia Solar

## Objetivo

Utilizar dados horários reais do **PVGIS (Photovoltaic Geographical Information System)** para estimar a potência produzida por um sistema fotovoltaico utilizando dois modelos de Regressão Linear.

## Dados utilizados

* **Local:** São Paulo - SP
* **Latitude:** -23.5505
* **Longitude:** -46.6333
* **Período:** 2021
* **Fonte:** PVGIS
* **Recurso utilizado:** `seriescalc`

Foram utilizadas variáveis relacionadas às condições de geração de energia solar:

* `P` — potência fotovoltaica
* `G(i)` — irradiância
* `H_sun` — altura do Sol
* `T2m` — temperatura
* `WS10m` — velocidade do vento

## Análise de correlação

A análise mostrou que a variável com maior correlação com a potência foi a irradiância:

| Variável | Correlação com P |
| -------- | ---------------: |
| G(i)     |            0.998 |
| H_sun    |            0.680 |
| T2m      |            0.379 |
| WS10m    |            0.009 |

A irradiância apresentou uma relação muito forte com a potência fotovoltaica, enquanto a velocidade do vento apresentou uma correlação praticamente nula.

## Modelos utilizados

Foram treinados dois modelos:

**Modelo 1**

* `G(i)`
* `H_sun`

**Modelo 2**

* `G(i)`
* `H_sun`
* `T2m`
* `WS10m`

Os dados foram separados em conjuntos de treinamento e teste para avaliar o desempenho dos modelos.

## Resultados

| Modelo   | Variáveis utilizadas    |   MAE |    MSE |     R² |
| -------- | ----------------------- | ----: | -----: | -----: |
| Modelo 1 | G(i), H_sun             | 10.54 | 217.08 | 0.9967 |
| Modelo 2 | G(i), H_sun, T2m, WS10m |  9.29 | 139.86 | 0.9978 |

O **Modelo 2 apresentou o melhor resultado** nas três métricas. Ele apresentou menor MAE e MSE e maior R².

A inclusão das variáveis de temperatura e velocidade do vento trouxe uma pequena melhora na previsão da potência.

## Conclusão da Parte 1

Os resultados mostram que a Regressão Linear conseguiu representar muito bem a relação entre as condições analisadas e a potência fotovoltaica.

A irradiância foi a variável com maior relação com a potência. Mesmo apresentando uma correlação muito baixa individualmente, a velocidade do vento pode contribuir para o modelo quando utilizada junto com outras variáveis.

Alguns fatores podem dificultar a previsão da potência, como variações de nuvens, temperatura, vento, posição do Sol, sombras e mudanças rápidas na irradiância.

---

# Parte 2 — Regressão Linear com PIB e Índice ABCR

## Objetivo

Analisar a relação entre a atividade econômica brasileira, representada pelo **índice de volume do PIB**, e o fluxo de veículos nas rodovias, representado pelo **Índice ABCR Total**.

Depois da análise, foi utilizado um modelo de Regressão Linear para estimar o Índice ABCR a partir do índice do PIB.

## Fontes dos dados

Foram utilizadas as seguintes fontes:

* **IBGE / SIDRA — Tabela 1620:** índice de volume trimestral do PIB, Brasil, PIB a preços de mercado, sem ajuste sazonal.
* **ABCR — Índice ABCR:** série histórica original, Brasil, TOTAL, em número-índice.

O período analisado foi de **2006 a 2025**, totalizando 20 anos completos.

## Preparação dos dados

Como as duas bases possuíam periodicidades diferentes, foi necessário transformar os dados para uma mesma escala anual.

* `PIB_indice`: média dos 4 índices trimestrais do PIB de cada ano.
* `ABCR_indice`: média dos 12 índices mensais da ABCR de cada ano.

Com isso, foi criada uma base anual contendo:

| Ano  | PIB_indice | ABCR_indice |
| ---- | ---------: | ----------: |
| 2006 |        ... |         ... |
| 2007 |        ... |         ... |
| ...  |        ... |         ... |
| 2025 |        ... |         ... |

A base anual também foi salva como `base_anual_2006_2025.csv`.

## Análise da correlação

Foi criado um gráfico de dispersão com:

* eixo X: `PIB_indice`
* eixo Y: `ABCR_indice`

A correlação de Pearson encontrada foi:

**0.9641**

Esse resultado indica uma associação linear **forte e positiva** entre o índice de volume do PIB e o Índice ABCR no período analisado.

Isso significa que anos com maior índice de volume do PIB tendem a apresentar também um maior Índice ABCR.

É importante destacar que uma correlação alta não significa que exista uma relação de causa e efeito entre as duas variáveis.

## Modelo de Regressão Linear

Para o treinamento do modelo foi utilizada:

* **X:** `PIB_indice`
* **y:** `ABCR_indice`

A divisão dos dados respeitou a ordem cronológica:

* **Treinamento:** 2006 a 2021
* **Teste:** 2022 a 2025

Não foi utilizado embaralhamento dos dados.

## Avaliação do modelo

Foram utilizadas as métricas MAE, MSE e R².

| Métrica | Resultado |
| ------- | --------: |
| MAE     |    4.9489 |
| MSE     |   25.9502 |
| R²      |    0.4849 |

### Interpretação

* **MAE = 4.9489:** as previsões ficaram, em média, aproximadamente 4.95 pontos do Índice ABCR distantes dos valores observados.
* **MSE = 25.9502:** representa a média dos erros ao quadrado, dando maior peso para erros maiores.
* **R² = 0.4849:** aproximadamente 48,49% da variação observada no conjunto de teste foi explicada pelo modelo.

Apesar da correlação entre PIB e ABCR ser forte, o modelo utilizando apenas o PIB não conseguiu explicar completamente as variações do Índice ABCR no período de teste.

## Conclusão da Parte 2

A análise mostrou uma relação forte e positiva entre o índice de volume do PIB e o Índice ABCR.

Porém, os resultados da regressão mostram que o PIB sozinho não é suficiente para explicar todas as variações no fluxo de veículos. Outros fatores podem influenciar o resultado, como preço dos combustíveis, transporte de cargas, mobilidade, infraestrutura e acontecimentos econômicos excepcionais.

---

# Dificuldades encontradas

Durante o desenvolvimento das duas partes, algumas dificuldades precisaram ser resolvidas.

### Parte 1

Foi necessário trabalhar com dados horários do PVGIS e organizar as informações para que pudessem ser utilizadas nos modelos de Regressão Linear.

Também foi necessário comparar diferentes combinações de variáveis para identificar qual modelo apresentava o melhor desempenho.

### Parte 2

A principal dificuldade foi trabalhar com bases de periodicidades diferentes. O PIB possuía dados trimestrais, enquanto o ABCR possuía dados mensais.

Para solucionar isso, foram calculadas médias anuais das duas séries.

Outra etapa importante foi selecionar corretamente as séries do PIB e do ABCR, mantendo os dados originais e sem ajuste sazonal conforme solicitado no enunciado.

---

# Tecnologias utilizadas

* Python
* Google Colab
* Pandas
* Matplotlib
* Scikit-learn
* Requests
* Jupyter Notebook

---

# Estrutura do projeto

```text
CP2_MLAM
├── CP02_MLAM_Pt_01.ipynb
├── CP02_MLAM_pt02.ipynb
├── README.md
└── dados
    ├── pib_ibge.xlsx
    └── abcr_historico..xlsx
```

Na Parte 2 também é gerado o arquivo:

```text
base_anual_2006_2025.csv
```

---

# Como executar

Os notebooks podem ser executados pelo **Google Colab**.

Na Parte 1, os dados são obtidos diretamente do PVGIS por meio da API.

Na Parte 2, as planilhas utilizadas são carregadas diretamente do repositório, evitando a necessidade de fazer upload manual dos arquivos no Colab.

Basta abrir o notebook correspondente e executar as células em ordem.

---

# Considerações finais

As duas partes permitiram aplicar a Regressão Linear em problemas diferentes.

Na **Parte 1**, o modelo apresentou um desempenho muito alto na previsão da potência fotovoltaica, principalmente quando foram utilizadas as variáveis `G(i)`, `H_sun`, `T2m` e `WS10m`.

Na **Parte 2**, foi encontrada uma forte relação entre PIB e Índice ABCR, mas o modelo apresentou desempenho mais limitado no conjunto de teste, mostrando que outras variáveis também são importantes para explicar o fluxo de veículos.

Dessa forma, o projeto permitiu comparar como a Regressão Linear pode apresentar resultados diferentes dependendo dos dados, das variáveis utilizadas e do problema analisado.
