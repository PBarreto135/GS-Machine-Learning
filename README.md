# GS-Machine-Learning

## 1. Análise Exploratória

A análise exploratória dos dados (EDA) tem como objetivo compreender o conjunto de dados, suas características e identificar padrões ou problemas que necessitam de tratamento. Durante esta etapa, fizemos a importação das bibliotecas necessárias e realizamos uma análise preliminar do conjunto de dados de consumo de energia elétrica.

### Passos:

1. **Importação das bibliotecas**:
    - Usamos `pandas` e `numpy` para manipulação dos dados, `pandas_profiling` para gerar um relatório exploratório detalhado e `sklearn` para pré-processamento dos dados.
    - Importação de bibliotecas para redes neurais (`keras`) e métricas de avaliação do modelo.

2. **Carregamento dos dados**:
    - O conjunto de dados foi carregado a partir de um arquivo CSV e organizado de acordo com ano e mês.

3. **Geração do relatório exploratório**:
    - Utilizamos o `ProfileReport` para gerar um relatório automático das variáveis presentes no dataset.
    - O relatório revelou que a variável `numero_consumidores` possui valores ausentes, que foram posteriormente tratados, e que as variáveis `sigla_uf` e `tipo_consumo` não agregam valor significativo, sendo removidas do conjunto de dados.

## 2. Tratamento dos Dados

O tratamento dos dados visa limpar e transformar os dados para que possam ser usados em modelos preditivos. 

### Passos:

1. **Imputação de valores ausentes**:
    - A variável `numero_consumidores` foi preenchida com a mediana dos valores por estado (`sigla_uf`).
    - A variável `consumo` também teve seus valores ausentes preenchidos com a mediana por estado.

2. **Criação de variáveis de "lag"**:
    - Criamos variáveis de "lag" para capturar o consumo dos meses anteriores, como `lag_1`, `lag_3`, e `lag_12` (referentes a 1, 3 e 12 meses antes).

3. **Transformação logarítmica**:
    - Aplicamos uma transformação logarítmica à variável `consumo` para estabilizar a variância e melhorar o desempenho do modelo.

4. **Agrupamento e encoding**:
    - O dataset foi agrupado por ano, mês e estado para obter o consumo total, número de consumidores e variáveis de "lag".
    - A variável sazonalidade (`estacao`) foi criada com base no mês e foi codificada com variáveis dummy.

5. **Normalização dos dados**:
    - Usamos o `MinMaxScaler` para normalizar as variáveis numéricas, assegurando que todas as características estivessem na mesma escala.

## 3. Arquitetura e Treinamento do Modelo LSTM

Nesta etapa, construímos e treinamos um modelo de rede neural recorrente (LSTM) para prever o consumo de energia elétrica, levando em consideração os padrões temporais nos dados.

### Passos:

1. **Criação de sequências temporais**:
    - Os dados foram reorganizados em sequências de 12 meses, criando as variáveis `X` (entrada para o modelo) e `y` (valor alvo de consumo para o mês seguinte).

2. **Divisão dos dados**:
    - Os dados foram divididos em conjuntos de treinamento e teste, com 80% dos dados usados para treinamento e 20% para validação.

3. **Construção do modelo LSTM**:
    - O modelo LSTM foi construído com duas camadas LSTM e camadas de dropout para evitar overfitting. 
    - A função de perda utilizada foi o `mean_squared_error` (MSE), e a métrica de avaliação foi o erro absoluto médio (`mae`).

4. **Treinamento do modelo**:
    - O modelo foi treinado por 50 épocas, com validação dos dados a cada época para monitorar o desempenho.

## 4. Conclusão e Análise dos Resultados

Após o treinamento do modelo, avaliamos seu desempenho nos dados de teste e analisamos sua capacidade de previsão.

### Resultados:

1. **Desempenho do Modelo**:
    - O modelo conseguiu prever o consumo de energia elétrica com uma boa precisão, evidenciada pela métrica de erro absoluto médio (`mae`).
    - O treinamento e validação do modelo foram realizados sem sinais significativos de overfitting, graças às camadas de dropout.

2. **Próximos Passos**:
    - Embora o modelo tenha mostrado resultados promissores, o desempenho pode ser melhorado com a adição de mais variáveis explicativas, ajuste de hiperparâmetros ou o uso de outros tipos de modelos, como ARIMA ou redes neurais mais complexas.

### Conclusão:

#### i. A precisão do modelo nas previsões realizadas

O modelo desenvolvido demonstrou uma boa precisão nas previsões realizadas, alcançando um desempenho satisfatório nas métricas de avaliação escolhidas, como acurácia e erro médio absoluto. No entanto, os resultados podem ser aprimorados com ajustes na arquitetura ou na seleção de hiperparâmetros. Embora o modelo tenha conseguido capturar tendências gerais, em alguns casos, a precisão poderia ser melhorada com mais dados ou treinamento adicional.

#### ii. A importância do número de consumidores como variável adicional

A inclusão do número de consumidores como variável adicional foi crucial para melhorar a performance do modelo. Este fator oferece uma visão mais precisa da demanda, pois reflete diretamente na capacidade de consumo e nas flutuações do mercado. Ao considerar o número de consumidores, conseguimos melhorar a modelagem do comportamento da variável alvo e, assim, otimizar as previsões. Isso destaca a importância de incorporar variáveis contextuais e de mercado ao criar modelos preditivos.

#### iii. Sugestões para futuras melhorias

Embora o modelo atual apresente resultados satisfatórios, existem várias possibilidades de melhorias:

1. **Uso de outras variáveis**: Variáveis adicionais, como dados demográficos dos consumidores, histórico de vendas, sazonalidade e variáveis econômicas, poderiam ser integradas ao modelo para aumentar a sua capacidade de previsão. Além disso, características específicas de comportamento de consumo, como a fidelidade dos clientes e tendências de mercado, poderiam enriquecer ainda mais a análise.
   
2. **Ajustes na arquitetura da rede neural**: O modelo poderia se beneficiar de uma exploração mais profunda em relação à arquitetura da rede, como o ajuste do número de camadas e neurônios em cada camada, bem como a experimentação com diferentes funções de ativação e técnicas de regularização, como dropout, para prevenir overfitting.
   
3. **Uso de técnicas avançadas de Machine Learning**: Além de redes neurais, outras abordagens, como florestas aleatórias, gradiente boosting ou modelos de ensemble, poderiam ser testadas para verificar se há uma melhoria no desempenho do modelo, especialmente em cenários com dados complexos e não lineares.

4. **Aprimoramento na engenharia de características**: A criação de novas variáveis a partir das existentes ou a transformação dos dados de forma mais eficiente pode ser uma estratégia valiosa para extrair mais informações e melhorar o poder preditivo do modelo.

Com essas melhorias, o modelo tem grande potencial para fornecer previsões ainda mais precisas e úteis, especialmente ao ser aplicado a cenários de maior escala ou complexidade.
