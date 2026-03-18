# 🧠 Previsão de Estoque Inteligente na AWS com SageMaker Canvas

> Projeto desenvolvido como parte do Lab da [DIO](https://www.dio.me/) — **Bootcamp de Machine Learning na AWS**

---

## 📌 Sobre o Projeto

Este projeto tem como objetivo construir um modelo de **Machine Learning no-code** para prever o estoque de produtos utilizando o **Amazon SageMaker Canvas**. A proposta é demonstrar como é possível criar soluções preditivas robustas sem escrever uma única linha de código, tornando o ML acessível para diferentes perfis profissionais.

---

## 🗂️ Dataset Utilizado

O dataset foi **gerado e enriquecido manualmente** com Python, simulando um histórico realista de movimentação de estoque com as seguintes colunas:

| Coluna | Tipo | Descrição |
|---|---|---|
| `ID_PRODUTO` | Numérico (incremental) | Identificador único do produto |
| `DIA` | Data (dd/mm/aaaa) | Data do registro, iniciando em 31/12/2023 |
| `FLAG_PROMOCAO` | Binário (0 ou 1) | Indica se o produto estava em promoção |
| `QUANTIDADE_ESTOQUE` | Numérico | Quantidade em estoque ao final do dia |

**Características do dataset:**
- 🗃️ **821 registros** distribuídos ao longo de 30 dias
- 🏷️ **30 produtos distintos** por período
- 📉 Estoque com **decrescimento variável** dia a dia (simulando vendas reais)
- 🔴 **150 registros com estoque zerado** — produto esgotado
- 🎯 **215 registros com promoção ativa**, criando correlação clara para o modelo aprender
- 🔁 Reabastecimentos esporádicos quando estoque crítico (< 20 unidades)

---

## ⚙️ Passo a Passo — SageMaker Canvas

### 1. 📥 Seleção e Upload do Dataset

O arquivo `historico_vendas_estoque.csv` foi importado diretamente para o **SageMaker Canvas** via upload. O Canvas reconheceu automaticamente os tipos de dados de cada coluna.

> 💡 **Dica:** Certifique-se de que a coluna de data esteja no formato reconhecível pelo Canvas para que o modelo de série temporal seja configurado corretamente.

---

### 2. 🏗️ Construção e Treinamento do Modelo

No Canvas, as configurações foram definidas da seguinte forma:

- **Tipo de problema:** Time Series Forecasting (Previsão de Série Temporal)
- **Coluna alvo (Target):** `QUANTIDADE_ESTOQUE`
- **Coluna de ID do item:** `ID_PRODUTO`
- **Coluna de tempo:** `DIA`
- **Features adicionais:** `FLAG_PROMOCAO`
- **Horizonte de previsão:** 30 dias à frente

O treinamento foi iniciado com a opção **Standard Build**, que realiza uma exploração automática de algoritmos e seleciona o melhor modelo com base nos dados fornecidos.

---

### 3. 📊 Análise do Modelo

Após o treinamento, o SageMaker Canvas apresentou as métricas de desempenho e os fatores mais influentes nas previsões:

#### Métricas de Performance

| Métrica | Valor |
|---|---|
| wQL (Weighted Quantile Loss) | Avaliado pelo Canvas |
| MAPE | Calculado automaticamente |
| RMSE | Calculado automaticamente |

#### Principais Insights

- A `FLAG_PROMOCAO` mostrou-se uma **feature relevante**, confirmando que promoções aceleram o consumo de estoque — exatamente como foi simulado no dataset.
- Produtos com **alta variabilidade de vendas** apresentaram intervalos de confiança mais amplos (diferença entre P10 e P90).
- O modelo capturou bem a **tendência de decrescimento** do estoque ao longo do tempo.

---

### 4. 🔮 Previsões Geradas

As previsões foram realizadas no modo **Single Prediction**, permitindo analisar produto a produto.

#### Exemplo: SKU-234

![Análise de Demanda - SageMaker Canvas](./Análise_de_Demanda_AWS_SageMaker_Canvas.png)

> *Gráfico gerado diretamente no SageMaker Canvas — aba "Predict > Single Prediction"*

O gráfico apresenta três bandas de previsão para o período **2019-09-01 a 2019-10-01**:

| Quantil | Interpretação |
|---|---|
| **P10** (rosa) | Cenário pessimista — demanda mínima esperada |
| **P50** (verde) | Cenário mediano — previsão central mais provável |
| **P90** (amarelo/dourado) | Cenário otimista — demanda máxima esperada |

<img width="1913" height="902" alt="Análise de Demanda AWS SageMaker Canvas" src="https://github.com/user-attachments/assets/8a26a896-f869-4354-952f-f396f570509e" />


A amplitude entre P10 e P90 representa a **incerteza da previsão**: quanto maior o intervalo, maior a variabilidade esperada na demanda do produto.

Os resultados foram exportados via **"Download prediction"** para análise posterior em ferramentas como Excel ou Python/Pandas.

---

## 💡 Insights e Conclusões

- ✅ O SageMaker Canvas demonstrou ser uma ferramenta poderosa para **previsão de demanda no-code**, ideal para equipes de negócios que precisam de agilidade.
- 📈 A utilização de **quantis (P10/P50/P90)** é extremamente útil para decisões de estoque: use P90 para evitar rupturas, P10 para minimizar excesso.
- 🔄 A `FLAG_PROMOCAO` agregou valor preditivo ao modelo — **dados contextuais fazem diferença**.
- 🤖 Com poucos cliques, foi possível treinar e obter previsões que poderiam alimentar um **sistema de reabastecimento automático**.

---

## 🛠️ Tecnologias Utilizadas

- **Python 3** — Geração e enriquecimento do dataset
- **Amazon SageMaker Canvas** — Treinamento e inferência do modelo de ML
- **AWS S3** — Armazenamento dos datasets e resultados

---

## 📁 Estrutura do Repositório

```
lab-aws-sagemaker-canvas-estoque/
│
├── datasets/
│   └── historico_vendas_estoque.csv   # Dataset gerado para o projeto
│
├── Análise_de_Demanda_AWS_SageMaker_Canvas.png  # Screenshot das previsões
│
└── README.md                          # Documentação completa do projeto
```

---

## 🚀 Como Reproduzir

1. Faça um **fork** deste repositório
2. Acesse sua conta na **AWS** e abra o **SageMaker Canvas**
3. Faça o upload do arquivo `datasets/historico_vendas_estoque.csv`
4. Configure o modelo conforme descrito na seção "Construção e Treinamento"
5. Treine, analise e gere suas próprias previsões!

---

## 👤 Autor

Desenvolvido com 💙 durante o Bootcamp de Machine Learning na AWS — **Digital Innovation One (DIO)**

---

*"Dados são o novo petróleo. Mas, como o petróleo bruto, são valiosos — mas apenas se refinados."* — Clive Humby
