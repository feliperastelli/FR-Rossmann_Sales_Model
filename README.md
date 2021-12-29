### :pushpin: [__Read in English__](https://github.com/feliperastelli/FR-Rossmann_Sales_Model/blob/main/README-en.md)

# Rossmann Sales Model

O objetivo desse projeto é fornecer para o CFO da Rossmann Drug Stores, um **modelo de previsão de vendas** para as próximas seis semanas para que ele possa definir um orçamento específico para reformas nas lojas. O modelo de previsão atualmente utilizado não atende as necessidades da empresa, portanto, o modelo de machine learning desenvolvido nesse projeto veio como uma solução exata para esse problema de negócio.

O projeto foi desenvolvido através da técnica CRISP-DM, e ao final do primeiro ciclo de desenvolvimento foi possível produzir um modelo de previsão com indíce **MAPE Error de 9%** utilizando o algoritmo **XGBoost**.

Em termos de negócio, o resultado desse modelo de previsão pode ser resumido com os números abaixo:

| __Scenarios__ | __Values__ |
| ------------- | -----------|
| predictions	| US$ 282,662,848.00 |
| worst scenario | US$ 281,907,880.11 |
| best scenario	| US$ 283,417,771.65 |

*O "worst scenario" considera o erro calculado do modelo (MAE) negativamente e o "best scenario", positivamente.

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
|**Id** | um Id que representa um (Store, Date) concatenado dentro do conjunto de teste |
|**Store** | um id único para cada loja |
|**Sales** | o volume de vendas em um determinado dia |
|**Customers** | o número de clientes em um determinado dia |
|**Open** | um indicador para saber se a loja estava aberta: 0 = fechada, 1 = aberta |
|**StateHoliday** | indica um feriado estadual. Normalmente todas as lojas, com poucas exceções, fecham nos feriados estaduais. Observe que todas as escolas fecham nos feriados e finais de semana. a = feriado, b = feriado da Páscoa, c = Natal, 0 = Nenhum |
|**SchoolHoliday** | indica se (Store, Date) foi afetada pelo fechamento de escolas públicas |
|**StoreType** | diferencia entre 4 modelos de loja diferentes: a, b, c, d | 
|**Assortment** | descreve um nível de sortimento: a = básico, b = extra, c = estendido | 
|**CompetitionDistance** | distância em metros até a loja concorrente mais próxima | 
|**CompetitionOpenSince[Month/Year]** | apresenta o ano e mês aproximados em que o concorrente mais próximo foi aberto| 
|**Promo** | indica se uma loja está fazendo uma promoção naquele dia | 
|**Promo2** | Promo2 é uma promoção contínua e consecutiva para algumas lojas: 0 = a loja não está participando, 1 = a loja está participando | 
|**Promo2Since[Year/Week]** | descreve o ano e a semana em que a loja começou a participar da Promo2 | 
|**PromoInterval** | descreve os intervalos consecutivos de início da promoção 2, nomeando os meses em que a promoção é iniciada novamente. Por exemplo. "Fev, maio, agosto, novembro" significa que cada rodada começa em fevereiro, maio, agosto, novembro de qualquer ano para aquela loja | 

### 1.4 Premissas do negócio:

- Os dias em que as lojas foram fechadas (Abertas) foram retirados da análise.
- Foram consideradas apenas lojas com valores de venda maiores que 0.
- Para as lojas que não possuíam informação de "Competition Distance", considerou-se que a distância deveria ser a maior distância observada no conjunto de dados.

## 2. Planejamento da solução:

O projeto foi desenvolvido através do método CRISP-DM, aplicando os seguintes passos:

**Passo 01 - Descrição dos dados:** Nessa etapa, o objetivo foi conhecer os dados, seus tipos, usar métricas estatísticas para identificar outliers no escopo do negócio e também analisar métricas estatísticas básicas como: média, mediana, máximo, mínimo, range, skew, kurtosis e desvio padrão. Nessa etapa também foram feitos alguns ajustes em features do dataset, como preenchimento de NA's por exemplo.

