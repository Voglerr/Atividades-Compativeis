# Atividades-Compativeis
## Introdução
Em "Introdução aos Algoritmos" (CLRS), algoritmo guloso é definido como um algoritmo que "sempre faz a escolha que parece ser a melhor no momento em questão. Isto é, faz uma escolha localmente ótima, na esperança de que essa escolha leve a uma solução globalmente ótima". Por conta dessa abordagem, o algoritmo guloso não é capaz de garantir soluções ótimas para vários problemas computacionais; essa abordagem da "melhor solução localmente" acaba levando frequentemente a soluções inferiores que a melhor solução possível para o problema geral.

Uma exceção a isso é o problema das atividades compatíveis, que a solução gulosa não só produz soluções ótimas como também é uma solução eficiente, com complexidade Θ(Nlog N). Aqui será abordado três abordagens gulosas possíveis para a solução desse problema e qual delas é uma boa solução, seu algoritmo e uma implementação em código, e também uma demonstração de que essa abordagem retorna soluções ótimas.

## O problema das atividades compatíveis
Dado um conjunto _T_ = {_t<sub>1</sub>_, ..., _t<sub>n</sub>_} com _n_ tarefas onde cada _t<sub>i</sub>_ ∈ _T_ tem um tempo inicial  _s<sub>i</sub>_ e um tempo final _f<sub>i</sub>_, encontrar o maior subconjunto de tarefas mutuamente compatíveis. Ou seja, problema consiste em seleciona o maior número possível de atividades todas compatíveis entre si dados os horários de início e término dessas atividades, sendo que atividades compatíveis são aquelas que não ocorre sobreposição de tempo entre elas. 

## Duas abordagens possíveis
Enquanto puder escolher tarefas:
  1. Escolha a de menor duração
  2. Escolha a de menor _s<sub>i</sub>_
  
  (que seja compatível com as tarefas já escolhidas)

Na duas existe a intenção de alcançar o objetivo (selecionar o maior número possível), priorizando no momento alguma variável que parece levar a uma solução ótima. Todas levam a uma solução correta, pois é forçado escolher sempre uma tarefa compatível com as outras que já foram escolhidos e obedecer as premissas do problema, mas nenhuma consegue retornar sempre uma solução ótima.

### Escolhendo sempre a de menor duração
Essa abordagem parece levar à melhor solução, pois aparentemente isso possibilitaria escolher o maior número possível de tarefas e assim maximizar a solução. Porém um simples contraexemplo mostra que ela nem sempre leva ao resultado desejado.
Considere o conjunto de tarefas _T_ = {_t<sub>1</sub>_, ..., _t<sub>6</sub>_}, com 6 tarefas, onde os tempos de início são:
  _s<sub>k</sub>_ = {1, 5, 7, 16, 20, 22} 
e tempos de término:
  _f<sub>k</sub>_ = {6, 8, 15, 21, 23, 28}

A melhor solução nesse caso é escolher as tarefas _t<sub>1</sub>_, _t<sub>3</sub>_, _t<sub>4</sub>_ e _t<sub>6</sub>_. Porém essa abordagem irá retornar as duas tarefas _t<sub>2</sub>_ e _t<sub>5</sub>_. Isso porque ao escolher a primeira tarefa o algoritmo terá que escolher entre _t<sub>1</sub>_ e  _t<sub>2</sub>_ (tarefas não compatíveis) e decidirá pela tarefa  _t<sub>2</sub>_, que é a menor; depois, o algoritmo não poderá escolher  _t<sub>3</sub>_, pois não é compatível com  _t<sub>2</sub>_; a próxima decisão será escolher entre  _t<sub>4</sub>_ e  _t<sub>5</sub>_, e o algoritmo escolherá  _t<sub>5</sub>_ por ser o de menor duração; depois, o algoritmo não poderá escolher  _t<sub>6</sub>_, pois não é compatível com  _t<sub>5</sub>_.

Por casos como esse que escolher as tarefas de menor duração, apesar de levar a soluções sempre corretas, não leva sempre a soluções ótimas.

