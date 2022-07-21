<div align="center">
  <h2>Relatório EP4 - MAC0323 Algoritmos e Estruturas de Dados II</h2>
  <h3>Sabrina Araújo - nºUSP 12566182</h3>
</div>

Programa que recebe uma expressão regular, que pode conter concatenações, alternativas(|), fechos(*), coringas(.), um ou mais(+), conjunto([ ]), intervalor ([ - ]) ou complementos ([^]) e um conjunto de palavras, e, para cada palavra, verifica se é ou não reconhecida pela expressão regular.

## Algoritmo

### Função regex()

A função `regex()` recebe um objeto da classe Grafo e um array de char que armazena a expressão regular. Essa função constrói um grafo que tem um vértice para cada caractere da expressão regular e as arestas correspondem às ε transições. Temos as seguintes operações:
<li> Concatenação: se é um caractere, guarda que o estado pode ler o caractere, caso contrário inclui arco para o estado seguinte. </li>
<li> Parêntesis: uma pilha é utilizada para lembrar onde começa o parêntesis, e inclui ε-transição para o próximo. </li>
<li> Alternativa: temos ε-transição do 1º parêntesis para o caractere depois do |, ε-transição do | para o fecha parêntesis, e o | é inserido na pilha para saber onde apontar. </li>
<li> Fecho: se ocorre depois de caractere tem arco de e para o caractere, se ocorre depois de ) deve ter arco de e para o abre parêntesis correspondente, e o arco para frente. </li>
<li> Um ou mais: se ocorre depois de caractere tem arco para o caractere, se ocorre depois de ) deve ter arco para o abre parêntesis correspondente, e o arco para frente. </li>
<li> Colchete: se é um [, o ] correspondente tem arco para frente. </li>

### Função reconhece()

A função `reconhece()` recebe um objeto da classe Grafo, um array de char que armazena a expressão regular (char *letra) e um array de char que armazena a palavra (char *pal). Essa função verifica se a partir da palavra é possível chegar no final do autômato construído na função `regex()`. Ao andar pelo autômato temos os seguintes casos quando um vértice é atingido:
<li> O vértice é igual ao caractere, então marcamos que o próximo vértice será verificado pelo `dfsR()`. </li>
<li> O vértice é '\', então se o caractere seguinte for igual ao caractere da palavra que estamos verificando, o vértice seguinte à esse caractere seguinte será verificado. </li>
<li> O vértice é '.', então para qualquer caractere que estamos verificando o vértice seguinte será verificado. </li>
<li> O vértice é '[', então temos 4 casos: conjunto, intervalo, conjunto com complemento e intervalo com complemento. Caso o caractere que está sendo verificado corresponda às condições desses casos, o vértice seguinte ao ']' será verificado pelo `dfsR()`. </li>
No final das verificações, a função devolve se o último vértice, que determina o final e não tem caractere, foi alcançado.

### grafo.h

Nesse programa, a classe Grafo está implementada. O objeto é iniciado com uma lista ligada com tamanho igual ao da expressão regular + 1. Ainda mais, a função `dfsR()` implementa uma busca em profundidade, no qual visita todos os vértices do grafo andando pelos arcos de um vértice a outro.

Observação: nos testes abaixo as letras do alfabeto não são case-sensitive nos casos dos intervalos ([A-Z] = [a-z]).

## Testes

### Testes usando os exemplos do enunciado do EP4

### Exemplo 1

Expressão regular: 
<li> (([a-z])*|([0-9])*)*@(([a-z])+\.)+br </li>
</br>
Palavras: 
<li> cef1999@ime.usp.br </li>
<li> thilio@bbb.com </li>
</br>

Saída:

