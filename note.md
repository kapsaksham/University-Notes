Here are the notes on the simulation problems, with the corrected mathematical notations:

---

## Notes on Simulation Problems (Chapter 1)

This chapter introduces simulation as a technique to solve problems by mimicking a real system's behavior. We'll look at two examples to understand how it works.

### 1. Pure Pursuit Problem

**What is it?**
Imagine a fighter jet (the pursuer) chasing an enemy bomber (the target). The fighter always flies directly towards the bomber, which is moving along a known path. The goal is to determine the fighter's flight path and how long it takes to catch the bomber.

**Why simulation helps:**
If the bomber flies in a straight line, we could use standard math to solve it. But if the bomber's path is curved, exact mathematical solutions become very difficult. Simulation allows us to follow the chase step-by-step.

**How it works (simplified conditions):**

* **Flat movement:** Both planes stay in the same horizontal plane (like a 2D map).
* **Fighter's speed:** The fighter has a constant speed, denoted as $V_F$ (e.g., 20 km/minute).
* **Bomber's path:** The bomber's position is known at all times. We can represent its coordinates as $X_B(t)$ and $Y_B(t)$ at time $t$.
* **Fighter's strategy:** At fixed time intervals, $\Delta t$ (e.g., every minute), the fighter adjusts its direction to point directly at the bomber and flies in that new direction for the duration of $\Delta t$.

**Key steps in the simulation:**

1. **Coordinate system:** We use a rectangular coordinate system (X and Y axes) to track the positions of both planes.
2. **Initial positions:** We start with known positions for the fighter $(X_F(0), Y_F(0))$ and the bomber $(X_B(0), Y_B(0))$ at time $t=0$.
3. **Bomber's path:** A table or function provides $X_B(t)$ and $Y_B(t)$ for the bomber at each time step.
4. **Fighter's calculations:** For each time step, from $t$ to $t+\Delta t$:

   * Calculate the distance between the fighter and the bomber at time $t$:

     $$
     DIST(t) = \sqrt{(Y_B(t) - Y_F(t))^2 + (X_B(t) - X_F(t))^2}
     $$
   * Determine the angle $\theta$ from the fighter to the bomber at time $t$:

     $$
     \sin \theta = \frac{Y_B(t) - Y_F(t)}{DIST(t)}
     $$

     $$
     \cos \theta = \frac{X_B(t) - X_F(t)}{DIST(t)}
     $$
   * Calculate the fighter's new position at $t+\Delta t$:

     $$
     X_F(t+1) = X_F(t) + V_F \cdot \cos \theta
     $$

     $$
     Y_F(t+1) = Y_F(t) + V_F \cdot \sin \theta
     $$
5. **Stopping condition:** The simulation ends if:

   * $DIST(t) \leq 10$ km (fighter catches the bomber).
   * $t > 12$ minutes (bomber escapes).

**What we learn:**
This simulation gives us the fighter's exact attack course and the outcome of the pursuit. This is a "continuous" problem because positions change smoothly, and "deterministic" because all paths and rules are known.

---

### 2. Inventory Problem

**What is it?**
This involves managing stock (e.g., car tires) in a store using a simple ordering rule: "When stock drops to 'P' items (reorder point), order 'Q' more items (reorder quantity)."

**Challenges:**

* **Demand varies:** The number of items customers want each day is random (e.g., between 0 and 99, with equal chance for each).
* **Lag time:** There's a delay for ordered items to arrive (e.g., 3 days).
* **Costs:**

  * **Carrying cost:** Cost to store an item (e.g., Re. 0.75 per item per night).
  * **Lost sales cost:** Loss due to running out of stock (e.g., Rs. 18.00 per item).
  * **Reorder cost:** Cost to place an order (e.g., Rs. 75.00 per order).
* **Order limit:** Only one order can be in transit at a time.
* **Starting point:** Initial stock is given (e.g., 115 items), with no outstanding orders.

**Why simulation helps:**
This problem is too complex for simple formulas due to random demand and the interplay of costs and timings. Simulation allows us to test different ordering policies (different 'P' and 'Q' values) over time to find the best one.

**How it works (for a specific (P, Q) policy):**

1. **Time step:** The simulation proceeds day by day for a set period (e.g., 180 days).
2. **Daily process:** For each day, $i$:

   * **Arrivals:** Check if any outstanding orders are due to arrive. If yes, add the reorder quantity $Q$ to the current stock.
   * **Demand:** Generate a random daily demand, $DEM$.
   * **Sales/Shortage:**

     * If $DEM \leq \text{Stock}$, sell $DEM$ items. Update stock: $\text{Stock} = \text{Stock} - DEM$. Add carrying cost: $\text{Cost} = \text{Cost} + \text{Stock} \cdot 0.75$.
     * If $DEM > \text{Stock}$, sell all available items. The difference $(DEM - \text{Stock})$ is a lost sale. Add lost sales cost: $\text{Cost} = \text{Cost} + (\text{DEM} - \text{Stock}) \cdot 18.00$. Stock becomes 0.
   * **Reorder Check:** If the total inventory (stock on hand + items on order) is $\leq P$ AND no order is currently outstanding, place a new order for $Q$ items. Note its arrival date (today + 3 days) and add reorder cost: $\text{Cost} = \text{Cost} + 75.00$.
3. **Repeat:** Move to the next day.
4. **End of simulation:** After the set period (e.g., 180 days), calculate the total cost for this $(P, Q)$ policy by summing all carrying, lost sales, and reorder costs.

**What we learn:**
By simulating various $(P, Q)$ policies, we can compare their total costs and identify the policy that minimizes cost. This is a "discrete" problem because events happen at distinct points in time (day by day), and "stochastic" due to the random daily demand.

---