### Escolhendo a de menor _s<sub>i</sub>_
Significa escolher sempre aquelas que começam primeiro. Também aparenta ser uma abordagem que retorna a melhor solução possível, pois escolhendo sempre a que começa antes dá a impressão de que com isso é possível escolher o maior número de tarefas. Porém mais uma vez esse acaba não sendo sempre o caso.
Considere o conjunto de tarefas _T_ = {_t<sub>1</sub>_, ..., _t<sub>4</sub>_}, com 4 tarefas, onde os tempos de início são:
  _s<sub>k</sub>_ = {1, 3, 6, 10} 
e tempos de término:
  _f<sub>k</sub>_ = {15, 5, 8, 13}
  
Com essa abordagem será escolhido apenas a tarefa _t<sub>1</sub>_, por ser a que começa primeiro. Escolhendo ela, não será possível escolher nenhuma das outras três tarefas pois todas elas não são compatíveis com _t<sub>1</sub>_. Logo teremos uma solução com apenas um elemento, onde a solução ótima seria uma com três elementos.

## A melhor abordagem
Existe uma outra forma de resolver esse problema de forma gulosa, que é escolher sempre a tarefa que termina primeiro, ou seja, escolher a de menor _f<sub>i</sub>_ (que seja compatível com as tarefas já escolhidas). Essa abordagem retorna sempre a solução ótima, seja qual for o conjunto de tarefas dado.

### Algoritmo
<pre>
// T: conjunto de tarefas
// s: conjunto de tempos de início
// f: Conjunto de tempos de término
// n: número de tarefas
Solucao(T, s, f, n)
  1. Ordene as tarefas em ordem crescente de tempo final, de modo que f<sub>1</sub> <= f<sub>2</sub> <= ... <= f<sub>n</sub>
  2. S = {t<sub>1</sub>}
  3. k = 1 // Índice da última tarefa adicionada
  4. para i = 2 até n faça
  5.   se s<sub>i</sub> ≥ f<sub>k</sub>
  6.     S = S ∪ {t<sub>i</sub>}
  7.     k = i
  8. retorna S
</pre>

Este algoritmo está correto, pois no loop da linha 4 sempre adicionamos tarefas que começam depois de terminadas todas as tarefas já adicionadas na solução, garantindo que as tarefas da solução são mutuamente compatíveis. Sua complexidade será de Θ(Nlog N), sendo Θ(Nlog N) para o algoritmo de ordenação e Θ(N) para percorrer todas as tarefas e decidir se adiciona ou não.

### Ele é ótimo?
Sim, trata-se de uma abordagem que sempre retorna uma solução ótima, e é isso que se busca demonstrar nessa seção:

Sendo _T_ uma lista de tarefas ordenadas de forma crescente pelo tempo de término de cada tarefa. A demonstração é baseada em duas propriedades:
1. Existe uma solução ótima para _T_ que contém a escolha gulosa _t<sub>1</sub>_
2. Se A for uma solução ótima que contém _t<sub>1</sub>_, então _A'_ = _A_ - {_t<sub>1</sub>_} é uma solução ótima para _T<sub>1</sub>_ = {_t<sub>i</sub>_ ∈ _T_, _s<sub>i</sub>_ ≥ _f<sub>1</sub>_}

Prova da propriedade 1:
<pre>
  Seja A uma solução ótima de T que não contém t<sub>1</sub>:
    1. Ordene as tarefas em A de modo que a primeira tarefa em A, k<sub>1</sub>, seja a que termina primeiro
    2. Seja A' = {A - {k<sub>1</sub>}} ∪ {t<sub>1</sub>}
    3. Então:
    4.   Os conjuntos A - {k<sub>1</sub>} e {t<sub>1</sub>} são disjuntos
    5.   As atividades em A' são compatíveis (_f<sub>1</sub>_ <= _s<sub>k</sub>_ e A é uma solução correta)
    6.   A' também é ótima, pois |A'| = |A|
  Então sempre existe uma solução ótima que começa com a escolha gulosa.
</pre>

Prova da propriedade 2:
<pre>
  Prova por contradição:
    1. Se existe a solução B' para S' tal que |B'| > |A'|, então seja B = B' ∪ {1}
    2. Logo |B| > |A|, o que contradiz que A é uma solução ótima
</pre>

Logo: 
- Após cada escolha é feita, resta um subproblema da mesma forma que o original
- Por indução, fazer a escolha gulosa a cada passo produz sempre uma solução ótima
  
Temos que a abordagem gulosa produz uma solução ótima para todo conjunto de tarefas.
