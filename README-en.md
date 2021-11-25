### :pushpin: [__Read in Portuguese__](https://github.com/feliperastelli/FR-Rossmann_Sales_Model/blob/main/README.md)

# Rossmann Sales Model

O objetivo desse projeto é fornecer para o CFO da Rossmann Drug Stores, um **modelo de previsão de vendas** para as próximas seis semanas para que ele possa definir um orçamento específico para reformas nas lojas. O modelo de previsão atualmente utilizado não atende as necessidades da empresa, portanto, o modelo de machine learning desenvolvido nesse projeto veio como uma solução exata para esse problema de negócio.

O projeto foi desenvolvido através da técnica CRISP-DM, e ao final do primeiro ciclo de desenvolvimento foi possível produzir um modelo de previsão com indíce **MAPE Error de 9%** utilizando o algoritmo **XGBoost**.

Em termos de negócio, o resultado desse modelo de previsão pode ser resumido com os números abaixo:

| __Scenarios__ | __Values__ |
| ------------- | -----------|
| predictions	| US$ 282,662,848.00 |
| worst scenario | US$ 281,907,880.11 |
| best scenario	| US$ 283,417,771.65 |

*O worst scenario considera o erro calculado do modelo (MAE) negativamente e o best scenario, positivamente.

Para visualização do resultado da previsão de cada loja, foi construindo um bot no aplicativo Telegram, onde o usuário pode inserir o número da loja e terá o retorno da previsão calculada pelo modelo que foi colocado em produção no Heroku. Ou seja, foi realizado o deploy em produção do modelo e bot para que possam ser acessados de qualquer lugar.

Para acessar basta apenas ter o aplicativo instalado no smartphone ou PC, criar uma conta, e solicitar para o contato Bot o número da loja. Ex: '/22', '/50'. Faça o teste:

