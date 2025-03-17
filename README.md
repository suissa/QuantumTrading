# QuantumTrading

Nesse repositório eu vou repassar os algoritmos quânticos que utilizo para Trading.

## QAOA (Quantum Approximate Optimization Algorithm)

O QAOA (Quantum Approximate Optimization Algorithm) é um algoritmo híbrido quântico-clássico desenvolvido para resolver problemas de otimização combinatória, como problemas de portfólio financeiro, roteamento, seleção de trading e outros. Ele funciona aplicando camadas alternadas de operações quânticas para aproximar a melhor solução possível.

## Fundamentos do QAOA
O QAOA é projetado para resolver problemas de QUBO (Quadratic Unconstrained Binary Optimization) e problemas modelados como um Hamiltoniano de Ising, que podem ser descritos por:
Estrutura do Algoritmo
O QAOA combina computação quântica e otimização clássica. Ele segue estes passos:

1. Definir o problema como um Hamiltoniano (Hamiltoniano de Custo).
2. Escolher uma Hamiltoniana de Mixer para explorar o espaço de soluções.
3. Construir um circuito quântico parametrizado com camadas alternadas de gates baseadas nesses Hamiltonianos.
4. Executar o circuito e medir os resultados, ajustando os parâmetros com um otimizador clássico.

## Componentes Chave do Algoritmo

O QAOA tem três partes principais:

🔹 Hamiltoniano de Custo: Representa o problema de otimização.
🔹 Hamiltoniano de Mixer: Explora possíveis soluções.
🔹 Parâmetros Variacionais: Ajustam a evolução do circuito.

## Definição dos Hamiltonianos

Os dois Hamiltonianos são aplicados iterativamente no circuito quântico.

🔹 Hamiltoniano de Custo: Este Hamiltoniano codifica a função objetivo do problema. Se estivermos resolvendo um problema QUBO, é definido como:
onde:

