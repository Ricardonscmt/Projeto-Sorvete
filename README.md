# 🚩 Projeto Sorvete - Previsão de Vendas com Azure ML Designer

Este projeto utiliza o **Azure Machine Learning Designer** para criar e avaliar um modelo de regressão. O objetivo é prever a quantidade de vendas de sorvete (`vendas`) com base na `temperatura` do dia.

## 📊 1. Conjunto de Dados (Dataset)

* **Nome do Arquivo:** `gelato-dados.csv`
* **Origem:** O conjunto de dados foi criado aleatoriamente para este exercício.
* **Tamanho:** 100 linhas.
* **Colunas Principais:**
    * `datas`: A data do registro.
    * `temperatura`: A temperatura registrada no dia (usada como *feature*).
    * `vendas`: A quantidade de sorvetes vendidos (usada como *label* ou alvo da previsão).

## 🖥️ 2. Configuração do Ambiente Azure ML

Antes da criação do pipeline, o ambiente no Azure Machine Learning foi preparado com os seguintes passos:

1.  **Criação do Workspace:** Um novo Workspace do Azure Machine Learning foi criado para centralizar todos os recursos.
2.  **Criação da Instância de Computação:** Utilizada como estação de trabalho para desenvolver e testar os pipelines.
3.  **Criação do Cluster de Computação:** Provisionado para ser o alvo de execução (treinamento) do pipeline.
4.  **Registro dos Dados:** O arquivo `gelato-dados.csv` foi carregado para a aba "Dados" (Data assets) dentro do workspace para ser acessível pelo Designer.

## ⚙️ 3. Pipeline do Designer

O pipeline de treinamento foi construído no Designer conectando os seguintes componentes:

[<img width="1343" height="698" alt="estrutura" src="https://github.com/user-attachments/assets/c4ab1602-cd89-446a-8026-089275165f23" />](https://github.com/Ricardonscmt/Projeto-Sorvete/blob/main/estrutura.PNG)

### Detalhes dos Componentes:

1.  **`gelato-dados-csv` (Dataset)**
    * O pipeline inicia carregando o conjunto de dados que foi previamente registrado no workspace.

2.  **`Select Columns in Dataset`**
    * Este componente foi usado para filtrar o dataset, selecionando apenas as colunas necessárias para o modelo: `temperatura` (a feature) e `vendas` (o label). A coluna `datas` foi provavelmente excluída nesta etapa.

3.  **`Split Data`**
    * Os dados foram divididos em dois conjuntos:
        * Um para **treinamento**.
        * Um para **teste**, usado para avaliar o modelo.

4.  **`Linear Regression` (Algoritmo)**
    * Este é o algoritmo de machine learning escolhido. Como o objetivo é prever um valor numérico (vendas), um modelo de **Regressão Linear** foi selecionado.

5.  **`Train Model` (Treinamento)**
    * Este componente é o coração do processo. Ele recebe:
        * O algoritmo não treinado (`Linear Regression`).
        * O conjunto de dados de **treinamento** (do `Split Data`).
    * Ele então treina o modelo para encontrar a relação matemática entre a `temperatura` e as `vendas`.

6.  **`Score Model` (Pontuação)**
    * Após o treinamento, o modelo (agora treinado) é usado para fazer previsões.
    * Este componente recebe o modelo treinado e o conjunto de dados de **teste** (do `Split Data`), gerando uma nova coluna com os valores de vendas previstos.

7.  **`Evaluate Model` (Avaliação)**
    * O componente final compara as previsões (do `Score Model`) com os valores reais de `vendas` do conjunto de teste.
    * Ele calcula métricas de desempenho para determinar o quão bom o modelo é.

## 📈 4. Resultados (Avaliação do Modelo)

Após a execução do pipeline, o componente `Score Model` (Pontuar Modelo) gera um novo conjunto de dados. Este conjunto inclui a `Temperatura` original, o valor real de `Vendas` e a nova coluna **`Scored Labels`**, que representa a previsão do modelo para cada linha.

A imagem abaixo mostra uma visualização da saída deste componente:

[<img width="1695" height="834" alt="Resultados" src="https://github.com/user-attachments/assets/8ac02ab8-7a10-4bf1-8c44-a75c13e4b9d4" />](https://github.com/Ricardonscmt/Projeto-Sorvete/blob/main/Resultados.PNG)

| Temperatura | Vendas (Real) | Scored Labels (Previsto) |
| :---: | :---: | :---: |
| 29.1 | 310 | 308.708744 |
| 25.0 | 258 | 250.167521 |
| 33.2 | 374 | 367.259968 |
| 33.7 | 378 | 374.400361 |

## 📊 5. Avaliação do Modelo

Por fim, o componente `Evaluate Model` compara os valores reais (`Vendas`) com os valores previstos (`Scored Labels`) para gerar as métricas de desempenho que determinam a qualidade do modelo.

* **Mean Absolute Error (MAE):** `7.166286`
* **Root Mean Squared Error (RMSE):** `8.486824`
* **Coefficient of Determination (R²):** `0.982384`
<img width="1558" height="755" alt="evaluetion" src="https://github.com/user-attachments/assets/d3aea12c-b019-4223-8b3d-62c7b7633f55" />

