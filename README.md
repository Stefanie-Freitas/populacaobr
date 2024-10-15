# populacaobr
Projeto População Brasileira

Pular para o conteúdo principal
População Brasileira
População Brasileira_
Última edição em 1 de abril
Estatística - Probabilidade e Amostragem
Desafio Final Considerando a base de dados de populacao_brasileira.json responda as questões abaixo (os dados são fictícios). Você é uma pesquisadora desenvolvendo uma análise sobre as características da força de trabalho nos estados brasileiros. Responda as perguntas abaixo:
Importando os Dados

[ ]
#Importando a base

import pandas

import pandas as pd

brasileiros = pd.read_csv ("/content/drive/MyDrive/Bootcamp Analista de Dados/RRLNbvGQQtu5fPdxzFPI_populacao_brasileira.csv.csv")

brasileiros [:3]




[ ]
brasileiros.columns
Index(['Unnamed: 0', 'estado', 'idade', 'escolaridade',
       'nível de proficiência em inglês', 'renda', 'sexo'],
      dtype='object')

[ ]
len ('nível de proficiência em inglês')
31

[ ]
subset = brasileiros[['nível de proficiência em inglês']]

display (subset)

subset = brasileiros[['estado']]

display (subset)

subset = brasileiros[['escolaridade']]
display (subset)




1. Considere pessoas fluentes em inglês, qual a probabilidade complementar? Ou seja, qual a probabilidade de escolhermos uma pessoa aleatória e ela não ser fluente em inglês. Considere fluente quem tem o nível avançado.

[ ]
total_pessoas = len(brasileiros['nível de proficiência em inglês'])
fluentes = len(brasileiros[brasileiros['nível de proficiência em inglês'] == 'Avançado'])
prob_nao_fluente = 1 - (fluentes / total_pessoas)

print (prob_nao_fluente)

0.656
2. Se uma pessoa escolhida aleatoriamente for de Alagoas ou do Pará, qual é a probabilidade de ela ter uma renda superior a 5 mil reais?

[ ]
filtro = brasileiros[(brasileiros['estado'].isin(['PA'])) & (brasileiros['renda'] > 5000)]
prob = len(filtro) / len(brasileiros[brasileiros['estado'].isin(['PA'])])

print (prob)

0.05263157894736842
3. Descubra a probabilidade de uma pessoa ter ensino superior completo no estado do Amazonas. Qual a probabilidade da quinta pessoa que você conversar, que é amazonense, ter ensino superior completo?

[ ]
superior_am = len(brasileiros[(brasileiros['estado'] == 'AM') & (brasileiros['escolaridade'] == 'Superior')])
total_am = len(brasileiros[brasileiros['estado'] == 'AM'])
prob_superior_am = superior_am / total_am

# Para a quinta pessoa ser a primeira com superior completo, usamos a distribuição geométrica
prob_quinta_pessoa = prob_superior_am * (1 - prob_superior_am) ** 4

display ( prob_quinta_pessoa)
display (prob_superior_am)


4. Considerando a renda das pessoas do nosso conjunto, podemos dizer que a renda de uma pessoa brasileira está na sua maioria em que faixa (faça faixa de 1.500 reais)? Qual é a sua função densidade de probabilidade?

[ ]
brasileiros['Faixa_Renda'] = pd.cut(brasileiros['renda'], bins=range(0, int(brasileiros['renda'].max()) + 1500, 1500))
densidade = brasileiros['Faixa_Renda'].value_counts(normalize=True)

print(densidade)

(3000, 4500]    0.442
(1500, 3000]    0.414
(4500, 6000]    0.089
(0, 1500]       0.055
Name: Faixa_Renda, dtype: float64
5. Calcule a média e a variância da renda da amostra. Depois faça a distribuição normal, inclua o gráfico

[ ]
media = brasileiros['renda'].mean()

print (media)

variancia = brasileiros['renda'].var()

print (variancia)

# Para o gráfico
import seaborn as sns
sns.histplot(brasileiros['renda'], kde=True)


