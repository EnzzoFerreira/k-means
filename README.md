# Kmeans

O KMeans é o algoritmo que junta dados tentando separar o maior número de grupos da variância igual, minimizando um critério conhecido como inércia soma dos quadrados dentro dos clusters. Este algortimo exige que o número de clusters seja especificado. Ele se encaixa bem em grandes quantidades de amostras e é usado em uma ampla gama de áreas de aplicação em diversos campos.

O algoritmo K-means tem como objetivo escolher centroides que minimizem a inércia , ou o critério da soma dos quadrados dentro do cluster :

$$
\sum_{i=0}^{N-1} \min_{\mu_j \in C} \left( \lVert x_i - \mu_j \rVert^2 \right)
$$

O KMeans também é conhecido como "algoritmo Lloyd", em termos básicos, é um algoritmo que é dividido em 3 etapas. A primeira etapa escolhe os centroides iniciais, sendo o método básico de escolher K amostras do conjunto de dados X. Após a inicialização, há um loop entre as duas etapas seguintes. A primeira atribui cada amostra ao seu centro mais próximo. A segunda etapa cria novos centros e calcula a média dos valores de todas as amostras atribuídas a cada centríde anterior. E a terceira etapa é a atualização dos centros, o momento em que os centroídes deixam de ser os pontos aleatórios do início e passam a refletir de verdade onde as amostras se agruparam.


## Método do Cotovelo

O K-means minimiza a **inércia** (soma dos quadrados das distâncias de cada amostra ao centroide do seu cluster), que mede a coesão interna dos clusters. A inércia diminui conforme k aumenta, então o método do cotovelo roda o K-means para vários valores de k e observa em que ponto a queda da inércia deixa de ser expressiva (a "dobra" do gráfico).

Limitações (segundo a documentação do scikit-learn):
- A inércia não é normalizada: só sabemos que menor é melhor e que zero é o ideal.
- Assume clusters convexos e isotrópicos; responde mal a clusters alongados ou irregulares.
- Em alta dimensionalidade, as distâncias euclidianas ficam infladas; usar PCA antes pode ajudar.

Por isso o cotovelo costuma ser complementado pelo método da silhueta, que a documentação cita como exemplo para escolher o número de clusters.


## Método da Silhueta

Quando não há rótulos reais, o Coeficiente de Silhueta avalia o próprio agrupamento. Para cada amostra, usa duas medidas: **a** (distância média até os outros pontos do mesmo cluster) e **b** (distância média até os pontos do cluster mais próximo seguinte). O coeficiente do conjunto é a média dos coeficientes de todas as amostras.

- Varia de -1 (agrupamento incorreto) a +1 (clusters densos e bem separados).
- Valores próximos de 0 indicam clusters sobrepostos.
- Desvantagem (segundo a doc do scikit-learn): tende a ser maior para clusters convexos do que para clusters baseados em densidade, como os do DBSCAN.

Uso para escolher k (prática, fora da documentação): rodar o K-means para vários valores de k (a partir de 2) e comparar o score médio de cada um. No Iris, k=2 obteve ~0,68 e k=3 ~0,55.