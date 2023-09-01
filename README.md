# Desafio Data Science - Lighthouse by Indicium 2023
Repositório para o projeto final da trilha de Data Scientist do programa Lighthouse da Indicium.
# Objetivo

Este projeto consiste na análise de uma série temporal econômica do PIB mundial. O desafio teve como objetivo o desenvolvimento de uma EDA e a resolução de problemas em análises de dados para a aplicação de modelos preditivos para séries temporais para prever o crescimento do índice GDP de cada país nos anos de 2024 a 2028. Foi necessário também substituir os campos 'no data' utilizando algumas inferências.

- Os valores numéricos de GDP são valores percentuais;
- O dataset do desafio possui variáveis com a informação 'no data'.
- O dataset possui valores de GDP para 2024 a 2028, resultados de uma previsão de série temporal cujo método é desconhecido. 

# Estruturação


- **Coleta de Dados:** Importar os dados em formato Excel.

## Pré Processamento
Sequência disponível no Notebook `v2_preprocessing` disponível em  [Notebooks/Pre Processamento/](https://github.com/rauenh/desafio_ds_indicium/tree/main/Notebooks/Pre%20Processamento)

- **Compreensão do Problema:** Analisar o conjunto de dados para entender a sua distribuição, identificar valores ausentes, detectar possíveis outliers, etc. Foi realizada uma breve pesquisa geopolítica (WorldBank, IMF) para entender como poderiam ser agrupados os países de acordo com as regiões existentes nas colunas do dataframe de acordo com algumas características econômicas similares. 

- **Análise Gráfica:** Representar graficamente os países em gráficos de linha para visualizar as tendências temporais, distribuição dos dados e possíveis outliers. A análise será realizada por grupos de países, verificando se o agrupamento realizado previamente era coerente com o comportamento de seus valores para GDP e média móvel.

- **Análise Estatística:**  É realizada uma análise estatística descritiva básica com o intuito de entender as características dos dados desta série temporal. Eles ajudam a entender a variação, a tendência média e os extremos nos resultados econômicos dessas nações. 

- **Tratamento de Valores Ausentes:** Substituir os valores 'no data' por valores nulos, utilizando a função np.nan. Observa-se que 62 países/regiões possuem valores ausentes, variando de 1 a 32 linhas. 

- **Inputar os dados faltantes:** Após o tratamento de valores ausentes por valores nulos procedeu-se a inputação dos dados faltantes. Como existem países com quantidades variadas de dados faltantes vamos adotar algumas estratégias para a inputação dos dados. 
1. Paises com até 5 dados faltantes, completar com a média do próprio país (Completar com a Média D-1)
2. Países que pertenciam ao regime da URSS, possuem comportamento econômico similar. Fazer o input do grupo.
3. Plotar para as demais regiões: Realiza-se a média móvel do grupo para um determinado período de ano (procurando não considerar vales de crise como 2008 ou 2020). Compara o crescimento do país com a média móvel do grupo: Se crescimento for acima da média móvel, o input dos dados ocorre somando o valor da média móvel com a variancia daquele pais; Se crescimento for abaixo da média móvel, o input dos dados ocorre subtraindo-se o valor da média móvel com a variância daquele país. 

O output desse notebook é o arquivo `df_transposed_copy_inputting_v19.csv` que pode ser encontrado na pasta Data/Output

## EDA
Sequência disponível no Notebook `v2_eda_final` disponível em [Notebooks/EDA](https://github.com/rauenh/desafio_ds_indicium/tree/main/Notebooks/EDA)

- **Decomposição Sazonal:** Realizar uma decomposição sazonal dos dados de PIB de cada país para entender a sazonalidade e tendência temporal. A biblioteca statsmodels será utilizada para esse propósito. Foram realizados decomposições com dois períodos: 1 ano e 4 anos e hipóteses foram estabelecidas para determinar a diferença nos resultados obtidos.

- Como os dados tratam a respeito de GDP (PIB), o período de 1 ano pode, realmente, não ser o mais apropriado para a análise. Como trata-se de dados econômicos de crescimento do PIB de países e grupos de países as variáveis que interferem nas questões econômicas podem depender caso a caso.
- Como se investiga a ocorrência de sazonalidade e trend para os dados de GDP, foi escolhido como período para uma decomposição sazonal o valor 4, uma vez que nossos dados são anuais e, no geral, o ciclo político de um país é de 4 anos, sendo este o fator escolhido para realizar a investigação. 

- **Teste de Autocorrelação (ACF):** O teste de autocorrelação (ACF) é uma ferramenta útil para analisar séries temporais. Ele mede a correlação entre os valores de uma série temporal em diferentes pontos no tempo. O gráfico de ACF mostra a autocorrelação para diferentes defasagens (lags) no tempo. Cada barra no gráfico representa a correlação entre os valores da série temporal separados por um determinado número de períodos de tempo. Observa-se que o teste de ACF é útil para entender a autocorrelação presente nos dados, porém não é uma abordagem direta para determinar a estacionariedade das séries temporais, uma vez que observa-se os padrões de autocorrelação em diferentes defasagens (lags). 

Para confirmar a estacionariedade de uma série temporal será utilizado dois testes estatísticos específicos, como o teste Augmented Dickey-Fuller (ADF) e o teste Kwiatkowski-Phillips-Schmidt-Shin (KPSS). Esses testes fornecem uma análise mais formal da estacionariedade, levando em consideração fatores como tendência e sazonalidade.

- **Teste de Estacionariedade ADF:** O teste de Dickey-Fuller Aumentado (ADF) é uma ferramenta estatística comum usada para testar a estacionariedade de uma série temporal. A estacionariedade é uma propriedade importante das séries temporais, pois muitos modelos de previsão assumem que a série temporal é estacionária. Se uma série temporal não é estacionária, pode ser necessário transformá-la antes de ajustar um modelo de previsão. Com o teste de ADF podemos observar que o conjunto de dados possui aproximadamente 82% de séries temporais estacionárias e 17.98% de séries não-estacionárias. 

- **Análise comparativa entre ADF e KPSS:** É realizada uma análise comparativa da estacionariedade de séries temporais usando os testes ADF (Augmented Dickey-Fuller) e KPSS (Kwiatkowski-Phillips-Schmidt-Shin) com o intuito de metrificar qual era o teste mais adequado para se confirmar a estacionariedade ou não estacionariedade dos dados. 

## Modelagem
Sequência disponível nos Notebooks:
- `v2_modeling_fb_prophet_all_countries`
- `v2_modeling_sarima`
- `v2_modeling_hw`
Todos disponíveis no caminho: [Notebooks/Modelagem](https://github.com/rauenh/desafio_ds_indicium/tree/main/Notebooks/Modelagem)

Descrição resumida dos notebooks:

- No notebook `v2_modeling_hw` há a modelagem dos dados com os modelos Holt-Winters e Simple Exponential Smoothing
- No notebook `v2_modeling_sarima` há a modelagem dos dados com os modelos Autoarim e SARIMAX (Com variáveis exógenas e sem variáveis exógenas)
- No notebook `v2_modeling_fb_prophet_all_countries` há a modelagem dos dados com o modelo Prophet.
Cada modelo será treinado com dados de 1980 a 2023 e testado com dados de 2024 a 2028.
## Performance dos Modelos
Sequência disponível no notebook `v2_modeling_final_metrics` disponível em [Notebooks/Modelagem](https://github.com/rauenh/desafio_ds_indicium/tree/main/Notebooks/Modelagem) 

**Métricas de Performance:** As métricas MAE (Erro Absoluto Médio), RMSE (Raiz do Erro Quadrático Médio) e MAPE (Erro Percentual Absoluto Médio) são importantes para avaliar o desempenho de um modelo de ciência de dados. Elas medem a diferença entre os valores previstos pelo modelo e os valores reais observados. <br/><br/> - O MAE mede a magnitude média dos erros, enquanto o RMSE dá mais peso aos erros grandes, pois eleva ao quadrado as diferenças antes de calcular a média. <br/><br/> - O MAPE mede o erro em termos percentuais, o que pode ser útil quando os valores observados variam em magnitude. <br/><br/> Ao comparar diferentes modelos, é importante escolher aquele que apresenta o menor valor para essas métricas, indicando que suas previsões são mais precisas.
- No notebook v2_modeling_final_metrics há o cross validation dos modelos que melhor performaram (SARIMAX e Prophet) para haver a escolha por um modelo final para a realização da predição. Para o cross-validation do modelo SARIMAX foi utilizado a ferramenta  `TimeSeriesSplit` do `sklearn`. Para o cross validation do modelo Prophet foi utilizada a ferramenta `cross_validation` do `prophet`

### Previsão
O arquivo com os valores previstos pelo modelo foi nomeado de `predicted.csv`. E foi salvo na pasta [Data/Output](https://github.com/rauenh/desafio_ds_indicium/tree/main/Projeto/Data/Output)

O arquivo com a modelagem de previsão está disponível na pasta [Predict](https://github.com/rauenh/desafio_ds_indicium/tree/main/Notebooks/Predict)
# Como executar o projeto

Para executar o projeto é necessário seguir os seguintes passos:

1. Clonar o repositório

`git clone https://github.com/rauenh/desafio_ds_indicium.git`

2. Criar um ambiente virtual

`python -m venv venv`

3. Ativar o ambiente virtual

`source venv/bin/activate`

4. Instalar as dependências

`pip install -r requirements.txt`

5. Executar o jupyter notebook

`jupyter notebook`

 - Para executar o preenchimento dos dados faltantes e a modelagem dos dados, executar os arquivos 
 <ul>

 - [v2_preprocessing.ipynb](https://github.com/rauenh/desafio_ds_indicium/blob/main/Notebooks/Pre%20Processamento/v2_preprocessing.ipynb)
    - como saída o arquivo csv `df_transposed_copy_inputting_v19.csv` é gerado com dados dos países e regiões com valores nulos preenchidos.
 - [v2_eda_final.ipynb](https://github.com/rauenh/desafio_ds_indicium/blob/main/Notebooks/EDA/v2_eda_final.ipynb)
    - EDA complementar dos dados.

</ul>

- Para executar a previsão dos dados, executar o arquivo
<ul>

- [predict_model_sarimax.ipynb](https://github.com/rauenh/desafio_ds_indicium/blob/main/Notebooks/Predict/predict_model_sarimax.ipynb)
    - como saída o arquivo csv `predicted.csv` é gerado com os valores previstos pelo modelo.
</ul>

# Ferramentas utilizadas

- Jupiter Notebook
- Pandas
- [Statsmodels](https://www.statsmodels.org/stable/index.html)
- [Facebook Prophet](https://facebook.github.io/prophet/)
- Scikit Learn
- Git
- Github
