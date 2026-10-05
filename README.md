#Integrantes 
- Felipe Mitsuo Takahashi Stephano RM570692
- Laura Godoy Callegari RM569181
- Letícia Araújo Espindola RM569308
- Mariana Dreset Carbollan 569207
- Milena de Aguiar Lopes Cardoso
RM570599
 

# Regressão Linear com Dados de Energia Solar

Projeto desenvolvido para aplicar conceitos de **Machine Learning** utilizando dados reais de geração de energia solar obtidos por meio da API pública do **PVGIS (Photovoltaic Geographical Information System)**.

O objetivo principal é estimar a potência gerada por um sistema fotovoltaico utilizando **Regressão Linear** e comparar o desempenho de dois modelos construídos com diferentes conjuntos de variáveis.

---

## Sobre o projeto

O projeto percorre as principais etapas de um problema de Machine Learning:

- consulta a uma API pública;
- transformação dos dados em DataFrame;
- inspeção e tratamento dos dados;
- análise exploratória;
- visualização das relações entre as variáveis;
- cálculo da matriz de correlação;
- definição das variáveis de entrada e da variável alvo;
- divisão entre treino e teste;
- treinamento de dois modelos de Regressão Linear;
- avaliação com MAE, MSE e R²;
- comparação dos resultados.

---

## Fonte dos dados

Os dados foram obtidos através do **PVGIS**, utilizando o recurso de dados horários `seriescalc`.

### Localização utilizada

- **Cidade:** São Paulo - SP
- **Latitude:** -23.5505
- **Longitude:** -46.6333
- **Período:** 2021
- **Fonte:** PVGIS
- **Formato da resposta:** JSON

A consulta é realizada diretamente em Python utilizando a biblioteca `requests`.

---

## Variáveis analisadas

As principais variáveis utilizadas no projeto são:

| Variável | Descrição |
|---|---|
| `P` | Potência fotovoltaica gerada |
| `G(i)` | Irradiância solar |
| `H_sun` | Altura do Sol |
| `T2m` | Temperatura do ar |
| `WS10m` | Velocidade do vento |
| `time` | Data e horário do registro |

A variável alvo dos modelos é:

```python id="r4x9km"
y = df_modelo["P"]
```

Portanto, o objetivo é prever a **potência fotovoltaica gerada**.

---

## Preparação dos dados

Antes do treinamento, o conjunto de dados é analisado utilizando recursos do Pandas, como:

```python id="0pgqns"
df.shape
df.columns
df.info()
df.isnull().sum()
df.describe()
```

Também são analisados os períodos sem irradiância solar.

Para o treinamento dos modelos, foram mantidos apenas registros com irradiância maior que zero:

```python id="vvto1h"
df_modelo = df[df["G(i)"] > 0].copy()
```

Essa decisão busca evitar que uma grande quantidade de registros noturnos, normalmente com irradiância e potência iguais a zero, influencie excessivamente o treinamento.

---

## Análise exploratória

Foram criados gráficos de dispersão para observar a relação da potência fotovoltaica com variáveis como:

- irradiância;
- temperatura;
- altura solar.

Também foi calculada uma **matriz de correlação** para analisar quais variáveis possuem maior relação linear com a potência gerada.

---

## Modelos de Regressão Linear

Foram construídos dois modelos utilizando diferentes conjuntos de variáveis.

### Modelo 1

Utiliza:

```text id="33rm1q"
G(i)
H_sun
```

Esse modelo considera principalmente variáveis diretamente relacionadas à incidência e à posição da radiação solar.

### Modelo 2

Utiliza:

```text id="v3dx4g"
G(i)
H_sun
T2m
WS10m
```

O segundo modelo adiciona variáveis meteorológicas para verificar se temperatura e velocidade do vento contribuem para melhorar as previsões.

---

## Divisão dos dados

Os dados são separados entre treinamento e teste utilizando `train_test_split()`.

```python id="lbpjvg"
train_test_split(
    X,
    y,
    test_size=0.20,
    random_state=42
)
```

A divisão utilizada é:

- **80% para treinamento**
- **20% para teste**

O `random_state` garante que a divisão possa ser reproduzida.

---

## Avaliação dos modelos

Os dois modelos são avaliados através de três métricas.

### MAE

**Mean Absolute Error**

Representa o erro médio absoluto entre os valores reais e previstos.

Quanto menor, melhor.

### MSE

**Mean Squared Error**

Calcula a média dos erros elevados ao quadrado.

Erros maiores recebem uma penalização maior.

Quanto menor, melhor.

### R²

**Coeficiente de Determinação**

Indica o quanto o modelo consegue explicar a variação da variável alvo.

Quanto mais próximo de `1`, melhor o ajuste do modelo.

---

## Comparação dos resultados

Ao final, os resultados dos modelos são organizados em uma tabela no seguinte formato:

| Modelo | Variáveis utilizadas | MAE | MSE | R² |
|---|---|---:|---:|---:|
| Modelo 1 | G(i), H_sun | resultado | resultado | resultado |
| Modelo 2 | G(i), H_sun, T2m, WS10m | resultado | resultado | resultado |

Os valores são calculados diretamente durante a execução do notebook.

O melhor modelo é analisado considerando:

- maior R²;
- menor MAE;
- menor MSE.

---

## Principais conclusões

A geração de energia solar possui forte relação com a irradiância, porém outros fatores também podem influenciar a potência produzida.

Entre eles:

- temperatura;
- velocidade do vento;
- posição do Sol;
- presença de nuvens;
- sombras;
- alterações rápidas nas condições ambientais.

Por esse motivo, nem todas as relações são perfeitamente lineares.

Além disso, uma variável com alta correlação individual com a potência não necessariamente produz o melhor resultado quando combinada com outras variáveis.

A comparação entre os dois modelos permite avaliar se a inclusão de novas características melhora ou não a capacidade de previsão.

---

## Tecnologias utilizadas

- Python
- Pandas
- Matplotlib
- Scikit-learn
- Requests
- Jupyter Notebook
- Google Colab
- PVGIS API

---

## Estrutura do projeto

```text id="waomfv"
├── Regressao_Linear_Energia_Solar_Organizado.ipynb
└── README.md
```

---

## Como executar

Clone o repositório:

```bash id="dz8jsy"
git clone URL_DO_REPOSITORIO
```

Acesse a pasta:

```bash id="gn97cl"
cd NOME_DO_REPOSITORIO
```

Instale as dependências:

```bash id="2wagw3"
pip install pandas matplotlib scikit-learn requests
```

Depois, abra o notebook utilizando:

- Google Colab;
- Jupyter Notebook;
- JupyterLab;
- VS Code.

Execute as células na ordem apresentada.

---

## Objetivo acadêmico

Este projeto foi desenvolvido como atividade prática de preparação para o estudo de **Regressão Linear**, utilizando dados reais de energia solar para aplicar conceitos de análise de dados e Machine Learning.
