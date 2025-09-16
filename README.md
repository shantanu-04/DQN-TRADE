# DQN-Based Trading Agent

This project demonstrates how **Reinforcement Learning (RL)** can be applied to algorithmic trading using a **Deep Q-Network (DQN)**.
The agent learns to make trading decisions (Buy, Sell, or Sit) by interacting with historical stock data.

The goal is not to beat the market but to show how reinforcement learning can be set up for trading, trained on real data, and produce meaningful outputs.

---

## Project Workflow

1. **Data loading:**
   Historical stock price data is fetched (Apple stock in this example) and saved into a CSV file.

2. **Exploratory Data Analysis (EDA):**
   Plots of closing prices, daily returns, and moving averages help understand the stock’s behavior before training.

3. **Helper functions:**
   Functions for state representation, profit formatting, and result visualization.

4. **Agent class:**
   Implements Deep Q-Learning with a neural network, memory, epsilon-greedy policy, and experience replay.

5. **Training loop:**
   Simulates trading on historical data, collects rewards, stores experiences, and trains the agent.

6. **Results:**
   Profits per trade and total cumulative profit are plotted.

---

## Mathematical Context

* **State (s):** recent price changes (a sliding window of past days).
* **Actions (a):** {0 = Sit, 1 = Buy, 2 = Sell}.
* **Reward (r):** profit from a Sell action (set to 0 if trade is a loss).
* **Experience:**

  $$
  (s, a, r, s', \text{done})
  $$

### Q-Learning

The agent learns the Q-function:

$$
Q(s,a) = \text{expected future reward for taking action } a \text{ in state } s
$$

Bellman update rule:

$$
Q_\text{target} =
\begin{cases}
r & \text{if episode ends} \\
r + \gamma \max_{a'} Q(s', a') & \text{otherwise}
\end{cases}
$$

* $\gamma$: discount factor for future rewards.

### Deep Q-Network

* Instead of a Q-table, a **neural network** approximates Q(s,a).
* Input: market state.
* Output: Q-values for Sit, Buy, Sell.
* Training objective: minimize mean squared error (MSE) between predicted Q and target Q.

### Epsilon-Greedy Policy

Action choice:

$$
a =
\begin{cases}
\text{random action} & \text{with probability } \epsilon \\
\arg\max_a Q(s,a) & \text{with probability } 1 - \epsilon
\end{cases}
$$

* Start with $\epsilon = 1.0$ (pure exploration).
* Decay epsilon until a minimum value, so the agent becomes more exploitative over time.

---

## Exploratory Data Analysis (EDA)

EDA provides insights into the stock before training:

* **Closing price trend** (overall movement of the stock).
* **Daily returns** (volatility and distribution of gains/losses).
* **Moving averages** (short-term vs long-term trends).

These plots help in understanding what features or windows may be useful for training the agent.

---

## Training Loop

1. The agent observes the current market state.
2. It chooses an action (Sit, Buy, or Sell) using epsilon-greedy policy.
3. If Buy: stock price is added to inventory.
4. If Sell: profit is calculated and given as reward.
5. Experience is stored in memory.
6. The agent trains on minibatches of past experiences using the Bellman update.
7. After episodes, cumulative profit is reported and profits per trade are plotted.

---

## Output

* Console logs of Buy and Sell actions with profits.
* Final cumulative profit.
* Plot of profit per trade.