![Hamiltonianos de custo](https://quicklatex.com/cache3/92/ql_77c199a9317eef6d78cab55f77cd6a92_l3.png)


𝑄𝑖𝑗: são os coeficientes da matriz QUBO.
𝑍𝑖: é o operador de Pauli-Z aplicado ao qubit 
🔹 O estado de menor energia do sistema quântico representará a melhor solução.

### O que o Operador Pauli-X faz?

O operador Pauli-X (𝑋) age como um interruptor, trocando os estados dos qubits:

🔹 Se um qubit estiver no estado ∣0⟩, o Pauli-X o transforma em ∣1⟩.
🔹 Se um qubit estiver no estado ∣1⟩, o Pauli-X o transforma em ∣0⟩.

Isso significa que o Hamiltoniano de Mixer age mudando os valores das variáveis no sistema, permitindo a exploração de novas soluções.

🔹 Hamiltoniano de Mixer: O Mixer permite a exploração do espaço de soluções. 

Ele é definido como:

![Hamiltoniano Mixer](https://quicklatex.com/cache3/47/ql_075ae5b341295dfd833e1ad53a19c447_l3.png)

𝑋𝑖: é o operador de Pauli-X aplicado ao qubit 
🔹 Este Hamiltoniano gira os qubits, permitindo explorar diferentes configurações.

A fórmula calcula a energia total de uma configuração de variáveis, levando em conta tanto interações entre variáveis quanto o efeito de cada variável isoladamente, ajudando o QAOA a encontrar a melhor solução possível para um problema de otimização.

## Construção do Circuito Quântico

O circuito QAOA é montado aplicando camadas alternadas de 𝐻C e 𝐻M, ajustadas pelos parâmetros variacionais (𝛾,𝛽).

1. Inicialização
🔹 Preparamos um estado inicial uniforme superposto

2. Aplicação do Hamiltoniano de Custo
🔹 Aplicamos gates de fase controladas, codificando a função objetivo no sistema quântico.

3. Aplicação do Hamiltoniano de Mixer
🔹 Aplicamos rotações, permitindo que o sistema explore diferentes soluções.

4. Medição
🔹 O circuito é medido em base computacional ∣0⟩,∣1⟩ para encontrar a solução ótima.

5. Otimização Clássica
🔹 Ajustamos os parâmetros γ,β usando um otimizador clássico (ex.: COBYLA, SPSA, Nelder-Mead).
🔹 Repetimos o processo até encontrar o melhor valor.

### O que é o Operador Pauli-Z?
O Pauli-Z (𝑍) é um dos três operadores de Pauli, fundamentais na mecânica quântica e na computação quântica. Ele é representado pela matriz de Pauli-Z:
[ 1 0
  0−1]
  
Esse operador age sobre qubits, alterando seu estado de uma maneira específica.

🔹 Como o Pauli-Z funciona na computação quântica?
O operador Z não altera o estado ∣0⟩, mas inverte o sinal do estado ∣1⟩:

Se aplicado ao qubit no estado ∣0⟩, o qubit permanece igual: Z∣0⟩ = ∣0⟩
Se aplicado ao qubit no estado ∣1⟩, o qubit muda de sinal: Z∣1⟩ = −∣1⟩
Isso significa que o operador introduz uma fase negativa no qubit ∣1⟩, mas não afeta ∣0⟩.

🔹 Interpretação Física do Pauli-Z
O operador Pauli-Z está diretamente relacionado ao eixo Z da esfera de Bloch, que representa os estados quânticos geometricamente.

- ∣0⟩ está no topo da esfera de Bloch e não é afetado pelo Pauli-Z.
- ∣1⟩ está na parte inferior da esfera de Bloch e recebe um fator de fase -1 quando o Pauli-Z é aplicado.

Isso significa que o operador Pauli-Z reflete os qubits em torno do eixo Z.

#### Uso do Pauli-Z na Computação Quântica

O operador Pauli-Z aparece em vários contextos, incluindo:

- Correção de Erros Quânticos: o Pauli-Z pode ser usado para detectar e corrigir certos tipos de erros em qubits.


#### Medições em Circuitos Quânticos

Muitas medições na computação quântica são feitas na base Z, onde o Pauli-Z ajuda a distinguir entre ∣0⟩ e ∣1⟩.


#### QAOA e Modelos de Ising

No Quantum Approximate Optimization Algorithm (QAOA), o operador Pauli-Z é usado para definir restrições e interações entre qubits, modelando problemas como otimização financeira e problemas logísticos.
4️⃣ Portas de Fase (RZ Gate)

O operador Pauli-Z pode ser visto como um caso especial da porta RZ(θ), usada para adicionar fases em circuitos quânticos.

No QAOA, ele aparece na Hamiltoniana de Custo para definir relações entre variáveis, ajudando a resolver problemas como trading e portfólio financeiro.




## Exemplo de Implementação no Qiskit

```py
from qiskit import Aer
from qiskit.optimization import QuadraticProgram
from qiskit_optimization.translators import from_docplex_mp
from qiskit.algorithms import QAOA
from qiskit.primitives import Estimator
from qiskit.algorithms.optimizers import COBYLA
from qiskit_optimization.algorithms import MinimumEigenOptimizer
from docplex.mp.model import Model

# Criar o problema de otimização (QUBO)
mdl = Model('QAOA_Optimization')
x1 = mdl.binary_var(name='x1')
x2 = mdl.binary_var(name='x2')
x3 = mdl.binary_var(name='x3')

# Definir a função objetivo (minimizar -x1x2 - x2x3 + x1)
mdl.maximize(-x1 * x2 - x2 * x3 + x1)

# Converter para QUBO
qp = from_docplex_mp(mdl)

# Criar simulador e otimizador QAOA
estimator = Estimator()
qaoa = QAOA(estimator, optimizer=COBYLA())

# Resolver o problema
optimizer = MinimumEigenOptimizer(qaoa)
result = optimizer.solve(qp)

# Exibir solução
print("Melhor Solução:", result.x)
print("Valor Ótimo:", result.fval)

```


## Explicação do Código
🔹 Criamos um problema QUBO com variáveis 
🔹 Definimos a função objetivo.
🔹 Convertamos para um Hamiltoniano quântico.
🔹 Aplicamos QAOA para encontrar a solução.
🔹 Medimos os qubits e ajustamos os parâmetros com um otimizador clássico.


## Aplicações do QAOA

O QAOA pode ser usado para otimizar:

🔹 Trading Algorítmico → Ajuste de parâmetros de indicadores como RSI, MACD, Bandas de Bollinger.
🔹 Seleção de Portfólio → Escolher ativos maximizando retorno e minimizando risco.
🔹 Roteamento Logístico → Encontrar o caminho mais eficiente para entregas.
🔹 Alocação de Recursos → Escolher a melhor distribuição de investimentos.

| Algoritmo	| Características |
------------|-----------------|
| QAOA	| Aproxima soluções combinatórias usando circuitos quânticos |
| Branch & Bound	| Divide o problema em subproblemas e explora soluções |
| Algoritmos Genéticos	| Evolui soluções através de mutação e seleção |
| Programação Linear	| Resolve problemas com restrições lineares |




Como o QAOA sabe que encontrou uma solução otimizada?
O QAOA (Quantum Approximate Optimization Algorithm) otimiza um problema baseado na minimização de energia da Hamiltoniana de Custo (𝐻𝐶). Mas como ele sabe que está otimizado? Ele segue três critérios principais:


1. Mede os qubits e verifica qual estado aparece com mais frequência

No final da execução do circuito quântico, os qubits são medidos. 
O QAOA repete o experimento várias vezes e registra quais estados aparecem com mais frequência.

🔹 Se um determinado estado aparece muitas vezes, isso indica que ele tem a menor energia possível → significa que ele é provavelmente a melhor solução.


2. Mede a energia da solução

O QAOA calcula a energia associada ao estado mais frequente.

A energia é dada por:

![](https://quicklatex.com/cache3/2c/ql_d51cd2b3088c1d8af440aaf90200652c_l3.png)

3. O Algoritmo Variacional Ajusta os Parâmetros
4. 
O QAOA usa otimização clássica para encontrar os melhores valores de 𝛾 e 𝛽, que controlam a evolução do circuito.

- O circuito começa com um chute inicial de 𝛾 e 𝛽.
- Executa e mede os qubits.
- Calcula a energia do estado encontrado.
- Ajusta 𝛾 e 𝛽 para reduzir ainda mais a energia.
- Repete até encontrar a melhor solução.

🔹 Ele otimiza como um jogador de xadrez que aprende com seus erros: se o movimento anterior não foi bom, ele tenta outro até encontrar a estratégia vencedora.

Como saber se a solução encontrada é boa?

🔹 O estado mais frequente entre as medições indica a melhor solução.
🔹 Se a energia do sistema for mínima, então essa é provavelmente a melhor configuração possível.
🔹 O otimizador clássico ajusta os parâmetros até que a energia não diminua mais.
🔹 Comparamos com soluções clássicas para ver se a resposta do QAOA bate com soluções exatas.



 
---

Bom com isso você já aprendeu bastanet sobre Otimização Quântica, agora é a hora de escolher os indicadores a serem otimizados, no mundo da análise técnica nós temos 6 categorias de indicadores:

1. Indicadores de Tendência

Objetivo: Identificar a direção predominante do mercado (tendência de alta, baixa ou lateral). 
Como Funcionam: Calculam médias ou suavizam preços para detectar tendências e reversões.

Exemplos:
🔹 Médias Móveis (SMA, EMA, WMA, ALMA) – Suavizam preços para mostrar a tendência.
🔹 MACD (Moving Average Convergence Divergence) – Mede a força e a direção da tendência com médias móveis.
🔹 ADX (Average Directional Index) – Mede a força da tendência, mas não sua direção.
🔹 Ichimoku Kinko Hyo – Fornece suporte, resistência e direção de tendência em um único gráfico.


2. Indicadores de Momentum (Força)

Objetivo: Medir a velocidade das mudanças nos preços e identificar momentos de entrada e saída. 
Como Funcionam: Comparam preços atuais e passados para medir a força da tendência.

Exemplos:
🔹 RSI (Relative Strength Index) – Mede a força do movimento de preços e detecta sobrecompra/sobrevenda.
🔹 Estocástico – Compara o preço de fechamento com a faixa de preços passada para indicar reversões.
🔹 CCI (Commodity Channel Index) – Mede variações de preço em relação a uma média estatística.
🔹 Momentum – Mede a taxa de mudança do preço ao longo do tempo.


3. Indicadores de Volume

Objetivo: Analisar o volume de negociações para confirmar tendências e prever reversões. 
Como Funcionam: Observam se o volume aumenta ou diminui em relação aos preços.

Exemplos:
🔹 OBV (On-Balance Volume) – Mede o fluxo de volume baseado em alta e baixa de preços.
🔹 Volume Profile – Analisa onde o volume foi negociado em diferentes níveis de preço.
🔹 MFI (Money Flow Index) – Indica pressão de compra/venda combinando preço e volume.
🔹 VWAP (Volume Weighted Average Price) – Calcula a média do preço ponderada pelo volume.


4. Indicadores de Volatilidade

Objetivo: Medir a variação e a imprevisibilidade dos preços para antecipar mudanças no mercado. 
Como Funcionam: Analisam a amplitude dos preços e sua variação ao longo do tempo.

Exemplos:
🔹 Bandas de Bollinger – Criam uma faixa ao redor do preço para indicar se está sobrecomprado ou sobrevendido.
🔹 ATR (Average True Range) – Mede a volatilidade do mercado ao calcular a média da amplitude dos preços.
🔹 Keltner Channels – Funciona como as Bandas de Bollinger, mas usa médias móveis.
🔹 VIX (Índice de Volatilidade) – Mede a volatilidade implícita no mercado de ações.


5. Indicadores de Suporte e Resistência

Objetivo: Identificar níveis onde os preços tendem a reverter ou consolidar. 
Como Funcionam: Calculam pontos estratégicos com base em máximas, mínimas e médias anteriores.

Exemplos:
🔹 Pivôs – Calculam pontos de suporte e resistência com base nos preços anteriores.
🔹 Fibonacci Retracement – Usa proporções matemáticas para identificar zonas de suporte e resistência.
🔹 Linhas de Tendência – Identificam níveis psicológicos importantes baseados em máximas e mínimas anteriores.


6. Indicadores de Ciclos e Estatísticos

Objetivo: Identificar padrões e ciclos de mercado que podem influenciar os preços. 
Como Funcionam: Utilizam cálculos matemáticos para prever mudanças de comportamento do mercado.

Exemplos:
🔹 Elliott Wave Theory – Análise de padrões de ondas para prever ciclos de alta e baixa.
🔹 Fourier Transform – Identifica ciclos ocultos nos preços do mercado.
🔹 Detrended Price Oscillator (DPO) – Remove tendências para focar nos ciclos de curto prazo.


Então a ideia ou escolher uma ou duas categorias e focar em um timeframe pequeno, médio ou grande, ou fazer um completo abrangendo todas as categorias.

Mas além dos indicadores nós também podemos otimizar o valor do StopLoss e do TakeProfit, isso muda completamente a sua estratégia, vai por mim.