[<img alt="Telegram" src="https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white"/>]( http://t.me/vs_rossmannbot)

## 1. Sobre a Rossmann Drug Store

### 1.1 Contexto do negócio:

A Rossmann é uma das maiores redes de drogaria da Europa, com cerca de 56.200 funcionários e mais de 4000 lojas em diversos países, como Alemanha, Polônia, Hungria, República Tcheca, Turquia, Albânia, Kosovo e Espanha. É uma empresa com um grande sortimento de produtos que são oferecidos as seus clientes, incluindo produtos próprios. A companhia está em grande expansão e num ritmo elevado, com grandes investimentos.

### 1.2 Questão do negócio:

Como citado acima, o projeto foi desenvolvido a partir da necessidade do CFO da empresa ter a previsão de vendas das próximas seis semanas, para garantir e avaliar o orçamento especifico para possíveis reformas das lojas. Outrora, o mesmo não possuia um modelo de previsão e o método utilizado não era satisfatório.

### 1.3 Sobre os dados:

Os dados foram disponibilizados pela empresa na plataforma do Kaggle: https://www.kaggle.com/c/rossmann-store-sales/data

|***Atributo*** | ***Descrição*** |
| -------- | --------- |
|**Id** | an Id that represents a (Store, Date) duple within the test set |
|**Store** | a unique Id for each store |
|**Sales** | the turnover for any given day (this is what you are predicting) |
|**Customers** | the number of customers on a given day |
|**Open** | an indicator for whether the store was open: 0 = closed, 1 = open |
|**StateHoliday** | indicates a state holiday. Normally all stores, with few exceptions, are closed on state holidays. Note that all schools are closed on public holidays and weekends. a = public holiday, b = Easter holiday, c = Christmas, 0 = None |
|**SchoolHoliday** | indicates if the (Store, Date) was affected by the closure of public schools |
|**StoreType** | differentiates between 4 different store models: a, b, c, d | 
|**Assortment** | describes an assortment level: a = basic, b = extra, c = extended | 
|**CompetitionDistance** | distance in meters to the nearest competitor store | 
|**CompetitionOpenSince[Month/Year]** | gives the approximate year and month of the time the nearest competitor was opened| 
|**Promo** | indicates whether a store is running a promo on that day | 
|**Promo2** | Promo2 is a continuing and consecutive promotion for some stores: 0 = store is not participating, 1 = store is participating | 
|**Promo2Since[Year/Week]** | describes the year and calendar week when the store started participating in Promo2 | 
|**PromoInterval** | describes the consecutive intervals Promo2 is started, naming the months the promotion is started anew. E.g. "Feb,May,Aug,Nov" means each round starts in February, May, August, November of any given year for that store | 

### 1.4 Premissas do negócio:

- The days when stores were closed (Open) were removed from the analysis.
- Only stores with sales values bigger than 0 were considered.
- For stores which did not have Competition Distance information, it was considered that the distance should be the longest distance observed in the data set.

## 2. Planejamento da solução:

O projeto foi desenvolvido através do método CRISP-DM, aplicando os seguintes passos:

**Step 01 - Data Description:** Nessa etapa, o objetivo foi conhecer os dados, seus tipos, usar métricas estatísticas para identificar outliers no escopo do negócio e também analisar métricas estatísticas básicas como: média, mediana, máximo, mínimo, range, skew, kurtosis e desvio padrão. Nessa etapa também foram feitos alguns ajustes em features do dataset, como preenchimento de NA's por exemplo.

**Step 02 - Feature Engineering:** Nessa etapa, foi desenvolvido um mapa mental para analisar o fenômeno, suas variáveis e os principais aspectos que impactam cada variável. A partir das características do hipóteses e da necessidade de novos atributos, foram elevados novos recursos a partir das variáveis originais, a fim de melhorar o fenômeno do ser modelado.

**Step 03 - Data Filtering:** O objetivo desta etapa foi filtrar linhas e excluir colunas que não são relevantes para o modelo ou não fazem parte do escopo do negócio, como por exemplo, desconsiderar dias que as lojas não estavam operando e/ou que não houveram vendas.

**Step 04 - Exploratory Data Analysis:** O objetivo desta etapa foi explorar os dados para encontrar insights, entender melhor a relevância das variáveis no aprendizado do modelo. Foram feitas analises univariadas, biváriadas e multivariadas, utilizandos os dados numéricos e categóricos do conjunto.

**Step 05 - Data Preparation:** Nessa etapa,  os dados foram preparados para o inicio das aplicações de modelos de machine learning. Foram utilizadas técnicas como Rescaling e Transformation, através de encodings e nature transformation.

**Step 06 - Feature Selection:** O objetivo desta etapa foi selecionar os melhores atributos para treinar o modelo. Foi utilizado o algoritmo Boruta para fazer a seleção das variáveis, destacando as que tinham mais relevância para o fenômeno.

**Step 07 - Machine Learning Modeling:** Nessa etapa foi feito os testes e treinamento de alguns modelos de machine learning, onde foi possível comparar suas respectivas performance e feita a escolha do modelo ideal para o projeto. Inclusive foi utilizada a técnica de Cross Validation para garantir a performance real sobre os dados selecionados.

**Step 08 - Hyperparameter Fine Tunning:** Tendo a escolha do algorotimo XBoost na etapa anterior, foi feita uma analise através do método Randon Search para escolher os melhores valores para cada um dos parâmetros do modelo. Ao final dessa etapa foi possível obter os valores finais da performance do modelo.

**Step 09 - Error Translation and Interpretation:** O objetivo dessa etapa foi de fato demonstrar o resultado do projeto, onde foi possível avaliar a performance do modelo com o viés de negócio, demonstrando o resultado financeiro que pode ser esperado se aplicado o modelo desenvolvido.

**Step 10 - Deploy Model to Production:** Após execução bem sucedida do modelo, o objetivo foi publica-lo em um ambiente de nuvem para que outras pessoas ou serviços possam usar os resultados para melhorar a decisão de negócios. A plataforma de aplicativo em nuvem escolhida foi o Heroku.

**Step 11 - Telegram Bot:** A etapa final do projeto foi criar um bot no app de mensagens - Telegram, que possibilita consultar as previsões a qualquer momento e lugar, visto que também foi feito o deploy na plataforma em nuvem.

## 3. Principais insights:

**Hypothesis 1:** Stores with larger assortments should sell more.
  **False:** Stores with a larger assortment sell LESS.

![image](https://user-images.githubusercontent.com/77105763/142774353-0b11753d-f737-4cd9-ba9c-9075dc34ee0e.png)

**Hypothesis 2:** Stores should sell more over the years.
  **False:** Stores sell less over the years.

![image](https://user-images.githubusercontent.com/77105763/142774441-47a4439b-f8c3-458d-a93c-8b22f544a6ce.png)

**Hypothesis 3:** Stores should sell more in the second half of the year.
  **False:** Stores sell less in the second half of the year.
  
  ![image](https://user-images.githubusercontent.com/77105763/142774550-6be3c362-d896-46c7-a57a-3b17914b68c1.png)

*Demais insights podem ser consultados nos notebooks do projeto.*

## 4. Performance dos Modelos de Machine Learning:

O dados do projeto foram testados com modelos lineares e não lineares.Foi utilizada a estratégia de selecionar 5 tipos de modelos: Modelo de média, dois modelos lineares, e dois não-lineares. A média por exemplo serviu como base de referência. Os modelos lineares servem para avaliar a complexidade de aprendizado do conjunto de dados. Caso a performance fosse ruim, poderia entender que seria necessário um modelo mais complexo 

**- Modelos Lineares:**

   - Média
   - Linear Regression 
   - Linear Regression Regularized

**- Modelos Não Lineares:**

   - Random Forest Regressor 
   - XGBoost Regressor

**Comparação da performance dos modelos:**

***Model Name*** | ***MAE CV*** | ***MAPE CV*** | ***RMSE CV*** |
| ---------------- | ---------- | --------- | ---------- |
|Random Forest Regressor | 842.56 +/- 220.07 | 0.12 +/- 0.02	 | 1264.33 +/- 323.29 |
|XGBoost Regressor | 1048.45 +/- 172.04 | 0.14 +/- 0.02	 | 1513.27 +/- 234.33 |
|Average Model | 1354.80 | 0.45	 | 1835.13 |
|Linear Regression | 2081.73 +/- 295.63 | 0.3 +/- 0.02	 | 2952.52 +/- 468.37 |
|Lasso | 2116.38 +/- 341.5 | 0.29 +/- 0.01	 | 3057.75 +/- 504.26 |

**Performance final do modelo escolhido após Hyperparameter Fine Tuning:**

***Model Name*** | ***MAE*** | ***MAPE*** | ***RMSE*** |
| -------- | --------- | --------- | --------- |
|XGBoost Regressor | 673.394631 | 0.097298	 | 965.731681 |

## 5. Resultado final - Model performance vs Business Values

O resultado final do projeto foi satisfatório para a maior parte das lojas abrangidas nos dados, conforme gráfico abaixo (Essas lojas em específico podem conter particularidades e possivelmente num segundo ciclo desse projeto, algo poderia ser feito para melhor a performance e predição para elas).

![image](https://user-images.githubusercontent.com/77105763/143149982-0e6c1f18-3874-412a-a82f-01ff03b13c85.png)

A maior parte das lojas tiveram o erro MAPE muito próximo do erro performado no modelo - **MAPE Error de 9%**

Como indicado no resumo prévio do projeto, o resultado que pode ser obtido utilizando-se do modelo, considerando o melhor e pior cenário, é o seguinte:

| __Scenarios__ | __Values__ |
| ------------- | -----------|
| predictions	| US$ 282,662,848.00 |
| worst scenario | US$ 281,907,880.11 |
| best scenario	| US$ 283,417,771.65 |



Podemos observar o performance do modelo, avaliando a relação entre as vendas (dados de teste) e as predições:

![image](https://user-images.githubusercontent.com/77105763/143151060-c9ef9bcd-a266-4a1a-9457-e99a203d77d6.png)

## 6. Conclusão

O projeto desenvolvido foi concluído com êxito, onde foi possível projetar as vendas das próximas semanas para que o CFO tenha informações reais para criar o budget das lojas, podendo consultar em tempo real cada predição.

- O deploy do modelo desenvolvido e da aplicação do Bot do Telegram foram construídos no ambiente em nuvem do **Heroku** e estão em funcionamento.

- Toda documentação do projeto pode ser consultada no repositório, incluindo os notebooks desenvolvidos e todos os scritps finais para as aplicações web.
