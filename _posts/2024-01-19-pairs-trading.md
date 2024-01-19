# Pairs Trading: uma arbitragem casual

Para começar a série de posts voltados a estratégias quantitativas para o mercado financeiro, utilizando python, trago para vocês o **Pairs Trading**.

O Pairs Trading é um tipo de estratégia que se enquadra na classe das arbitragens. Para essa estratégia iremos utilizar dois ativos e operar Long & Short neles simultaneamente, buscando um retorno frente ao spread desses ativos. Para uma maior especificidade no caso, é aconcelhado utilizar ativos que estejam no mesmo setor.

## Mão na massa

> O notebook para esse estudo está neste [link](https://github.com/gabrielp18/quant_notebooks/tree/main).

### Buscando ativos
Primeiro passo devemos importar os dados dos ativos que iremos utilizar, para esse caso: `EGIE3` e `TRPL4`, dois ativos do setor elétrico!

~~~python
ativo1, ativo2, ativo1_relativo, ativo2_relativo = baixando_dados(inicio = '2020-01-01', fim = '2022-12-31', tick1 = 'TRPL4.SA', tick2 = 'EGIE3.SA')
~~~

Podemos então visualizar o movimento do preço desses ativos e observar que em certos períodos notamos correlação entre elas, mas há momentos que não. Nesses momentos onde não há correlação, geralmente, os preços divergem em sua tendência e podemos então abrir nossas posições de long & short simultaneamente.

<p align='center'><img src="/images/ativos_arbitragem.png"/></p>

### Testando a cointegração
A verificação de cointegração pode ser feito utilizando os teste de Johansen, Engle-Granger e Phillips-Ouliaris. Nessa nosso caso iremos realizar essa validação utilizando os resíduos da regressão linear entre os ativos, por meio do teste de Dickey-Fuller aumentado (ADF), segundo [Eagle and Granger (1987)](https://www.scirp.org/reference/referencespapers?referenceid=1743628). 

O ponto principal da realização de um teste ADF na estratégia é garantir se o par de ativos são estacionários ou não. Assim, para que o par de ativos seja negociado na estratégia de pairs trading, é necessário que a série temporal seja estacionária. Uma série temporal estacionária faz previsões eficazes e precisas. Além disso, uma série temporal estacionária significa que o par de ativos são cointegrados e pode ser negociado em conjunto através da geração de sinais.

$$
\begin{aligned}
H_{0} & : \phi =\ 1\ \implies y_{t} \sim I(0) \ | \ (raiz unitária) \\
H_{1} & : |\phi| <\ 1\ \implies y_{t} \sim I(0) \ | \ (estacionária)  \\
\end{aligned}
$$

A primeira parte então é realizar nossa regressão, nesse caso uma OLS, e obter os resíduos dessa modelagem.

~~~python
alpha, beta, errors = gerando_residuos(dados_y = ativo2['Close'], dados_x = ativo1['Close'])
~~~

<p align='center'><img src="/images/residuos.png"/></p>

A partir desses resíduos, ou spread, podemos então realizar o teste de Dickey-Fuller e validar se esses ativos possuem, realmente, uma semelhança estatística forte através da cointegração.

~~~python
teste_adfuller(dados = errors, lag = 1, ativo2 = ativo2, ativo1 = ativo1)
~~~

<p align='center'><img src="/images/ad.png"/></p>

Confirmado nossa tese de que esses ativos são cointegrados, ou seja, apresentam uma forte relação estatística entre si. Podemos agora criar o zscore baseado neles.

~~~python
zscore = gerando_zscore(dados = errors)
~~~

Com isso podemos delimitar 2 zonas que serviram para o monitoramento da entrada e saída das posições de trade entre esses ativos.

<p align='center'><img src="/images/zscore.png"/></p>

Por fim, definimos nossa estratégia e realizamos nosso backtest para avaliar o desempnho da mesma

~~~python
btest = algo_arbitragem(entrada = 1, saida = 0, ativo1 = ativo1, ativo2 = ativo2, zscore = zscore)

backtest_result = backtest_arbitragem(dados = btest)
~~~

<p align='center'><img src="/images/backtest.png"/></p>

Na imagem podemos observar o backtest dessa estratégia frente ao retorno acumulado dos dois ativos isolados e do benchmark IBOVESPA.

---------------------------------------------------
<h1 align='center'><i>Stay hungry!</i></h1>
<p align='center'><img src="/images/e25f7dc63a9bf9389f194ae4d2fcb02b.gif" width="400"/></p>