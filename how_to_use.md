# TexasSolver GUI — How to Use It

### The Basic Workflow

```
Set Ranges → Set Board → Set Stack Sizes → Configure Bet Sizes → Build Tree → Solve → View Results
```

---

### Step 1: Set Hand Ranges

Two ranges are required — **OOP** (out of position, acts first) and **IP** (in position, acts last).

**Range format:**
```
AA,KK,QQ,JJ,TT,99:0.75,88:0.75,77:0.5,AKs,AQs,AJs,ATs
```
- No suffix = 100% frequency (always in range)
- `:0.75` = 75% frequency (mixed strategy input)
- Suits: `s`=spades, `h`=hearts, `d`=diamonds, `c`=clubs
- `AKs` = suited only, `AKo` = offsuit only, `AK` = both

Use the **Select OOP / Select IP** buttons to open a visual hand matrix picker.

---

### Step 2: Set the Board

Enter 3 cards (flop), 4 (turn), or 5 (river):
```
Qs,Jh,2h
```
Use the **Select Board Card** button for a visual card picker.

---

### Step 3: Set Stack Sizes

| Field | Example | Meaning |
|---|---|---|
| **Pot** | `10` | Current pot size in chips |
| **Effective Stack** | `95` | Chips remaining per player |
| **All-in Threshold** | `1.0` | Auto-add all-in when SPR ≤ this value |

SPR (Stack-to-Pot Ratio) = Stack / Pot. At SPR=1, the remaining stack equals the pot.

---

### Step 4: Configure Bet Sizes

Set for each street (Flop/Turn/River) and each position (IP/OOP):

| Field | Example | Meaning |
|---|---|---|
| **Bet Size** | `100` | 100% pot-sized bet |
| **Raise Size** | `50` | 50% of the bet as a raise |
| **Donk Bet** | `75` | OOP leads out (turn/river only) |
| **All-in** | checkbox | Allow shove as an option |

Multiple sizes separated by commas: `33,50,100` gives the solver three bet options.

---

### Step 5: Solver Settings

| Setting | Typical Value | Meaning |
|---|---|---|
| **Threads** | `6` | Match your CPU core count |
| **Accuracy** | `0.3` | Stop at 0.3% exploitability |
| **Max Iterations** | `200` | Hard cap on CFR passes |
| **Print Interval** | `10` | Log progress every 10 iterations |
| **Use Isomorphism** | checked | Speeds up solve via card symmetry |

---

### Step 6: Build Tree → Solve

1. Click **Build Tree** — generates the game tree, shows estimated memory usage
2. Click **Solve** — starts CFR iterations, logs progress at the bottom
3. Watch exploitability drop in the log toward your accuracy target
4. Click **Stop** at any time — the current strategy is still usable
5. Click **Show Result** to open the strategy explorer

---

### Step 7: Reading the Results

The strategy explorer shows a tree view. At each node you'll see:

- **Actions available**: Bet, Check, Raise, Call, Fold — each as a percentage
- **Per-hand breakdown**: For each hand combo (e.g. `AsAh`), the frequency of each action
- **EV values**: Expected value for each player at each node

**Export:** Use **Save Strategy** to dump the full result to a JSON file for external analysis.

---

### Example Scenario (from the benchmark)

```
Pot:             10
Effective Stack: 95     (SPR = 9.5, deep stack)
Board:           Qs Jh 2h   (flop)
OOP Range:       AA,KK,QQ,JJ,TT,99:0.75,88:0.75...
IP Range:        QQ:0.5,JJ:0.75,TT,99,88,77...
Bet sizes:       100% pot bets, 50% raises, all-in enabled
Threads:         6
Accuracy:        0.3%
```

This replicates the PioSolver benchmark — converges in ~172 seconds to 0.275% exploitability.

---

### Saving/Loading Configurations

- **Export Parameters** — saves current settings to a `.km` file in the parameters folder
- **Import Parameters** — loads a previously saved `.km` file
- Example `.km` files are in `resources/gametree/`
