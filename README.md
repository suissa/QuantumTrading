# QuantumTrading

Nesse repositório eu vou repassar os algoritmos quânticos que utilizo para Trading.

## QAOA (Quantum Approximate Optimization Algorithm)

O QAOA (Quantum Approximate Optimization Algorithm) é um algoritmo híbrido quântico-clássico desenvolvido para resolver problemas de otimização combinatória, como problemas de portfólio financeiro, roteamento, seleção de trading e outros. Ele funciona aplicando camadas alternadas de operações quânticas para aproximar a melhor solução possível.

## Fundamentos do QAOA
O QAOA é projetado para resolver problemas de QUBO (Quadratic Unconstrained Binary Optimization) e problemas modelados como um Hamiltoniano de Ising, que podem ser descritos por:
Estrutura do Algoritmo
O QAOA combina computação quântica e otimização clássica. Ele segue estes passos:

1️⃣ Definir o problema como um Hamiltoniano (Hamiltoniano de Custo).
2️⃣ Escolher uma Hamiltoniana de Mixer para explorar o espaço de soluções.
3️⃣ Construir um circuito quântico parametrizado com camadas alternadas de gates baseadas nesses Hamiltonianos.
4️⃣ Executar o circuito e medir os resultados, ajustando os parâmetros com um otimizador clássico.

## Componentes Chave do Algoritmo

O QAOA tem três partes principais:

- Hamiltoniano de Custo: Representa o problema de otimização.
- Hamiltoniano de Mixer: Explora possíveis soluções.
- Parâmetros Variacionais: Ajustam a evolução do circuito.

## Definição dos Hamiltonianos

Os dois Hamiltonianos são aplicados iterativamente no circuito quântico.

- Hamiltoniano de Custo: Este Hamiltoniano codifica a função objetivo do problema. Se estivermos resolvendo um problema QUBO, é definido como:
onde:

𝑄𝑖𝑗: são os coeficientes da matriz QUBO.
𝑍𝑖: é o operador de Pauli-Z aplicado ao qubit 
- O estado de menor energia do sistema quântico representará a melhor solução.

- Hamiltoniano de Mixer: O Mixer permite a exploração do espaço de soluções. Ele é definido como:


onde:

𝑋𝑖: é o operador de Pauli-X aplicado ao qubit 
- Este Hamiltoniano gira os qubits, permitindo explorar diferentes configurações.

--- 
Uma das primeiras coisas que você pode fazer antes de começar a *tradear* é escolher um grupo de indicadores, no mundo da análise técnica nós temos 6 categorias de indicadores:

1. Indicadores de Tendência

Objetivo: Identificar a direção predominante do mercado (tendência de alta, baixa ou lateral). :pino: Como Funcionam: Calculam médias ou suavizam preços para detectar tendências e reversões.

Exemplos:
- Médias Móveis (SMA, EMA, WMA, ALMA) – Suavizam preços para mostrar a tendência.
- MACD (Moving Average Convergence Divergence) – Mede a força e a direção da tendência com médias móveis.
- ADX (Average Directional Index) – Mede a força da tendência, mas não sua direção.
- Ichimoku Kinko Hyo – Fornece suporte, resistência e direção de tendência em um único gráfico.


2. Indicadores de Momentum (Força)

Objetivo: Medir a velocidade das mudanças nos preços e identificar momentos de entrada e saída. :pino: Como Funcionam: Comparam preços atuais e passados para medir a força da tendência.

Exemplos:
- RSI (Relative Strength Index) – Mede a força do movimento de preços e detecta sobrecompra/sobrevenda.
- Estocástico – Compara o preço de fechamento com a faixa de preços passada para indicar reversões.
- CCI (Commodity Channel Index) – Mede variações de preço em relação a uma média estatística.
- Momentum – Mede a taxa de mudança do preço ao longo do tempo.


3. Indicadores de Volume

Objetivo: Analisar o volume de negociações para confirmar tendências e prever reversões. :pino: Como Funcionam: Observam se o volume aumenta ou diminui em relação aos preços.