![image](https://i.imgur.com/jz1muX2.png)

### Exemplo 2

Expressão regular: 
<li> (.)*A(.)* </li>
</br>
Palavras: 
<li> AAAAAAAAA </li>
<li> BCA </li>
<li> AAAAABBBBBB </li>
<li> BBB </li>
</br>

Saída:

![image](https://i.imgur.com/I2rMtvV.png)

### Exemplo 3

Expressão regular: 
<li> (A*CG|A*TA|AAG*T)* </li>
</br>
Palavras: 
<li> AACGTAAATA </li>
<li> CAAGA </li>
<li> ACGTA </li>
<li> AAAGT </li>
</br>

Saída:

![image](https://i.imgur.com/5ZLJjXx.png)

### Exemplo 4

Expressão regular: 
<li> [^AEIOU][AEIOU][^AEIOU][AEIOU] </li>
</br>
Palavras: 
<li> GATO </li>
<li> FINO </li>
<li> OLHO </li>
<li> BELO </li>
<li> RUSSO </li>
</br>

Saída:

![image](https://i.imgur.com/Gwd5yPW.png)

### Outros Testes

### Teste 1

Expressão regular: 
<li> [0-9]*\-[0-9]* </li>
</br>
Palavras: 
<li> 347823-213 </li>
<li> 472A-23 </li>
<li> 3434- </li>
<li> -324 </li>
<li> 9-A </li>
</br>

Saída:

![image](https://i.imgur.com/cW9gln5.png)

### Teste 2

Expressão regular: 
<li> [^0-9][0-9]* </li>
</br>
Palavras: 
<li> 12345 </li>
<li> A4905 </li>
<li> 96BS </li>
<li> -980 </li>
<li> K </li>
</br>

Saída:

![image](https://i.imgur.com/2D9F9Qx.png)

### Teste 3

Expressão regular: 
<li> \(([0-9])*\)9([0-9])*\-([0-9])* </li>
</br>
Palavras: 
<li> (11)91234-5678 </li>
<li> (1234)1234-1234 </li>
<li> (AB)91234-2334 </li>
<li> ()9- </li>
<li> (1)9-1 </li>
</br>

Saída:

![image](https://i.imgur.com/lRZhlrX.png)

### Teste 4

Expressão regular: 
<li> ([A-G])+([0-5])* </li>
</br>
Palavras: 
<li> AEDG023 </li>
<li> ABHJZ09 </li>
<li> E </li>
<li> E901 </li>
<li> ABC234 </li>
</br>

Saída:

![image](https://i.imgur.com/a7Ig1QW.png)

### Teste 5

Expressão regular: 
<li> g(oog)+le </li>
</br>
Palavras: 
<li> google </li>
<li> googoogle </li>
<li> goooogle </li>
<li> googoooogle </li>
<li> googoogoogoogoogle </li>
</br>

Saída:

![image](https://i.imgur.com/DqypqFz.png)

### Teste 6

Expressão regular: 
<li> ([0-9])*\.([0-9])*\.[0-9]\.([0-9])* </li>
</br>
Palavras: 
<li> 192.168.1.255 </li>
<li> 345.233.9.314 </li>
<li> 1234.1234.1234.1234 </li>
<li> ab346.123a.34 </li>
<li> 1.1.1.1 </li>
</br>

Saída:

![image](https://i.imgur.com/OkYd1R0.png)

### Teste 7

Expressão regular: 
<li> ([a-z])\-([a-b])*\-([a-c])+ </li>
</br>
Palavras: 
<li> z-ababab-abcabcabc </li>
<li> a-b-c </li>
<li> x-y-z </li>
<li> abc </li>
<li> aaaaaaaa-bbbbbbbcccc-a </li>
</br>

Saída:

![image](https://i.imgur.com/fmFDVub.png)

### Teste 8

Expressão regular: 
<li> (a|b)*c </li>
</br>
Palavras: 
<li> aaaaaaaabbbbbbc </li>
<li> aaababababc </li>
<li> cababab </li>
<li> abc </li>
<li> aaaaac </li>
</br>

Saída:

![image](https://i.imgur.com/9xSVDxQ.png)

### Teste 9

Expressão regular: 
<li> (.)*([0-2])+. </li>
</br>
Palavras: 
<li> abcdefg </li>
<li> aaaaa020202 </li>
<li> bbbb0303 </li>
<li> cccc0a </li>
<li> x0yz9 </li>
</br>

Saída:

![image](https://i.imgur.com/VwU8DAM.png)

### Teste 10

Expressão regular: 
<li> (AA*B) </li>
</br>
Palavras: 
<li> A </li>
<li> AAAAA </li>
<li> AB </li>
<li> B </li>
<li> BA </li>
</br>

Saída:

![image](https://i.imgur.com/zwx0aQo.png)

### Teste 11

Expressão regular: 
<li> ((a|b)*c) </li>
</br>
Palavras: 
<li> a </li>
<li> ab </li>
<li> abc </li>
<li> ac </li>
<li> bc </li>
</br>

Saída:

![image](https://i.imgur.com/BkM3iac.png)

### Teste 12

Expressão regular: 
<li> A*B </li>
</br>
Palavras: 
<li> A </li>
<li> B </li>
<li> AAB </li>
<li> AAABBB </li>
<li> AB </li>
</br>

Saída:

![image](https://i.imgur.com/8G5d0q5.png)

### Teste 13

Expressão regular: 
<li> A([A-Z]) </li>
</br>
Palavras: 
<li> AA </li>
<li> aA </li>
<li> aa </li>
<li> Aa </li>
<li> Za </li>
</br>

Saída:

![image](https://i.imgur.com/40KhoV7.png)

Observação: em relação aos tempos de execução, é possível notar que são mínimos.