**Passo 02 - Feature Engineering:** Nessa etapa, foi desenvolvido um mapa mental para analisar o fenômeno, suas variáveis e os principais aspectos que impactam cada variável. A partir das características do hipóteses e da necessidade de novos atributos, foram elevados novos recursos a partir das variáveis originais, a fim de melhorar o fenômeno do ser modelado.

**Passo 03 - Filtragem dos dados:** O objetivo desta etapa foi filtrar linhas e excluir colunas que não são relevantes para o modelo ou não fazem parte do escopo do negócio, como por exemplo, desconsiderar dias que as lojas não estavam operando e/ou que não houveram vendas.

**Passo 04 - Análise Exploratória dos dados:** O objetivo desta etapa foi explorar os dados para encontrar insights, entender melhor a relevância das variáveis no aprendizado do modelo. Foram feitas analises univariadas, biváriadas e multivariadas, utilizandos os dados numéricos e categóricos do conjunto.

**Passo 05 - Preparação dos dados:** Nessa etapa,  os dados foram preparados para o inicio das aplicações de modelos de machine learning. Foram utilizadas técnicas como Rescaling e Transformation, através de encodings e nature transformation.

**Passo 06 - Seleção de Features:** O objetivo desta etapa foi selecionar os melhores atributos para treinar o modelo. Foi utilizado o algoritmo Boruta para fazer a seleção das variáveis, destacando as que tinham mais relevância para o fenômeno.

**Passo 07 - Modelagem de Machine Learning:** Nessa etapa foram feitos os testes e treinamento de alguns modelos de machine learning, onde foi possível comparar suas respectivas performance e feita a escolha do modelo ideal para o projeto. Inclusive foi utilizada a técnica de Cross Validation para garantir a performance real sobre os dados selecionados.

**Passo 08 - Hyperparameter Fine Tunning:** Tendo a escolha do algorotimo XBoost na etapa anterior, foi feita uma analise através do método Randon Search para escolher os melhores valores para cada um dos parâmetros do modelo. Ao final dessa etapa foi possível obter os valores finais da performance do modelo.

**Passo 09 - Tradução e interpretação de erros:** O objetivo dessa etapa foi de fato demonstrar o resultado do projeto, onde foi possível avaliar a performance do modelo com o viés de negócio, demonstrando o resultado financeiro que pode ser esperado se aplicado o modelo desenvolvido.

**Passo 10 - Deploy do modelo em produção:** Após execução bem sucedida do modelo, o objetivo foi publica-lo em um ambiente de nuvem para que outras pessoas ou serviços possam usar os resultados para melhorar a decisão de negócios. A plataforma de aplicativo em nuvem escolhida foi o Heroku.

**Passo 11 - Bot do Telegram:** A etapa final do projeto foi criar um bot no app de mensagens - Telegram, que possibilita consultar as previsões a qualquer momento e lugar, visto que também foi feito o deploy na plataforma em nuvem.

## 3. Principais insights:

**Hipótese 1:** Lojas com sortimento maior devem vender mais.
  **Falsa:** Lojas com uma variedade maior vendem MENOS.

![image](https://user-images.githubusercontent.com/77105763/142774353-0b11753d-f737-4cd9-ba9c-9075dc34ee0e.png)

**Hipótese 2:** As lojas devem vender mais ao longo dos anos.
  **Falsa:** As lojas vendem menos ao longo dos anos.

![image](https://user-images.githubusercontent.com/77105763/142774441-47a4439b-f8c3-458d-a93c-8b22f544a6ce.png)

**Hipótese 3:** Lojas devem vender mais no segundo semestre.
  **Falsa:** Lojas vendem menos no segundo semestre do ano.
  
  ![image](https://user-images.githubusercontent.com/77105763/142774550-6be3c362-d896-46c7-a57a-3b17914b68c1.png)

*Demais insights podem ser consultados nos notebooks do projeto.*

## 4. Performance dos Modelos de Machine Learning:

O dados do projeto foram testados com modelos lineares e não lineares.Foi utilizada a estratégia de selecionar 5 tipos de modelos: Modelo de média, dois modelos lineares, e dois não-lineares. A média por exemplo serviu como base de referência. Os modelos lineares servem para avaliar a complexidade de aprendizado do conjunto de dados. Caso a performance fosse ruim, poderia entender que seria necessário um modelo mais complexo. 

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





