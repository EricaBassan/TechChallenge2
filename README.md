
<h1 align="center">Previsão Bolsa de Valores</h1>
<h4 align="center">Repositório criado para o tech challenge 2 do curso de data analytics da FIAP</h4>

<p align="center">
  <a href="">
    <img src="Imagens\Stock_figure.png"
         alt="version">
  </a>

</p>




## Sumário

- [Sobre](#sobre)
- [Bibliotecas](#bibliotecas)
- [Analise exploratoria de dados (EDA)](#analise-exploratoria-de-dados-eda)
- [Métodos aplicados](#métodos-aplicados)
- [Modelos preditivos](#modelos-preditivos)
- [Resultados](#resultados)
  


# Sobre
Este projeto foi criado para atender os requisitos do projeto Tech Challenge da [Faculdade de Tecnologia - FIAP](https://postech.fiap.com.br/?gclid=Cj0KCQjwnf-kBhCnARIsAFlg49228y9z3y6lf_mWZEekgcxZRZBDavxtRT-zAUNs33TZOJtXpGVMNlAaAue5EALw_wcB).<br>
O Tech Challenge foi divido em três etapas:
- Aplicação de um modelo de machine learning para projetar os valores de fechamento da bolsa de valores (Ibovespa).
- Atinigir uma acurácia maior do que 70% com o modelo.
- Justificar métodos realizados e os resultados obtidos.

# Bibliotecas

Foram utilizadas as seguintes bibliotecas para o desenvolvimento do projeto:

- [Pandas](https://pandas.pydata.org/);

- [Numpy](https://numpy.org/);

- [Statsmodels](https://www.statsmodels.org/stable/index.html);

- [Matplotlib](https://matplotlib.org/);

- [Seaborn](https://seaborn.pydata.org/);

- [Pmdarima](https://pypi.org/project/pmdarima/);

- [Prophet](https://pypi.org/project/prophet/);

- [Sklearn](https://scikit-learn.org/stable/).

# Analise exploratoria de dados (EDA)

Iniciamos o projeto com a análise dos dados.

Exportamos uma base de dados histórica dos últimos 10 anos da [Ibovespa](https://br.investing.com/indices/bovespa-historical-data), com o valor do fechamento mensal, para avaliar o comportamento dos dados.

Através do gráfico obtido (figura 1), podemos observar que antes de 2021, os valores tinham um comportamento que evidenciava uma certa tendencia a crescimento do fechamento. 

Após o ano de 2021, o gráfico começa a apresentar um comportamento diferente, em comparação com o passado, que vem se mantendo até presente data.

Somando essa observação com o fato de que ambos os contextos geopolitico e economico atuais são bem divergentes dos anos anteriores, consideramos que os valores anteriores a 2021 poderiam levar o modelo a uma tendencia incorreta da situação atual da bolsa de valores.

Dessa forma, para o nosso modelo estamos utilizando uma base histórica diária do mercado de ações, com valores que se iniciam em jan/21 - até presente data.


<p align="center">
  <a href=" ">
    <img src="Imagens\Fechamento Ibovespa últimos 10 anos.PNG" alt="webapp-architecture">
  </a>
  <caption style="font-size: smaller; text-align: center;">Figura 1</caption>
</p>


Prosseguindo com a analise exploratória, criamos mais um gráfico com os valores diários da Ibovespa de jan/21 até ago/23, para analisar o comportamento dos valores (figura 2).

Observando o gráfico não encontramos uma tendencia evidente dos dados.

<p align="center">
  <a href=" ">
    <img src="Imagens\Fechamento Ibovespa-2021-2023.PNG" alt="webapp-architecture">
  </a>
  <caption style="font-size: smaller;">Figura 2</caption>
</p>

Assim, criamos um gráfico com a média e o desvio padrão junto dos dados originais (figura 3);

<p align="center">
  <a href=" ">
   <img src="Imagens\Fechamento-media-desvio-padrao.PNG" alt="webapp-architecture">
  </a>
  <caption style="font-size: smaller;"> Figura 3</caption>
</p>

O próximo gráfico é um histograma, indicando a distribuição dos dados (figura 4);


<p align="center">
  <a href=" ">
    <img src="Imagens\Histrograma-ibovespa.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 4</caption>
</p>

E por fim, temos um gráfico analisando a % de variancia acumulada das ações (figura 5), indicando o desempenho do mercado nos ultimos anos:

<p align="center">
  <a href=" ">
    <img src="Imagens\Acumulo-variacao-porcental-ibovespa.PNG">
   </a>
  <caption style="font-size: smaller;">Figura 5</caption>
</p>

# Métodos aplicados

Após definido o período de interesse, fizemos a decomposição dos dados afim de determinar os seguintes pontos:

- Tendencia (direção de uma série temporal ao longo do tempo);

- Sazonalidade (fenômenos que ocorrem durante o tempo e se repetem a cada período idêntico de tempo);

- Resíduo (variação aleatória de uma série temporal, que não pode ser explicada por algum componente. Os ruídos podem ocorrer definindo fatores externos imprevisíveis ou por erros de medições).

A figura 6 demonstra essa decomposição.


<p align="center">
  <a href="">
    <img src="Imagens\decomposicao-dos-dados.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 6</caption>
</p>

Podemos observar que os dados não apresentam uma tendência explicita, porém para determinar se de fato nossa série temporal é estacionária, precisamos realizar o teste de Dickey-Fuller.

Consideramos uma série estacionária quando a mesma apresenta propriedade estatísticas estáveis ao longo do tempo (média e variância).

Para determinados modelos, como por exemplo o ARIMA, é necessário que a série temporal seja estacionária. 

O teste de Dickey-Fuller apresenta parametros que servem para determinamos se nossa série é estacionária ou não. No caso, o teste revelou que nossa série era não estacionária (figura 7), pois o p-value apresentado era acima de 0.05 e os valores do teste estatístico era maior do que os valores críticos.

<p align="center">
  <a href="">
    <img src="Imagens\Resultado-primeiro-teste-adf.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 7</caption>
</p>

Assim, era necessário que transformassemos essa série em estacionária para podermos aplicar o modelo ARIMA.

Para tanto, aplicamos o método da diferenciação (figura 8):

<p align="center">
  <a href="">
    <img src="Imagens\dados-estacionarios-primeira-diferenciacao-aplicada.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 8</caption>
</p>

Depois, incluímos a média e o desvio padrão para analisar o novo comportamento dos dados (figura 9):

<p align="center">
  <a href="">
    <img src="Imagens\dados-estacionarios-media-desvio-padrao.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 9</caption>
</p>

Depois, aplicamos novamente o teste de Dickey-Fuller para verificar se a base tinha se tornado estacionária (figura 10):

<p align="center">
  <a href="">
    <img src="Imagens\Resultado-segundo-teste-adf.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 10</caption>
</p>

E conseguimos provar pelo p-value=0.0 e pelo valor do teste estatístico, que a série tinha se tornado estacionária.

Aplicamos as funções de autocorrelação na série (figura 11), e os gráficos ACF (figura 12) E PACF (figura 13):

<p align="center">
  <a href="">
    <img src="Imagens\graficos-autocorrelacao.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 11</caption>
</p>

<p align="center">
  <a href="">
    <img src="Imagens\grafico-acf.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 12</caption>
</p>


<p align="center">
  <a href="">
    <img src="Imagens\grafico-pacf.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 13</caption>
</p>

A autocorrelação (ACF) se dá quando comparamos os valores do presente com valores do passado de uma mesma série.

A diferença entre a autocorrelação e a autocorrelação parcial (PACF) é quase um detalhe: em uma ACF temos a correlação direta e indireta e em uma PACF apenas a correlação direta.

Exemplificando, com a ACF vemos a correlação direta do mês de janeiro em março e também a correlação indireta que o mês de janeiro teve em fevereiro que também teve em março.

Depois de estudarmos a estacionariedade da série, optamos por utilizar a base original, pois ela não exibe uma tendência de longo prazo. 

Isso implica que não há uma direção clara de aumento ou diminuição dos preços ao longo do tempo.
Dessa forma, divimos a série entre a base de treino e a base de teste para aplicação dos modelos.


# Modelos preditivos

O primeiro modelo aplicado foi o ARIMA (figura 14).

O modelo ARIMA (Autoregressive Integrated Moving Average) é uma técnica de previsão de séries temporais. Ele é projetado para modelar e prever séries temporais que exibem comportamento estacionário.

Ele é composto por três principais componentes:

- Componente Auto-Regressivo (AR - Autoregressive): Este componente modela a relação entre o valor atual da série temporal e seus valores passados. 

- Componente de Média Móvel (MA - Moving Average): Este componente modela a relação entre o valor atual da série temporal e os erros de previsão passados. 

- Componente de Integração (I - Integrated): A parte "Integrada" do ARIMA se refere ao número de vezes que a série temporal precisa ser diferenciada para torná-la estacionária. 

Podemos perceber que a previsão dada pelo modelo foi uma linha contínua.



<p align="center">
  <a href="">
    <img src="Imagens\grafico-previsao-arima.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 14</caption>
</p>


O segundo modelo utilizado foi o Prophet.

O Prophet é uma biblioteca desenvolvida pelo Facebook para a previsão de séries temporais. 

Ao contrário da previsão do ARIMA, o prophet (figura 15) foi capaz de prever os dados levando em conta a sazonalidade dos dados, como feriados e tendencias não lineares.


<p align="center">
  <a href="">
    <img src="Imagens\previsao-prophet.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 15</caption>
</p>

Analisando a comparação entre os dados reais (base de validação) e a previsão realizada pelo Prophet (figura 16), podemos ver que por vezes, os valores reais fogem do intervalo de confiança da predisão do modelo.



<p align="center">
  <a href="">
    <img src="Imagens\valores-reais-previsao-prophet.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 16</caption>
</p>

Assim, para definir qual modelo performou melhor, iniciamos a avaliação de performance dos modelos.

# Resultados

Primeiramente, calculamos a performance dos modelos através dos parametros abaixos:

- MAE (Mean Absolute Error - Erro Médio Absoluto):
Ele mede o erro médio absoluto, ou seja, o quão distantes as previsões estão, em média, dos valores reais.

- MSE (Mean Squared Error - Erro Médio Quadrático):
Ele penaliza erros maiores de forma mais significativa do que o MAE devido à natureza quadrática das diferenças.

- RMSE (Root Mean Squared Error - Raiz do Erro Médio Quadrático):
Ele é útil para interpretar o erro de previsão em uma escala semelhante aos valores reais.


- MAPE (Mean Absolute Percentage Error - Erro Médio Percentual Absoluto):
Ele fornece uma medida de erro como uma porcentagem da magnitude dos valores reais.


- WMAPE (Weighted Mean Absolute Percentage Error - Erro Médio Percentual Absoluto Ponderado):
É útil quando você deseja dar mais importância a certos pontos de dados em sua avaliação.

Para o modelo arima, primeiro calculamos o MAE,MSE, RMSE e MAPE (figura 17), e depois calculamos o WMAPE (figura 18):

<p align="center">
  <a href="">
    <img src="Imagens\performance-arima.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 17</caption>
</p>


<p align="center">
  <a href="">
    <img src="Imagens\wmape-arima.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 18</caption>
</p>

E fizemos os mesmos cáculos para o modelo Prophet:

<p align="center">
  <a href="">
    <img src="Imagens\performance-prophet.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 19</caption>
</p>


<p align="center">
  <a href="">
    <img src="Imagens\wmape-prophet.PNG">
  </a>
  <caption style="font-size: smaller;">Figura 20</caption>
</p>

Com isso, finalizamos indicando que no Arima, em média, as previsões têm um erro absoluto de cerca de 6.49 unidades em relação aos valores reais. Enquanto que no Prophet, esse valor sobe para 9.94.
Além disso, o erro absoluto do Arima foi menor do que o Prophet.