Exemplos:
- OBV (On-Balance Volume) – Mede o fluxo de volume baseado em alta e baixa de preços.
- Volume Profile – Analisa onde o volume foi negociado em diferentes níveis de preço.
- MFI (Money Flow Index) – Indica pressão de compra/venda combinando preço e volume.
- VWAP (Volume Weighted Average Price) – Calcula a média do preço ponderada pelo volume.


4. Indicadores de Volatilidade

Objetivo: Medir a variação e a imprevisibilidade dos preços para antecipar mudanças no mercado. :pino: Como Funcionam: Analisam a amplitude dos preços e sua variação ao longo do tempo.

Exemplos:
- Bandas de Bollinger – Criam uma faixa ao redor do preço para indicar se está sobrecomprado ou sobrevendido.
- ATR (Average True Range) – Mede a volatilidade do mercado ao calcular a média da amplitude dos preços.
- Keltner Channels – Funciona como as Bandas de Bollinger, mas usa médias móveis.
- VIX (Índice de Volatilidade) – Mede a volatilidade implícita no mercado de ações.


5. Indicadores de Suporte e Resistência

Objetivo: Identificar níveis onde os preços tendem a reverter ou consolidar. :pino: Como Funcionam: Calculam pontos estratégicos com base em máximas, mínimas e médias anteriores.

Exemplos:
- Pivôs – Calculam pontos de suporte e resistência com base nos preços anteriores.
- Fibonacci Retracement – Usa proporções matemáticas para identificar zonas de suporte e resistência.
- Linhas de Tendência – Identificam níveis psicológicos importantes baseados em máximas e mínimas anteriores.


6. Indicadores de Ciclos e Estatísticos

Objetivo: Identificar padrões e ciclos de mercado que podem influenciar os preços. :pino: Como Funcionam: Utilizam cálculos matemáticos para prever mudanças de comportamento do mercado.

Exemplos:
- Elliott Wave Theory – Análise de padrões de ondas para prever ciclos de alta e baixa.
- Fourier Transform – Identifica ciclos ocultos nos preços do mercado.
- Detrended Price Oscillator (DPO) – Remove tendências para focar nos ciclos de curto prazo.


Então a ideia ou escolher uma ou duas categorias e focar em um timeframe pequeno, médio ou grande, ou fazer um completo abrangendo todas as categorias.

Mas além dos indicadores nós também podemos otimizar o valor do StopLoss e do TakeProfit, isso muda completamente a sua estratégia, vai por mim.




```go
func runQAOA(strategy Strategy, candles []Candle, initialParams map[string]float64, iterations int) map[string]float64 {
	bestParams := make(map[string]float64)
	copyParams(bestParams, initialParams)

	paramRanges := strategy.GetParamRanges()
	bestFitness := -math.MaxFloat64

	// Define os parâmetros do QAOA
	temperature := 1.0
	coolingRate := 0.95

	strategy.SetParams(initialParams)
	result := strategy.Simulate(candles)
	bestFitness = calculateFitness(result)

	for iter := 0; iter < iterations; iter++ {
		// Gera uma nova solução com perturbação quântica
		newParams := make(map[string]float64)
		for param, value := range bestParams {
			rang := paramRanges[param]
			// Aplica perturbação quântica
			delta := (rang.Max - rang.Min) * temperature * (rand.Float64()*2 - 1)
			newValue := value + delta
			newParams[param] = math.Max(math.Min(newValue, rang.Max), rang.Min)
		}

		// Avalia a nova solução
		strategy.SetParams(newParams)
		result := strategy.Simulate(candles)
		newFitness := calculateFitness(result)

		// Aceita ou rejeita a nova solução baseado na probabilidade quântica
		if newFitness > bestFitness || rand.Float64() < math.Exp((newFitness-bestFitness)/temperature) {
			bestFitness = newFitness
			copyParams(bestParams, newParams)
		}

		// Reduz a temperatura
		temperature *= coolingRate

		fmt.Printf("Iteração QAOA %d/%d - Melhor Fitness: %.2f\n", iter+1, iterations, bestFitness)
	}

	return bestParams
}
```
