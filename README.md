# Rossmann Sales Model

O objetivo desse projeto é fornecer para o CFO da Rossmann Drug Stores, um modelo de previsão de vendas para as próximas seis semanas para que ele possa definir um orçamento específico para reformas nas lojas. O modelo de previsão atualmente utilizado não atende as necessidades da empresa, portanto, o modelo de machine learning desenvolvido nesse projeto veio como uma solução exata para esse problema de negócio.

O projeto foi desenvolvido através da técnica CRISP-DS, e ao final do primeiro ciclo de desenvolvimento foi possível produzir um modelo de previsão com indíce MAPE Error de 9% utilizando o algoritmo XGBoost.

Em termos de negócio, o resultado desse modelo de previsão pode ser resumido com os números abaixo:

| __Scenarios__ | __Values__ |
| ------------- | -----------|
| predictions	| US$ 282,662,848.00 |
| worst scenario | US$ 281,907,880.11 |
| best scenario	| US$ 283,417,771.65 |

*O worst scenario considera o erro calculado do modelo (MAE) negativamente e o best scenario, positivamente.

Para visualização do resultado da previsão de cada loja, foi construindo um bot no aplicativo Telegram, onde o usuário pode inserir o número da loja e terá o retorno da previsão calculada pelo modelo que foi colocado em produção no Heroku. Ou seja, foi realizado o deploy em produção do modelo e bot para que possam ser acessados de qualquer lugar.

Para acessar basta apenas ter o aplicativo instalado no smartphone ou PC, criar uma conta, e solicitar para o contato Bot o número da loja. Ex: '/22', '/50'. Faça o teste:

[<img alt="Telegram" src="https://img.shields.io/badge/Telegram-2CA5E0?style=for-the-badge&logo=telegram&logoColor=white"/>]( http://t.me/rossmann_fr_bot)
