# 📊 Limit Order Book Simulation with Agent-Based Modelling

## 1. Limit Order Book (LOB) Environment

We simulate a **continuous double-auction limit order book (LOB)** where agents submit buy/sell limit orders.

### 🔁 Matching Rules

* Buy executes if:
  [
  P_{\text{buy}} \geq \text{best ask}
  ]
* Sell executes if:
  [
  P_{\text{sell}} \leq \text{best bid}
  ]

### 📈 Mid Price

[
M_t = \frac{\text{best bid}_t + \text{best ask}_t}{2}
]

---

## 🔧 Simulation Parameters (ZIT Baseline)

| Parameter    | Value       | Description         |
| ------------ | ----------- | ------------------- |
| ( N )        | 500         | Number of agents    |
| ( P_f )      | 10,000      | Fundamental price   |
| ( S )        | 1,000       | Private value range |
| ( \delta P ) | 1           | Tick size           |
| ( t_c )      | 10,000      | Order expiry        |
| ( T_E )      | 100,000     | Simulation horizon  |
| ( L )        | 200         | Warm-start levels   |
| ( \Delta )   | 1           | Price step          |
| ( D )        | 5           | Depth per level     |
| ( B )        | 1% of (T_E) | Burn-in             |
| ( R )        | 30          | Number of runs      |

---

## 2. Zero-Intelligence Traders (ZIT)

ZIT agents follow **random but rational (no-loss) rules**.

### 📐 Order Generation

Buyers:
[
v_i \sim U(P_f, P_f + S), \quad b_i \sim U(P_{\min}, v_i)
]

Sellers:
[
c_i \sim U(P_f - S, P_f), \quad a_i \sim U(c_i, P_{\max})
]

✔ No optimization
✔ No learning
✔ Pure randomness under constraints

---

## 3. Behavioural Traders (BT)

Behavioural traders form expectations using **fundamental + technical + noise signals**.

---

## 📐 Expected Return Model (Core Formula)

[
r^{t}*{e,j} = \frac{1}{\sum_i w*{i,j}} \left(
w_{1,j} \log \frac{P_f}{P^{t-1}} +
w_{2,j} \log \frac{P^{t-1}}{P^{t-\tau_j-1}} +
w_{3,j} \epsilon^t_j
\right)
]

---

## 🔍 Explanation of Each Term

| Term             | Meaning                            |
| ---------------- | ---------------------------------- |
| ( j )            | Agent index                        |
| ( t )            | Time step                          |
| ( P_f )          | Fundamental price                  |
| ( P^{t-1} )      | Current market price               |
| ( \tau_j )       | Lookback window (momentum horizon) |
| ( \epsilon^t_j ) | Noise (random shock)               |
| ( w_{1,j} )      | Fundamental weight                 |
| ( w_{2,j} )      | Technical weight                   |
| ( w_{3,j} )      | Noise weight                       |

---

## 🧠 Intuition Behind the Formula

The expected return is a **weighted combination of three signals**:

### 1. Fundamental Component

[
\log \frac{P_f}{P^{t-1}}
]

* If price < fundamental → **buy signal**
* If price > fundamental → **sell signal**

---

### 2. Technical (Momentum) Component

[
\log \frac{P^{t-1}}{P^{t-\tau_j-1}}
]

* Captures **trend-following behavior**
* If price is rising → positive momentum → buy

---

### 3. Noise Component

[
\epsilon^t_j \sim \mathcal{N}(0, \sigma_\epsilon)
]

* Represents randomness / imperfect information
* Adds unpredictability

---

### ⚖️ Normalization

[
\frac{1}{\sum_i w_{i,j}}
]

Ensures the total weight is normalized.

---

## 📈 Expected Price

Once expected return is computed:

[
P^t_{e,j} = P^{t-1} \cdot \exp(r^t_{e,j})
]

➡️ This converts expected return into a **future price estimate**

---

## 🔧 Behavioural Model Parameters

| Parameter           | Value  | Description            |
| ------------------- | ------ | ---------------------- |
| ( w_{1,\max} )      | 1      | Fundamental weight cap |
| ( w_{2,\max} )      | 10     | Technical weight cap   |
| ( w_{3,\max} )      | 1      | Noise weight cap       |
| ( \tau_{\max} )     | 10,000 | Max lookback           |
| ( \sigma_\epsilon ) | 0.03   | Noise volatility       |
| ( P_d )             | 1,000  | Price interval         |

---

## 4. Return Construction

Returns are computed using **non-overlapping windows**:

[
r_k = \ln \left( \frac{M_{B+100k}}{M_{B+100(k-1)}} \right)
]

---

## 📊 Stylized Facts

We evaluate:

### ✔ Excess Kurtosis (Fat Tails)

Measures extreme returns

---

### ✔ Volatility Clustering

[
\text{ACF}*\ell = \text{Corr}(r_t^2, r*{t-\ell}^2)
]

for lags ( \ell = 1,...,6 )

---

## 5. Real Market Benchmark

Compared with:

* **S&P 500 (2013–2018)**

Metrics:

* Kurtosis
* ACF of squared returns

---

## 6. Learning Process

Agents adapt weights dynamically.

### 📐 Learning Rule

Weights are updated based on signal agreement:

* If signals agree → increase weight
* If signals disagree → decrease weight

Example update:

[
w_{i,j} \leftarrow w_{i,j} + k \cdot r'*{i,j} \cdot w*{i,j}
]

or

[
w_{i,j} \leftarrow w_{i,j} - k \cdot r'*{i,j} \cdot w*{i,j}
]

---

### 🔧 Learning Parameters

| Parameter | Value  | Description       |
| --------- | ------ | ----------------- |
| ( k_j )   | 4      | Learning rate     |
| ( m_j )   | 0.01   | Noise probability |
| ( t_l )   | 10,000 | Evaluation window |

---


If you want next level (very important for jobs), I can:

* 🔥 Turn this into a **clean GitHub README with diagrams**
* 🎯 Convert this into **CV bullet (top 1% quality)**
* 💼 Prepare how to **explain this in interviews (very important)**
