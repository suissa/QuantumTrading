# QuantumTrading

Nesse repositório eu vou repassar os algoritmos quânticos que utilizo para Trading.

## QAOA (Quantum Approximate Optimization Algorithm)

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






3h27
O indicador ALMA (Arnaud Legoux Moving Average) pertence à categoria de médias móveis e, mais especificamente, às médias 

usar QAOA (Quantum Approximate Optimization Algorithm) para ajustar os parâmetros ideais de uma estratégia de trading baseada em indicadores técnicos. Isso permite encontrar combinações ótimas de médias móveis, RSI, bandas de Bollinger, entre outros, maximizando lucro e minimizando risco.
