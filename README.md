# Teorema-Central-do-Limite: de várias distribuições uniformes para uma normal (Python)
Aplicação empírica do Teorema Central do Limite - Como transformar distribuições Uniformes em uma Normal usando Python

O Teorema do Limite Central é um conceito da estatística que afirma que, quando somamos muitas variáveis aleatórias independentes e identicamente distribuídas, a distribuição resultante se aproxima de uma distribuição normal, independentemente da distribuição original das variáveis individuais.

No nosso caso, queremos fazer a repetição (uma simulação conforme o Método de Monte Carlo) de três conjuntos de variáveis aleatórias com distribuição uniforme, na qual o tamanho das amostras são 12, 48 e 96. Ademais, queremos ajustar essas variáveis para possuir média 0 e desvio padrão 1. Por fim, calcular as estatísticas solicitadas.

Para gerar essa simulação e fazer os cálculos, podemos tomar os seguintes passos:

Importamos as bibliotecas numpy para realizar cálculos numéricos e matplotlib.pyplot, para criar os histogramas.

Definimos as variáveis N (número de simulações) e nvalues (uma lista de diferentes tamanhos de sequência de variáveis uniformes com as médias definidas, elas serão usados para simular as variáveis normais). A N foi atribuído arbitrariamente o valor de 10000, considerado suficientemente grande para nos fornecer resultados mais precisos, para cada valor em nvalues (12, 48, 96), a simulação será repetida N vezes.

Definimos uma função chamada simulate_normal, que realiza a simulação das variáveis aleatórias.

Criamos uma lista vazia para receber as médias, denominada means.

Criamos um loop que itera N vezes. A cada iteração, geramos uma sequência de n variáveis uniformes aleatórias entre 0 e 1 usando np.random.uniform().

Em seguida, calculamos a média dessas variáveis uniformes e ajustamos a média subtraindo 0,5, isto é, normalizamos a média para 0 uma vez que as variáveis uniformes no intervalo 0-1 têm uma média de 0,5.

Também devemos normalizar o desvio padrão para 1, por isso dividimos a média ajustada pelo desvio padrão estimado das variáveis uniformes, que é dado por np.sqrt(1/12). Essa média ajustada e padronizada representa a variável aleatória normal simulada. Armazenamos essas variáveis em uma lista que denominamos de means.

Usamos o if __name__ == "__main__": para verificar se o módulo está sendo executado diretamente como um script, em vez de ser importado como um módulo em outro código.

Criamos uma lista vazia chamada results, que será usada para armazenar os resultados das simulações para diferentes tamanhos de sequência.

Fazemos um loop for para iterar sobre os valores em n_values, que representa os diferentes tamanhos de sequência que queremos simular.

Dentro do loop, chamamos a função simulate_normal para simular as variáveis aleatórias normais usando o tamanho de sequência atual. Os resultados são armazenados na lista result.

Em seguida, imprimimos os resultados numéricos solicitados para as simulações, isto é, os momentos (média, variância e desvio padrão), os intervalos de confiança (95% por convenção) e cálculos de probabilidade de cauda (convencionalmente +/- dois desvios padrão) com base nas variáveis geradas.

Por fim, geramos o histograma dos resultados usando plt.hist(). Configuramos o número de barras para 30 e definimos os rótulos dos eixos e o título do gráfico. Mostramos o histograma na tela usando plt.show().

A implementação do algoritmo pode ser vista abaixo:


import numpy as np
import matplotlib.pyplot as plt

N = 10000
n_values = [12, 48, 96]

def simulate_normal(n):
    means = []
    for _ in range(N):
        u = np.random.uniform(size=n)
        mean = np.mean(u) - 0.5
        means.append(mean / np.sqrt(1/12))
    return means

if __name__ == "__main__":
    results = []
    for n in n_values:
        result = simulate_normal(n)
        results.append(result)
    
    for i, n in enumerate(n_values):
        print(f"n = {n}")
        print("Mean:", np.mean(results[i]))
        print("Variance:", np.var(results[i]))
        print("Standard Deviation:", np.std(results[i]))
        print("95% Confidence Interval:", np.percentile(results[i], [2.5, 97.5]))
        print("Probability P(X > 2):", np.mean(np.array(results[i]) > 2))
        print("Probability P(X < -2):", np.mean(np.array(results[i]) < -2))
        print()
    
        plt.hist(results[i], bins=30, density=True, alpha=0.5)
        plt.xlabel('Values')
        plt.ylabel('Frequency')
        plt.title(f'Histogram - n = {n}')
        plt.show()

O resultado devolvido pelo Python foi o seguinte:


n = 12
Mean: -0.0007403938833733143
Variance: 0.08370335817061365
Standard Deviation: 0.2893153265394242
95% Confidence Interval: [-0.56784464 0.56964851]
Probability P(X > 2): 0.0
Probability P(X < -2): 0.0


n = 48
Mean: -0.0013580343049823108
Variance: 0.020927030031124064
Standard Deviation: 0.14466177805876734
95% Confidence Interval: [-0.28376393 0.28081683]
Probability P(X > 2): 0.0
Probability P(X < -2): 0.0


n = 96
Mean: 0.0002171907430629777
Variance: 0.010425800285454168
Standard Deviation: 0.10210680822283189
95% Confidence Interval: [-0.20286497 0.1992716 ]
Probability P(X > 2): 0.0
Probability P(X < -2): 0.0

Observe que o histograma das simulações de fato se aproxima de uma normal, a média se aproxima de zero, mas o desvio padrão, apesar da normalização, não está tão próximo da unidade.