6. Primeiro considere a probabilidade encontrada no nosso conjunto de pessoas com escolaridade de pós-graduação. Considerando a amostra de população brasileira com 1 milhão de habitantes, qual a probabilidade de encontrarmos 243 mil pessoas com pós-graduação?

[ ]
# Substituir com os valores reais do seu conjunto de dados
prob_pos_grad = len(brasileiros[brasileiros['escolaridade'] == 'Pós-graduação']) / len(brasileiros)
# Para uma população de 1 milhão, é uma questão teórica mais complexa, pois envolve inferência.

print (prob_pos_grad)

0.253
7. Somando as densidades nós temos a função de densidade acumulada. Considerando a coluna ‘Escolaridade’ faça a função de densidade acumulada discreta para cada nível de escolaridade.

[ ]
brasileiros_escolaridade = brasileiros['escolaridade'].value_counts(normalize=True).sort_index().cumsum()

print (brasileiros_escolaridade)

Fundamental      0.266
Médio            0.504
Pós-graduação    0.757
Superior         1.000
Name: escolaridade, dtype: float64
8. Qual a margem de erro amostral da proporção populacional considerando a proporção de pessoas com nível de inglês intermediário?

[ ]
import scipy.stats as st
n = total_pessoas
p = len(brasileiros[brasileiros['nível de proficiência em inglês'] == 'intermediário']) / n
margem_erro = st.norm.ppf(0.975) * (p * (1 - p) / n) ** 0.5
print (margem_erro)
0.0
9. Calcula a renda da população. Qual a probabilidade de encontrar 60 pessoas com uma renda mil reais superior à média?
Passo 1: Calcular a média e o desvio padrão da renda

[ ]
import pandas as pd
import numpy as np
from scipy.stats import norm

# Supondo que você já tenha o DataFrame df carregado com os dados
# brasileiros = pd.read_csv('sua_base_de_dados.csv')

media_renda = brasileiros['renda'].mean()
desvio_padrao_renda = brasileiros['renda'].std()

print(f"Média da renda: {media_renda}")
print(f"Desvio padrão da renda: {desvio_padrao_renda}")

Média da renda: 3082.5371800000003
Desvio padrão da renda: 996.572239312141
Passo 2: Calcular a probabilidade de uma pessoa ter renda mil reais acima da média

[ ]
p_mais_1000 = 1 - norm.cdf((1000 + media_renda - media_renda) / desvio_padrao_renda)
print(f"Probabilidade de uma pessoa ter renda mais de 1000 reais acima da média: {p_mais_1000}")

Probabilidade de uma pessoa ter renda mais de 1000 reais acima da média: 0.15782441468557806
Passo 3: Calcular a probabilidade de encontrar exatamente 60 pessoas com essa condição em uma amostra

[ ]
from scipy.stats import binom

# Suponha que n seja o tamanho total da sua amostra
n = 1000  # Exemplo: tamanho da sua amostra

# Calculando a probabilidade de encontrar exatamente 60 pessoas com renda >1000 reais acima da média
prob_exatamente_60 = binom.pmf(60, n, p_mais_1000)
print(f"Probabilidade de encontrar exatamente 60 pessoas com essa condição: {prob_exatamente_60}")

Probabilidade de encontrar exatamente 60 pessoas com essa condição: 1.159703840678535e-21
10. Qual a probabilidade de escolhermos alguém do Sudeste que seja homem, com ensino fundamental e com renda maior que 2 mil reais por mês

[ ]
filtro_sudeste = brasileiros[(brasileiros['estado'].isin(['ES', 'MG', 'RJ', 'SP'])) & (brasileiros['sexo'] == 'Homem') & (brasileiros['escolaridade'] == 'Fundamental') & (brasileiros['renda'] > 2000)]
prob_sudeste = len(filtro_sudeste) / len(brasileiros)

print(prob_sudeste)
0.0
Produtos pagos do Colab - Cancelar contratos

