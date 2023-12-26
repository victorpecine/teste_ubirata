![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)

![Python](https://img.shields.io/badge/python-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54) ![Pandas](https://img.shields.io/badge/pandas-%23150458.svg?style=for-the-badge&logo=pandas&logoColor=white) ![NumPy](https://img.shields.io/badge/numpy-%23013243.svg?style=for-the-badge&logo=numpy&logoColor=white) ![Plotly](https://img.shields.io/badge/Plotly-%233F4F75.svg?style=for-the-badge&logo=plotly&logoColor=white) ![scikit-learn](https://img.shields.io/badge/scikit--learn-%23F7931E.svg?style=for-the-badge&logo=scikit-learn&logoColor=white) ![SciPy](https://img.shields.io/badge/SciPy-%230C55A5.svg?style=for-the-badge&logo=scipy&logoColor=%white)


# Objetivos

1. Realizar a exploração do arquivo “dataset_tch.csv” com análises estatísticas das variáveis, gráficos de distribuição, etc.
2. Desenvolver um modelo capaz de prever o TCH dos blocos para a safra de 2019
3. Utilizar métricas de erro para avaliar o modelo

# Metodologia

1. Análise exploratória e estatística dos dados
2. Treino e teste de modelos com os dados disponíveis das safras de 2014 a 2018
- XGBoost Regressor
- Random Forest com RandomizedSearchCV
- Random Forest com BayesSearchCV
3. Análise das métricas e definição do modelo com melhor performance
4. Estimativa da TCH (toneladas de cana por hectare) na safra de 2019 com o modelo escolhido

# Dataset
**1906 rows × 18 columns**
- bloco: índice da região de plantação e colheita de cana-de-açúcar
- talhao: índice da sub-região de plantação e colheita de cana-de-açúcar (Um bloco contém diversos talhões)
- area: área do talhão
- safra: ano que a cana-de-açúcar de cada talhão foi colhida
- data_colheita: data em que a cana-de-açúcar foi colhida
- TCH: Toneladas de cana-de-açúcar colhida por hectare
- NDVI_b01: NDVI é o nome dado a um popular índice de vegetação, e o “b01” corresponde ao índice no primeiro mês antes da colheita
- NDVI_bN: NDVI no N-ésimo mês antes da colheita

# Desenvolvimento
## Análise exploratória e estatística dos dados
- Criação de nova feature ‘mil_tonelada_cana’ aqui referenciada como produção total
- A multiplicação da área pela respectiva TCH resultou no total de cana produzido em cada talhão
- A divisão desses resultados por 1.000 foi colocado no dataset com a identificação ‘‘mil_tonelada_cana’ ’
- Plot da distribuição dos NDVI_b1 a NDVI_b12 com cálculo de média e mediana
- Identificação dos valores únicos de safra
- Tipo dos dado em cada coluna do dataset
- Levantamento dos valores NaN
- Divisão do dataset com dados de 2014 a 2018 e outro com dados de 2019
- Estatísticas descritiva
- Plot da variação do TCH por safra entre 2014 e 2018
- Identificação do bloco/talhão com menor e maior TCH entre todos os dados
- Agrupamento da produção total em milhões de toneladas por safra
- Plot da variação da produção total por safra
- Identificação do bloco/talhão com menor e maior produção total entre todos os dados
- Plot da produção total por bloco e safra
- Plot da produtividade por bloco e safra
- Análise de correlação e multicolinearidade

## Produção total por safra (em milhões de toneladas)
![Produção total por safra (em milhões de toneladas)](https://i.ibb.co/dGdzKcD/Produ-o-total-por-safra-em-milh-es-de-toneladas.png)
- O gráfico anterior chama a atenção pela queda brusca de produção em 2015 e sequente redução em 2016
- Através da análise exploratória foi possível identificar que a causa, ou uma delas, foi a redução na quantidade de blocos que produziram cana nos referidos anos
- Entretanto, nota-se que a produtividade média por bloco em mil toneladas não foi tão discrepante como a produção total
produtividade média por bloco = (producao total * 1000 / quant. de blocos)
- O resumo da análise está na tabela a seguir
![Resumo da produção total e produtividade média por safra e bloco](https://i.ibb.co/tKvv1MK/tabela-resumo-safra-producao-blocos.png)

## Modelagens de machine learning

- Separação em teste (70%) e treino (30%) dos dados de 2014 a 2018
-- X_treino, y_treino, X_teste, y_teste
- Dados para estimativa do TCH de 2019
-- X_2019, y_2019
- Features redimensionadas com StandardScaler()

### Abordagens
1. teste_ubirata_1.ipynb
- TCH como target
2. teste_ubirata_2.ipynb
- TCH como target
- mil_tonelada_cana adicionada como feature
3. teste_ubirata_3.ipynb
- mil_tonelada_cana como target
- TCH de cada talhão calculada ao final do processo
- TCH de cada bloco definida com a soma dos TCH dos talhões pertencentes ao respectivo bloco

# Conclusão
Apesar do modelo XGBoost Regressor ter o maior tempo de execução no treino e teste nas três abordagens, as métricas de
erro nas modelagens 2 e 3 foram satisfatórias.
Portanto, recomenda-se a aplicação do modelo XGBoost Regressor 2 com a target TCH e adição da feature mil_tonelada_cana (produção total por talhão) para estimativa das toneladas de cana por hectare (em cada bloco) na safra 2019.
Ainda destaca-se que o modelo indicado teve a maior estimativa de TCH e produção total para 2019, sendo essas de 38,48 mil toneladas e 3,02 milhões de toneladas respectivamente.

![TCH por safra em mil toneladas (2019 por XGBRegressor)](https://i.ibb.co/7YfQ4SV/TCH-por-safra-em-mil-toneladas-2019-por-XGBRegressor.png)

![Produção total de cana (em milhões de toneladas)](https://i.ibb.co/jGbqYhV/Produ-o-total-de-cana-em-milh-es-de-toneladas.png)

# Referências bibliográficas
1. Felipe Maldaner, L., de Paula Corrêdo, L., Fernanda Canata, T., & Paulo Molin, J. (2021). Predicting the sugarcane yield in real-time by harvester engine parameters and machine learning approaches. Computers and Electronics in Agriculture, 181(105945), 105945. doi:[10.1016/j.compag.2020.105945](https://www.sciencedirect.com/science/article/abs/pii/S0168169920331501)
2. Akbarian, S., Rahimi Jamnani, M., Xu, C., Wang, W., & Lim, S. (2023). Plot level sugarcane yield estimation by machine learning on multispectral images: a case study of Bundaberg, Australia. Information Processing in Agriculture. [doi:10.1016/j.inpa.2023.06.004](https://www.sciencedirect.com/science/article/pii/S2214317323000574)
