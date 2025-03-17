![](https://i.imgur.com/8n7z5yH.png)


# QuantumTrading

In this repository, I will share the quantum algorithms that I use for Trading.

## QAOA (Quantum Approximate Optimization Algorithm)

QAOA (Quantum Approximate Optimization Algorithm) is a hybrid quantum-classical algorithm developed to solve combinatorial optimization problems, such as financial portfolio problems, routing, trading selection, and others. It works by applying alternating layers of quantum operations to approximate the best possible solution.

## QAOA Fundamentals
QAOA is designed to solve QUBO (Quadratic Unconstrained Binary Optimization) problems and problems modeled as an Ising Hamiltonian, which can be described by:
Algorithm Structure
QAOA combines quantum computing and classical optimization. It follows these steps:

1. Define the problem as a Hamiltonian (Cost Hamiltonian).
2. Choose a Mixer Hamiltonian to explore the solution space.
3. Build a parameterized quantum circuit with alternating layers of gates based on these Hamiltonians.
4. Execute the circuit and measure the results, adjusting parameters with a classical optimizer.

## Key Algorithm Components

QAOA has three main parts:

- Cost Hamiltonian: Represents the optimization problem.
- Mixer Hamiltonian: Explores possible solutions.
- Variational Parameters: Adjust the circuit evolution.

## Hamiltonian Definition

The two Hamiltonians are applied iteratively in the quantum circuit.

- Cost Hamiltonian: This Hamiltonian encodes the objective function of the problem. If we're solving a QUBO problem, it is defined as:
where:

![Cost Hamiltonians](https://quicklatex.com/cache3/92/ql_77c199a9317eef6d78cab55f77cd6a92_l3.png)

𝑄𝑖𝑗: are the QUBO matrix coefficients.
𝑍𝑖: is the Pauli-Z operator applied to qubit 
- The lowest energy state of the quantum system will represent the best solution.

### What does the Pauli-X Operator do?

The Pauli-X operator (𝑋) acts as a switch, swapping qubit states:

- If a qubit is in state ∣0⟩, Pauli-X transforms it to ∣1⟩.
- If a qubit is in state ∣1⟩, Pauli-X transforms it to ∣0⟩.

This means that the Mixer Hamiltonian acts by changing the values of variables in the system, allowing the exploration of new solutions.

- Mixer Hamiltonian: The Mixer allows exploration of the solution space. 

It is defined as:

![Mixer Hamiltonian](https://quicklatex.com/cache3/47/ql_075ae5b341295dfd833e1ad53a19c447_l3.png)

𝑋𝑖: is the Pauli-X operator applied to qubit 
- This Hamiltonian rotates the qubits, allowing exploration of different configurations.

The formula calculates the total energy of a variable configuration, taking into account both interactions between variables and the effect of each variable in isolation, helping QAOA find the best possible solution for an optimization problem.

## Quantum Circuit Construction

The QAOA circuit is assembled by applying alternating layers of 𝐻C and 𝐻M, adjusted by variational parameters (𝛾,𝛽).

1. Initialization
- We prepare a uniform superposed initial state

2. Cost Hamiltonian Application
- We apply controlled phase gates, encoding the objective function in the quantum system.

3. Mixer Hamiltonian Application
- We apply rotations, allowing the system to explore different solutions.

4. Measurement
- The circuit is measured in computational basis ∣0⟩,∣1⟩ to find the optimal solution.

5. Classical Optimization
- We adjust parameters γ,β using a classical optimizer (e.g., COBYLA, SPSA, Nelder-Mead).
- We repeat the process until finding the best value.

### What is the Pauli-Z Operator?
The Pauli-Z (𝑍) is one of the three Pauli operators, fundamental in quantum mechanics and quantum computing. It is represented by the Pauli-Z matrix:
[ 1 0
  0−1]
  
This operator acts on qubits, altering their state in a specific way.

- How does Pauli-Z work in quantum computing?
The Z operator doesn't change the ∣0⟩ state, but inverts the sign of the ∣1⟩ state:

When applied to a qubit in state ∣0⟩, the qubit remains the same: Z∣0⟩ = ∣0⟩
When applied to a qubit in state ∣1⟩, the qubit changes sign: Z∣1⟩ = −∣1⟩
This means the operator introduces a negative phase in the ∣1⟩ qubit but doesn't affect ∣0⟩.

- Physical Interpretation of Pauli-Z
The Pauli-Z operator is directly related to the Z-axis of the Bloch sphere, which represents quantum states geometrically.

- ∣0⟩ is at the top of the Bloch sphere and is not affected by Pauli-Z.
- ∣1⟩ is at the bottom of the Bloch sphere and receives a -1 phase factor when Pauli-Z is applied.

This means the Pauli-Z operator reflects qubits around the Z-axis.

#### Use of Pauli-Z in Quantum Computing

The Pauli-Z operator appears in various contexts, including:

- Quantum Error Correction: Pauli-Z can be used to detect and correct certain types of errors in qubits.

#### Measurements in Quantum Circuits

Many measurements in quantum computing are made in the Z basis, where Pauli-Z helps distinguish between ∣0⟩ and ∣1⟩.

#### QAOA and Ising Models

In the Quantum Approximate Optimization Algorithm (QAOA), the Pauli-Z operator is used to define constraints and interactions between qubits, modeling problems such as financial optimization and logistic problems.
4️⃣ Phase Gates (RZ Gate)

The Pauli-Z operator can be seen as a special case of the RZ(θ) gate, used to add phases in quantum circuits.

In QAOA, it appears in the Cost Hamiltonian to define relationships between variables, helping solve problems like trading and financial portfolio.

## Implementation Example in Qiskit

```py
from qiskit import Aer
from qiskit.optimization import QuadraticProgram
from qiskit_optimization.translators import from_docplex_mp
from qiskit.algorithms import QAOA
from qiskit.primitives import Estimator
from qiskit.algorithms.optimizers import COBYLA
from qiskit_optimization.algorithms import MinimumEigenOptimizer
from docplex.mp.model import Model

# Create optimization problem (QUBO)
mdl = Model('QAOA_Optimization')
x1 = mdl.binary_var(name='x1')
x2 = mdl.binary_var(name='x2')
x3 = mdl.binary_var(name='x3')

# Define objective function (minimize -x1x2 - x2x3 + x1)
mdl.maximize(-x1 * x2 - x2 * x3 + x1)

# Convert to QUBO
qp = from_docplex_mp(mdl)

# Create simulator and QAOA optimizer
estimator = Estimator()
qaoa = QAOA(estimator, optimizer=COBYLA())

# Solve the problem
optimizer = MinimumEigenOptimizer(qaoa)
result = optimizer.solve(qp)

# Display solution
print("Best Solution:", result.x)
print("Optimal Value:", result.fval)
```

## Code Explanation
- We create a QUBO problem with variables 
- We define the objective function.
- We convert to a quantum Hamiltonian.
- We apply QAOA to find the solution.
- We measure the qubits and adjust parameters with a classical optimizer.

## QAOA Applications

QAOA can be used to optimize:

- Algorithmic Trading → Adjustment of indicator parameters like RSI, MACD, Bollinger Bands.
- Portfolio Selection → Choose assets maximizing return and minimizing risk.
- Logistic Routing → Find the most efficient delivery path.
- Resource Allocation → Choose the best investment distribution.

| Algorithm | Characteristics |
------------|-----------------|
| QAOA | Approximates combinatorial solutions using quantum circuits |
| Branch & Bound | Divides the problem into subproblems and explores solutions |
| Genetic Algorithms | Evolves solutions through mutation and selection |
| Linear Programming | Solves problems with linear constraints |

How does QAOA know it found an optimized solution?
QAOA (Quantum Approximate Optimization Algorithm) optimizes a problem based on minimizing the energy of the Cost Hamiltonian (𝐻𝐶). But how does it know it's optimized? It follows three main criteria:

1. Measures the qubits and verifies which state appears most frequently

At the end of the quantum circuit execution, the qubits are measured. 
QAOA repeats the experiment several times and records which states appear most frequently.

- If a certain state appears many times, this indicates it has the lowest possible energy → means it is probably the best solution.

2. Measures the solution's energy

QAOA calculates the energy associated with the most frequent state.

The energy is given by:

![](https://quicklatex.com/cache3/2c/ql_d51cd2b3088c1d8af440aaf90200652c_l3.png)

3. The Variational Algorithm Adjusts Parameters

QAOA uses classical optimization to find the best values of 𝛾 and 𝛽, which control the circuit evolution.

- The circuit starts with an initial guess of 𝛾 and 𝛽.
- Executes and measures the qubits.
- Calculates the energy of the found state.
- Adjusts 𝛾 and 𝛽 to further reduce energy.
- Repeats until finding the best solution.

- It optimizes like a chess player learning from mistakes: if the previous move wasn't good, it tries another until finding the winning strategy.

How to know if the found solution is good?

- The most frequent state among measurements indicates the best solution.
- If the system's energy is minimal, then this is probably the best possible configuration.
- The classical optimizer adjusts parameters until energy no longer decreases.
- We compare with classical solutions to see if QAOA's answer matches exact solutions.

---

Now that you've learned a lot about Quantum Optimization, it's time to choose the indicators to be optimized. In the world of technical analysis, we have 6 categories of indicators:

1. Trend Indicators

Objective: Identify the predominant market direction (uptrend, downtrend, or sideways). 
How They Work: Calculate averages or smooth prices to detect trends and reversals.

Examples:
- Moving Averages (SMA, EMA, WMA, ALMA) – Smooth prices to show trend.
- MACD (Moving Average Convergence Divergence) – Measures trend strength and direction with moving averages.
- ADX (Average Directional Index) – Measures trend strength, but not direction.
- Ichimoku Kinko Hyo – Provides support, resistance, and trend direction in a single chart.

2. Momentum Indicators

Objective: Measure the speed of price changes and identify entry and exit points. 
How They Work: Compare current and past prices to measure trend strength.

Examples:
- RSI (Relative Strength Index) – Measures price movement strength and detects overbought/oversold conditions.
- Stochastic – Compares closing price with past price range to indicate reversals.
- CCI (Commodity Channel Index) – Measures price variations relative to a statistical average.
- Momentum – Measures the rate of price change over time.

3. Volume Indicators

Objective: Analyze trading volume to confirm trends and predict reversals. 
How They Work: Observe if volume increases or decreases relative to prices.

Examples:
- OBV (On-Balance Volume) – Measures volume flow based on price ups and downs.
- Volume Profile – Analyzes where volume was traded at different price levels.
- MFI (Money Flow Index) – Indicates buying/selling pressure combining price and volume.
- VWAP (Volume Weighted Average Price) – Calculates volume-weighted average price.

4. Volatility Indicators

Objective: Measure price variation and unpredictability to anticipate market changes. 
How They Work: Analyze price range and its variation over time.

Examples:
- Bollinger Bands – Create a range around price to indicate overbought or oversold conditions.
- ATR (Average True Range) – Measures market volatility by calculating average price range.
- Keltner Channels – Works like Bollinger Bands but uses moving averages.
- VIX (Volatility Index) – Measures implied volatility in the stock market.

5. Support and Resistance Indicators

Objective: Identify levels where prices tend to reverse or consolidate. 
How They Work: Calculate strategic points based on previous highs, lows, and averages.

Examples:
- Pivots – Calculate support and resistance points based on previous prices.
- Fibonacci Retracement – Uses mathematical ratios to identify support and resistance zones.
- Trend Lines – Identify important psychological levels based on previous highs and lows.

6. Cycle and Statistical Indicators

Objective: Identify market patterns and cycles that can influence prices. 
How They Work: Use mathematical calculations to predict market behavior changes.

Examples:
- Elliott Wave Theory – Wave pattern analysis to predict up and down cycles.
- Fourier Transform – Identifies hidden cycles in market prices.
- Detrended Price Oscillator (DPO) – Removes trends to focus on short-term cycles.

So the idea is to either choose one or two categories and focus on a small, medium, or large timeframe, or do a complete analysis covering all categories.

But besides indicators, we can also optimize the StopLoss and TakeProfit values, which completely changes your strategy, trust me.

I chose 4 basic indicators as an example:

- MACD
- RSI
- OBV
- ATR

And this is our Quantum Optimization code. I recommend you run this code on IBM Quantum.

```py
import numpy as np
import requests
import pandas as pd
import ta
import matplotlib.pyplot as plt
from datetime import datetime
from qiskit import Aer
from qiskit.algorithms import QAOA
from qiskit.primitives import Estimator
from qiskit.algorithms.optimizers import COBYLA
from qiskit_optimization import QuadraticProgram
from qiskit_optimization.algorithms import MinimumEigenOptimizer
from docplex.mp.model import Model
import random

# - List to store opened/closed orders logs
trade_log = []

# - Function to fetch BTC/USDT data in 5m from Binance
def get_binance_data(symbol="BTCUSDT", interval="5m", limit=500):
    url = f"https://api.binance.com/api/v3/klines?symbol={symbol}&interval={interval}&limit={limit}"
    response = requests.get(url).json()
    df = pd.DataFrame(response, columns=['time', 'open', 'high', 'low', 'close', 'volume', 
                                         'close_time', 'qav', 'trades', 'taker_base', 'taker_quote', 'ignore'])
    df = df[['time', 'open', 'high', 'low', 'close', 'volume']].astype(float)
    df['time'] = pd.to_datetime(df['time'], unit='ms')
    return df

# - Calculate technical indicators
def calculate_indicators(df, macd_short=12, macd_long=26, macd_signal=9, rsi_length=14, atr_length=14):
    df['MACD'] = ta.trend.macd(df['close'], window_slow=macd_long, window_fast=macd_short)
    df['RSI'] = ta.momentum.RSIIndicator(df['close'], window=rsi_length).rsi()
    df['OBV'] = ta.volume.OnBalanceVolumeIndicator(df['close'], df['volume']).on_balance_volume()
    df['ATR'] = ta.volatility.AverageTrueRange(df['high'], df['low'], df['close'], window=atr_length).average_true_range()
    return df

# - Strategy simulation with log of operations
def simulate(params, df):
    """ Simulates the trading strategy and returns final profit """
    balance = 1000  # Initial fictional balance
    position = None  # Indicates if we are long or short
    entry_price = None  # Operation entry price

    for i in range(1, len(df)):
        time = df.iloc[i]['time']
        price = df.iloc[i]['close']

        # - Entry rules
        if df.iloc[i]['MACD'] > df.iloc[i-1]['MACD'] and df.iloc[i]['RSI'] < 30:
            if position is None:  # Buy
                position = "LONG"
                entry_price = price
                trade_log.append({
                    "Entry Date": time,
                    "Side": position,
                    "Entry Price": entry_price,
                    "MACD": params['MACD_Short'],
                    "RSI": params['RSI_Length'],
                    "ATR": params['ATR_Length'],
                })

        elif df.iloc[i]['MACD'] < df.iloc[i-1]['MACD'] and df.iloc[i]['RSI'] > 70:
            if position is None:  # Sell
                position = "SHORT"
                entry_price = price
                trade_log.append({
                    "Entry Date": time,
                    "Side": position,
                    "Entry Price": entry_price,
                    "MACD": params['MACD_Short'],
                    "RSI": params['RSI_Length'],
                    "ATR": params['ATR_Length'],
                })

        # - Exit rules
        if position == "LONG" and df.iloc[i]['RSI'] > 50:
            profit = (price - entry_price) / entry_price * 100
            trade_log[-1].update({"Close Date": time, "Close Price": price, "Profit (%)": profit})
            balance += balance * (profit / 100)
            position = None

        elif position == "SHORT" and df.iloc[i]['RSI'] < 50:
            profit = (entry_price - price) / entry_price * 100
            trade_log[-1].update({"Close Date": time, "Close Price": price, "Profit (%)": profit})
            balance += balance * (profit / 100)
            position = None

    return {"FinalProfit": balance}

# - Creating a QUBO problem to optimize indicators
def create_qubo_problem():
    mdl = Model('QAOA_Trading')

    # - Adjustment variables for each indicator
    macd_short = mdl.integer_var(lb=5, ub=20, name='MACD_Short')
    macd_long = mdl.integer_var(lb=20, ub=50, name='MACD_Long')
    macd_signal = mdl.integer_var(lb=5, ub=15, name='MACD_Signal')
    rsi_length = mdl.integer_var(lb=7, ub=21, name='RSI_Length')
    atr_length = mdl.integer_var(lb=7, ub=21, name='ATR_Length')

    # - Objective function: Maximize final strategy profit
    mdl.maximize(
        0.4 * macd_short + 0.3 * macd_long + 0.2 * macd_signal +
        0.5 * rsi_length + 0.3 * atr_length
    )

    # - Conversion to QUBO
    qp = QuadraticProgram()
    qp.from_docplex(mdl)
    return qp

# - Function to run QAOA and find best indicator configuration
def run_qaoa(df, iterations=5):
    qp = create_qubo_problem()

    # - Configure QAOA
    estimator = Estimator()
    qaoa = QAOA(estimator, optimizer=COBYLA(), reps=iterations)
    optimizer = MinimumEigenOptimizer(qaoa)

    # - Execute QAOA
    result = optimizer.solve(qp)

    # - Best parameters found
    best_params = {
        "MACD_Short": result.x[0],
        "MACD_Long": result.x[1],
        "MACD_Signal": result.x[2],
        "RSI_Length": result.x[3],
        "ATR_Length": result.x[4]
    }

    # - Simulate strategy with best parameters
    simulation_result = simulate(best_params, df)
    
    # - Display final strategy report
    print("\n📌 **Best Parameters Found:**", best_params)
    print("📈 **Simulated Final Profit:**", simulation_result["FinalProfit"])
    
    # - Display order logs
    trade_df = pd.DataFrame(trade_log)
    print("\n📊 **Orders Report:**")
    print(trade_df)
    
    # - Show orders graph
    trade_df.plot(x="Entry Date", y="Profit (%)", kind="bar", title="Profit per Trade")
    plt.show()
```

Log:

```
✅ 1000 candles loaded successfully!
🚀 Starting QAOA...
🔄 Running optimization...

**Best Parameters Found:** {'MACD_Short': np.float64(1.0), 'MACD_Long': np.float64(1.0), 'MACD_Signal': np.float64(1.0), 'RSI_Length': np.float64(1.0), 'ATR_Length': np.float64(1.0)}
📈 **Simulated Final Profit:** 1003.8771479209456
```

As a bonus, I'll give you the implementation with dynamic Take Profit and Stop Loss:

```py
import numpy as np
import requests
import pandas as pd
import ta
import matplotlib.pyplot as plt
from datetime import datetime
from qiskit_optimization.translators import from_docplex_mp
from qiskit_optimization import QuadraticProgram
from qiskit_optimization.algorithms import MinimumEigenOptimizer
from qiskit_algorithms import QAOA
from qiskit.primitives import Sampler
from qiskit_algorithms.optimizers import COBYLA
from qiskit_optimization.converters import QuadraticProgramToQubo
from docplex.mp.model import Model
import random

# 🔹 List to store opened/closed orders logs
trade_log = []

# 🔹 Function to fetch BTC/USDT data in 5m from Binance
def get_binance_data(symbol="BTCUSDT", interval="5m", limit=1000):
    url = f"https://api.binance.com/api/v3/klines?symbol={symbol}&interval={interval}&limit={limit}"
    try:
        response = requests.get(url, timeout=10)
        response.raise_for_status()  # Raises error if response is not 200
        df = pd.DataFrame(response.json(), columns=['time', 'open', 'high', 'low', 'close', 'volume',
                                                     'close_time', 'qav', 'trades', 'taker_base', 'taker_quote', 'ignore'])
        df = df[['time', 'open', 'high', 'low', 'close', 'volume']].astype(float)
        df['time'] = pd.to_datetime(df['time'], unit='ms')
        print(f"✅ {len(df)} candles loaded successfully!")
        return df
    except requests.exceptions.RequestException as e:
        print(f"❌ Error fetching data from Binance: {e}")
        return None

# 🔹 Calculate technical indicators
def calculate_indicators(df, macd_short=12, macd_long=26, macd_signal=9, rsi_length=14, atr_length=14):
    df['MACD'] = ta.trend.macd(df['close'], window_slow=macd_long, window_fast=macd_short)
    df['RSI'] = ta.momentum.RSIIndicator(df['close'], window=rsi_length).rsi()
    df['OBV'] = ta.volume.OnBalanceVolumeIndicator(df['close'], df['volume']).on_balance_volume()
    df['ATR'] = ta.volatility.AverageTrueRange(df['high'], df['low'], df['close'], window=atr_length).average_true_range()
    return df

# 🔹 Strategy simulation with dynamic TP and SL
def simulate(params, df):
    """ Simulates trading strategy and returns final profit with dynamic TP and SL """
    balance = 1000  # Initial fictional balance
    position = None  # Indicates if we are long or short
    entry_price = None  # Operation entry price
    take_profit = None
    stop_loss = None

    for i in range(1, len(df)):
        time = df.iloc[i]['time']
        price = df.iloc[i]['close']
        atr = df.iloc[i]['ATR']

        # 🔹 Entry rules
        if position is None:
            if df.iloc[i]['MACD'] > df.iloc[i-1]['MACD'] and df.iloc[i]['RSI'] < 30:
                position = "LONG"
                entry_price = price
                take_profit = price + (atr * 2)  # Dynamic TP based on ATR
                stop_loss = price - (atr * 1.5)  # Dynamic SL based on ATR
                trade_log.append({
                    "Entry Date": time,
                    "Side": position,
                    "Entry Price": entry_price,
                    "Take Profit": take_profit,
                    "Stop Loss": stop_loss,
                    "MACD": params['MACD_Short'],
                    "RSI": params['RSI_Length'],
                    "ATR": atr,
                })

            elif df.iloc[i]['MACD'] < df.iloc[i-1]['MACD'] and df.iloc[i]['RSI'] > 70:
                position = "SHORT"
                entry_price = price
                take_profit = price - (atr * 2)
                stop_loss = price + (atr * 1.5)
                trade_log.append({
                    "Entry Date": time,
                    "Side": position,
                    "Entry Price": entry_price,
                    "Take Profit": take_profit,
                    "Stop Loss": stop_loss,
                    "MACD": params['MACD_Short'],
                    "RSI": params['RSI_Length'],
                    "ATR": atr,
                })

        # 🔹 Exit rules (TP or SL)
        if position == "LONG":
            if price >= take_profit or price <= stop_loss:
                profit = (price - entry_price) / entry_price * 100
                trade_log[-1].update({"Close Date": time, "Close Price": price, "Profit (%)": profit})
                balance += balance * (profit / 100)
                position = None

        elif position == "SHORT":
            if price <= take_profit or price >= stop_loss:
                profit = (entry_price - price) / entry_price * 100
                trade_log[-1].update({"Close Date": time, "Close Price": price, "Profit (%)": profit})
                balance += balance * (profit / 100)
                position = None

    return {"FinalProfit": balance}

def create_qubo_problem():
    mdl = Model('QAOA_Trading')

    macd_short = mdl.binary_var(name='MACD_Short')
    macd_long = mdl.binary_var(name='MACD_Long')
    macd_signal = mdl.binary_var(name='MACD_Signal')
    rsi_length = mdl.binary_var(name='RSI_Length')
    atr_length = mdl.binary_var(name='ATR_Length')

    mdl.maximize(
        10 * macd_short + 15 * macd_long + 7 * macd_signal +
        12 * rsi_length + 9 * atr_length
    )

    problem = from_docplex_mp(mdl)

    return problem

def run_qaoa(df, iterations=5):
    qp = create_qubo_problem()

    qubo_converter = QuadraticProgramToQubo()
    qubo_problem = qubo_converter.convert(qp)

    print("🚀 Starting QAOA...")
    sampler = Sampler()
    qaoa = QAOA(sampler, optimizer=COBYLA(), reps=iterations)
    optimizer = MinimumEigenOptimizer(qaoa)

    print("🔄 Running optimization...")
    result = optimizer.solve(qubo_problem)

    
    best_params = {qp.variables[i].name: result.x[i] for i in range(len(result.x))}

    print("\n📌 **Best Parameters Found:**", best_params)

    simulation_result = simulate(best_params, df)

    print("📈 **Simulated Final Profit:**", simulation_result["FinalProfit"])



df = get_binance_data()
if df is not None:
    df = calculate_indicators(df)
    run_qaoa(df, iterations=10)

```

![see you](https://i.imgur.com/YNns6u1.png)

Now you can just venture into testing various indicator combinations.

![Quantum Hive Fund](https://i.imgur.com/oTW3BMs.pngg)

[Quantum Hive Fund site](https://quantumhivefund.com)
