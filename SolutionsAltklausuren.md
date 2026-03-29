# 🏆 DSEA Exam Solutions: The "Chewed" Walkthroughs

This document contains step-by-step, trap-dodging solutions for the old exams (Altklausuren). 
Always read "The Trap" before looking at "The Solution" to understand *why* the professor asked the question!

---

## 📝 Probeklausur 2025/2026

### Aufgabe 1: Kurze Fragen (16 Punkte)

#### 1(a) Graph Laufzeitvergleich
**The Problem:** You have a graph $G=(V, E)$ where every node has a *maximum* of $d$ neighbors ($d$ is a constant number). Which algorithm is asymptotically better: Algorithmus A with $O(|V| + |E|)$ or Algorithmus B with $O(|V| \log |V|)$?

*   **The Trap:** Usually, $|E|$ can be as big as $|V|^2$ in a dense graph, which would make $O(|V| + |E|)$ worse. But read carefully: "höchstens $d$ Nachbarn" (at most $d$ neighbors).
*   **The Logic:** If every person in a room can hold a maximum of 5 hands ($d=5$), the total number of hand-holds in the room can never exceed $5 \times \text{People}$. Mathematically, $|E| \le d \cdot |V|$. 
*   Because $d$ is just a constant number, we drop it in Big-O! Therefore, $|E| \in O(|V|)$. 
*   **The Solution:** Algorithmus A is $O(|V| + |V|) = \mathbf{O(|V|)}$. Algorithmus B is $\mathbf{O(|V| \log |V|)}$. Because linear time is strictly faster than linearithmic time, **Algorithmus A ist asymptotisch besser.**

#### 1(b) Master Theorem
**The Problem:** Solve $S(n) = 3S(\frac{n}{9}) + \sqrt{n}$.

*   **The Logic:** Let's use our Master Theorem cheat code.
    *   $a = 3$ (Clones)
    *   $b = 9$ (Shrink factor)
    *   Toll Fee $f(n) = \sqrt{n} = n^{1/2}$.
*   **The Battle:** How much work do the clones do? The formula is $n^{\log_b a} \rightarrow n^{\log_9 3}$. 
    *   *Math check:* What power do I raise 9 to, to get 3? The square root! So $\log_9 3 = 1/2$. 
    *   The clones do $n^{1/2}$ work. 
*   The Clones do $\sqrt{n}$. The Toll Fee is $\sqrt{n}$. **It is a perfect tie! (Case 2)**.
*   **The Solution:** Multiply the clones' work by $\log n$. The answer is **$\Theta(\sqrt{n} \log n)$**.

#### 1(c) Dijkstra vs. MST (True/False)
**The Problem:** True or False: The Shortest-Path Tree created by Dijkstra's algorithm (with positive weights) is always a Minimum Spanning Tree (MST).

*   **The Trap:** They both build trees, and they both look for "cheap" edges. But they have entirely different goals! Dijkstra cares about the distance from *one specific starting point*. MST cares about the *total cost of all wood used*. 
*   **The Solution:** **Falsch (Widerlegung durch Gegenbeispiel).**
    *   Imagine a triangle of 3 cities: A, B, and C. 
    *   Edges: (A-B = 5), (A-C = 5), (B-C = 1).
    *   *Dijkstra (starting at A):* Wants the fastest route to B and C. It takes the direct paths A-B (5) and A-C (5). Total tree weight = **10**.
    *   *MST (Kruskal):* Wants the cheapest total bridge cost. It builds B-C (1) and A-B (5). Now everyone is connected. Total tree weight = **6**.
    *   10 $\neq$ 6. Therefore, a Dijkstra tree is not automatically an MST.

#### 1(d) Scenario: Range Queries
**The Problem:** You have $n$ favorite numbers $r_i$. You are allowed to preprocess them. Then you get asked: "How many numbers are strictly smaller than $q$?" Do this as efficiently as possible.

*   **The Connection:** We need to search an array really fast. 
*   **The Solution:** 
    *   *Vorverarbeitung (Preprocessing):* Sort the array of numbers in ascending order using MergeSort or HeapSort in **$O(n \log n)$** time. 
    *   *Anfrage (Query):* Use **Binary Search** to find the number $q$ (or the spot where $q$ *would* be inserted). The index of that spot tells you exactly how many elements are to the left of it! This takes **$O(\log n)$** time per query.

#### 1(e) Scenario: ER Waiting Room
**The Problem:** Patients arrive at a doctor's office continuously. When a room is free, you must pick the patient with the highest injury severity. 

*   **The Connection:** "Highest severity", "Continuously arriving". This is literally Topic 1 in our Study Guide!
*   **The Solution:** Use a **Max-Heap** (Priority Queue). The severity is the key. 
    *   Inserting a new patient takes $O(\log n)$ time (`trickle-up`).
    *   Extracting the most critical patient takes $O(\log n)$ time (`extract-max` and `trickle-down`).

#### 1(f) Scenario: Catching a Fugitive
**The Problem:** A suspect is fleeing from your city (Source) to another city (Sink). You don't know the route. You know how many police officers are needed to block each road. Find the minimum number of police needed to guarantee the fugitive cannot reach the destination.

*   **The Connection:** Severing all connections between a Source and a Sink using the minimum "cost" is the exact definition of a **Minimum S-T Cut**.
*   **The Solution:** Model the cities and roads as a **Flow Network**, where the capacity of an edge is the number of police needed to block it. 
    *   According to the **Max-Flow Min-Cut Theorem**, the Minimum Cut is mathematically identical to the Maximum Flow.
    *   Run the **Ford-Fulkerson** algorithm to find the Max Flow. The value of the Max Flow equals the exact minimum number of police needed! Runtime is $O(|E| \cdot f_{max})$.

#### 1(g) Scenario: Power Grid
**The Problem:** Connect a new neighborhood of houses to the power grid. Houses can connect to each other. You want to minimize the total cable length. 

*   **The Connection:** Connect everything together. No loops. Minimum total cost.
*   **The Solution:** This is the exact definition of a **Minimum Spanning Tree (MST)**. Use **Kruskal's Algorithmus** or **Prim's Algorithmus**. Laufzeit: **$O(|E| \log |V|)$**.

*   ### Aufgabe 2: AVL-Bäume (10 Punkte)
**The Problem:** Insert the numbers `[7, 2, 8, 3, 4, 5, 1]` one by one into an empty AVL tree. Draw the tree after each step and state the rotations used.

*   **The Trap:** It is incredibly easy to lose track of the "Balance Factor" (Left Height - Right Height). Always check the balance of the *grandparent* after inserting. If it's a "dog-leg" (e.g., Left child's Right subtree is too heavy), you MUST do a double rotation. 
*   **The Solution (Step-by-Step Trace):**

    **1. Insert 7, then 2, then 8:**
    *   7 becomes Root. 2 goes Left. 8 goes Right. 
    *   *Tree:* `7 -> L:2, R:8`. 
    *   *Balance:* Perfect. No rotations.

    **2. Insert 3:**
    *   3 is `< 7` (goes L), `> 2` (goes R). 
    *   *Tree:* `7 -> L:2 (R:3), R:8`. 
    *   *Balance:* Node 7 has Left height 2, Right height 1. Diff is 1. All good.

    **3. Insert 4 (The First Imbalance):**
    *   4 is `< 7` (L), `> 2` (R), `> 3` (R). 
    *   *Tree:* `7 -> L:2 (R:3 (R:4)), R:8`.
    *   *The Imbalance:* Check node 2. Left is 0, Right is 2. Balance is -2!
    *   *The Fix:* This is a straight line to the right (Right-Right Case). We do a **Left Rotation at Node 2**.
    *   *Resulting Tree:* `7 -> L:3 (L:2, R:4), R:8`.

    **4. Insert 5 (The Double Rotation Trap):**
    *   5 is `< 7` (L), `> 3` (R), `> 4` (R).
    *   *Tree:* `7 -> L:3 (L:2, R:4 (R:5)), R:8`.
    *   *The Imbalance:* Check node 7! Left height is 3 (path 7-3-4-5). Right height is 1 (path 7-8). Balance is +2!
    *   *Which Case?* The imbalance is at 7. The heavy child is 3 (Left). But 3's heavy side is 4 (Right). This is a "dog-leg" shape: **Left-Right (LR) Case**.
    *   *The Fix:* **Double Rotation (Left-Right)**. 
        1. First, **Left Rotate at 3**. Node 4 moves up, 3 becomes its left child. (5 remains right child of 4).
        2. Second, **Right Rotate at 7**. Node 4 moves up to the Root. 7 drops down to become the right child of 4. Node 5 (which was hanging off 4's right) hops over to become the left child of 7.
    *   *Resulting Tree:* 
        ```text
              4
            /   \
           3     7
          /     / \
         2     5   8
        ```

    **5. Insert 1 (The Final Imbalance):**
    *   1 is `< 4` (L), `< 3` (L), `< 2` (L).
    *   *Tree:* `4 -> L:3 (L:2 (L:1)), R:7 (L:5, R:8)`.
    *   *The Imbalance:* Check node 3. Left height is 2, Right is 0. Balance is +2!
    *   *The Fix:* Straight line to the left (Left-Left Case). We do a **Right Rotation at Node 3**.
    *   *Resulting Tree (Final Answer):*
        ```text
              4
            /   \
           2     7
          / \   / \
         1   3 5   8
        ```

---

### Aufgabe 3: Stabile Paarungen (6 Punkte)
**The Problem:** There are $n$ people and $n$ jobs. $k$ are "cool", the rest are "uncool". EVERYONE strictly prefers cool over uncool. Prove that the Gale-Shapley algorithm will *never* match a cool person with an uncool job.

*   **The Trap:** Don't try to trace the algorithm step-by-step. Use a **Proof by Contradiction** based on the definition of "Stability" (Blocking Pairs).
*   **The Logic / Solution Proof:**
    1. **Assumption:** Assume for contradiction that the Gale-Shapley algorithm *does* produce a matching where a Cool Person ($P_{cool}$) is assigned to an Uncool Job ($J_{uncool}$).
    2. **The Pigeonhole Principle:** There are exactly $k$ cool people and exactly $k$ cool jobs. If one cool person is stuck with an uncool job, it mathematically means at least one cool job ($J_{cool}$) must have been assigned to an uncool person ($P_{uncool}$).
    3. **The Blocking Pair (Instabilität):** 
        * Let's look at $P_{cool}$ and $J_{cool}$. They are currently not matched to each other.
        * $P_{cool}$'s current partner is an uncool job. Because everyone prefers cool over uncool, $P_{cool}$ strictly prefers $J_{cool}$ over their current match.
        * $J_{cool}$'s current partner is an uncool person. Because everyone prefers cool over uncool, $J_{cool}$ strictly prefers $P_{cool}$ over their current match.
    4. **Conclusion:** Because they both prefer each other over their assigned partners, $(P_{cool}, J_{cool})$ forms a **Blocking Pair (blockierendes Paar)**.
    5. **The Fatal Blow:** The Gale-Shapley algorithm is mathematically proven to *only* return Stable Matchings (matchings with zero blocking pairs). Therefore, our assumption creates an impossible paradox. No cool person can ever be matched with an uncool job. **Q.E.D.**

---

### Aufgabe 4: Blumenanbau (5 Punkte)
**The Problem:** $n^2$ total flower types. One random flower is picked per day. Prof. Hirnriss plants $2i-1$ total flowers in his garden by day $i$. 
Part A: Prove mathematically that $\sum_{i=1}^n (2i-1) = n^2$.
Part B: Prove that the expected number of times the mayor visits him over $n$ days is exactly 1.

#### 4(a) The Induction Proof
*   **The Connection:** Standard mathematical induction. 
*   **The Solution:**
    *   **Base Case ($n=1$):** $\sum_{i=1}^1 (2i-1) = 2(1) - 1 = 1$. And $1^2 = 1$. The base case holds.
    *   **Induction Hypothesis (IV):** Assume it holds for a specific $n$: $\sum_{i=1}^n (2i-1) = n^2$.
    *   **Inductive Step ($n \to n+1$):** We must prove it equals $(n+1)^2$.
        *   $\sum_{i=1}^{n+1} (2i-1) = \Big(\sum_{i=1}^n (2i-1)\Big) + \Big(2(n+1) - 1\Big)$
        *   Substitute our IV assumption: $= n^2 + (2n + 2 - 1)$
        *   Simplify: $= n^2 + 2n + 1$
        *   Factor: $= (n+1)^2$. 
    *   **Conclusion:** The statement holds for all $n \in \mathbb{N}$.

#### 4(b) The Expected Value (Linearity of Expectation)
*   **The Trap:** Dealing with expected values over multiple days can feel confusing because the probabilities change every day! The cheat code here is **Linearity of Expectation** (Linearität des Erwartungswertes), which states you can just calculate the expected value of *each individual day* and add them all together, even if they seem connected!
*   **The Logic / Solution:**
    1. Let $X_i$ be an indicator variable: $X_i = 1$ if the mayor visits on day $i$, and $0$ if not.
    2. Since there are $n^2$ total flowers, and the mayor picks 1 at random, the probability of any specific flower being picked is $\frac{1}{n^2}$.
    3. On day $i$, the Professor has exactly $(2i-1)$ flowers in his garden. So the probability of a visit on day $i$ is exactly: $P(X_i = 1) = \frac{2i-1}{n^2}$.
    4. By the definition of Indicator Variables, $E[X_i] = \frac{2i-1}{n^2}$.
    5. The total expected visits over $n$ days is $E[X] = \sum_{i=1}^n E[X_i]$.
    6. Substitute: $E[X] = \sum_{i=1}^n \frac{2i-1}{n^2}$.
    7. Pull out the constant fraction: $E[X] = \frac{1}{n^2} \cdot \sum_{i=1}^n (2i-1)$.
    8. Substitute the formula we proved in Part A: $E[X] = \frac{1}{n^2} \cdot n^2 = \mathbf{1}$.
    9. **Conclusion:** In expectation, the mayor visits exactly 1 time.
 
    ### Aufgabe 5: Mehrheitselement (13 Punkte)
**The Problem:** We are given an array where one element appears strictly more than $n/2$ times. We need to find it using different algorithmic paradigms.

#### 5(a) Divide-and-Conquer in $O(n \log n)$
*   **The Connection:** This is exactly HA 4, Aufgabe 3 from our Study Guide!
*   **The Solution:**
    1.  **Divide:** Split the array perfectly in half. 
    2.  **Conquer:** Recursively call the algorithm on the Left half and the Right half. 
    3.  **Base Case:** If an array has size 1, return that element.
    4.  **Merge (The Logic):** The recursion returns a `left_winner` and a `right_winner`. 
        *   *The Magic Rule:* If a global majority exists, it MUST be the local majority of at least one of the halves!
        *   If `left_winner == right_winner`, we found it!
        *   If they differ, do a full $O(n)$ scan of the *entire current array* to count how many times `left_winner` and `right_winner` actually appear. Return the one that has $> n/2$ counts.
    5.  **Runtime Proof:** $T(n) = 2T(n/2) + O(n)$. By the Master Theorem ($a=2, b=2, f(n)=n$), this is Case 2 (perfect tie). Runtime is **$O(n \log n)$**.

#### 5(b) Trace the Algorithm (Boyer-Moore Majority Vote)
*   **The Problem:** Trace the given pseudocode on `[1, 1, 2, 2, 2, 3, 2]`. 
*   **The Trace:**
    *   `i=1 (A=1)`: counter=0 $\to$ element=1, counter=1.
    *   `i=2 (A=1)`: matches element $\to$ counter=2.
    *   `i=3 (A=2)`: doesn't match $\to$ counter=1.
    *   `i=4 (A=2)`: doesn't match $\to$ counter=0.
    *   `i=5 (A=2)`: counter=0 $\to$ element=2, counter=1.
    *   `i=6 (A=3)`: doesn't match $\to$ counter=0.
    *   `i=7 (A=2)`: counter=0 $\to$ element=2, counter=1.
    *   **Return:** `2`. 

#### 5(c) Proof of Correctness and Linear Time
*   **The Trap:** Proving this requires thinking about the algorithm as a "battlefield". 
*   **The Solution:**
    *   **Runtime:** The algorithm contains a single `for`-loop from $1$ to $n$. Inside the loop, it only does basic $O(1)$ `if/else` checks and math. Therefore, it strictly takes **$O(n)$** time. 
    *   **Correctness (The Battlefield):** Every time the `counter` drops to 0, it means the current `element` was paired up 1-to-1 with completely different elements, and they destroyed each other. If an element is the true Majority, it appears strictly $> n/2$ times. Even if *every single other element* in the array teams up to destroy it, they don't have enough troops! The majority element will always be the last one standing when the array ends.

#### 5(d) Counterexample (When it fails)
*   **The Problem:** Create a list that has NO strict majority ($>n/2$), where the algorithm fails to return the *most frequent* element. 
*   **The Solution:** `[1, 1, 2, 2, 2, 3, 4]`. 
    *   *Most frequent:* 2 (appears 3 times). 
    *   *The Trace:* 
        * 1, 1 $\to$ element 1 (count 2)
        * 2, 2 $\to$ destroys the 1s. Count drops to 0.
        * 2 $\to$ element 2 (count 1)
        * 3 $\to$ destroys the 2. Count drops to 0. 
        * 4 $\to$ element 4 (count 1). 
    *   *Result:* It returns `4`. It completely missed the 2! 

#### 5(e) Another Linear Time Algorithm
*   **The Trap:** How else can you find a majority in $O(n)$? The hint says: "Imagine it was sorted." 
*   **The Connection:** QuickSelect! (Topic 12 in the Study Guide).
*   **The Solution:** If the array *was* perfectly sorted, the majority element (since it takes up $>50\%$ of the space) is mathematically guaranteed to cross the exact middle index. Therefore, the **Median** of the array is ALWAYS the majority element. We can use the **QuickSelect Algorithm** to find the exact Median element in expected **$O(n)$** time without actually sorting the array!

---

### Aufgabe 6: Rift of the Necrodancer (10 Punkte)
**The Problem:** We want to activate a "Vibe-Power" that doubles points for $t$ seconds, but goes on cooldown for another $t$ seconds (total cycle is $2t$). We want to maximize total bonus points.

#### 6(a) Why Greedy works for 1 Activation
*   **The Problem:** Prove Greedy is optimal if we are only allowed to activate it *once* ($k=1$).
*   **The Solution:** If we can only activate it once, we only need to find the single contiguous subarray of length $t$ that has the highest mathematical sum. The Greedy strategy evaluates all valid placements and simply picks the one with the maximum sum. Since there are no future cooldown consequences to worry about, picking the absolute maximum is by definition optimal. 

#### 6(b) The Greedy Counterexample ($t=1$)
*   **The Trap:** If $t=1$, an activation takes 1 second, and the cooldown takes 1 second. So activations must be spaced exactly 2 indices apart ($i$, $i+2$, $i+4$). 
*   **The Solution:** Array = `[15, 20, 15]`.
    *   *Greedy:* Sees 20 is the highest number. Activates at index 2. This triggers a cooldown at index 3, and index 1 is too close. Greedy finishes with total points = **20**. 
    *   *Optimal:* Activates at index 1 (15 points), cools down at index 2, activates at index 3 (15 points). Total points = **30**. Greedy fails!

#### 6(c) Why does OPT start at $t+1$ and end at $n+2t$?
*   **The Connection:** Index Out of Bounds errors in DP tables!
*   **The Solution:** The DP recurrence needs to look backward in time by $2t$ steps to check if the cooldown has finished. If we start at $k=1$, looking back $2t$ steps gives a negative index, which crashes the math. By shifting our timeline forward so the real game starts at $t+1$ (and padding the start with dummy zeros), our algorithm can safely look backward without leaving the boundaries of the array. We extend it to $n+2t$ to give the final cooldown room to resolve.

#### 6(d) The DP Recurrence Equation
*   **The Trap:** Translate the rules into DP choices. At any given moment $k$ where the Vibe-Power is *ready*, you have two universes.
*   **The Solution / Logic:**
    1.  **Universe 1 (Wait):** We do absolutely nothing at time $k$. Therefore, our total points are exactly the same as they were at $k-1$. $\to OPT(k-1)$.
    2.  **Universe 2 (We just finished cooling down):** If the power is perfectly ready at exactly time $k$, and we *did* use it recently, it means we MUST have activated it exactly at $k-2t$. 
        *   If we activated it at $k-2t$, we gained the bonus points for $t$ seconds: $\sum_{j=k-2t}^{k-t-1} a_j$.
        *   And before we did that, the power must have been ready at $k-2t$. So we add the historical points: $OPT(k-2t)$.
    3.  **The Master Formula:** The optimal choice is always the maximum of those two universes:
        $$OPT(k) = \max \left( OPT(k-1), \quad OPT(k-2t) + \sum_{j=k-2t}^{k-t-1} a_j \right)$$

---

## 📝 Klausur 2022/2023 (Prof. Schömer / Fischer / etc.)

### Aufgabe 1: Kurze Fragen (Divide and Conquer)

#### 1(a) Master Theorem
**The Problem:** Solve three recurrences. State $a, b, \alpha$ and the bounds.
*   **The Trap:** Read the third one carefully. It asks *why* the bound is optimal. 
*   **The Solutions:**
    1.  **$T(n) = 4T(n/2) + cn^2$**
        *   $a=4$, $b=2$, $f(n) = cn^2$. Clones do $n^{\log_2 4} = n^2$ work.
        *   *Battle:* $n^2$ vs $n^2$. Tie! (Case 2). 
        *   *Result:* **$\Theta(n^2 \log n)$**.
    2.  **$T(n) = 3T(n/2) + cn$**
        *   $a=3$, $b=2$, $f(n) = cn$. Clones do $n^{\log_2 3} \approx n^{1.58}$ work.
        *   *Battle:* $n^{1.58}$ vs $n^1$. Clones win! (Case 1).
        *   *Result:* **$\Theta(n^{\log_2 3})$**.
    3.  **$T(n) = 2T(n/2) + c \log_2(n)$**
        *   $a=2$, $b=2$, $f(n) = c \log n$. Clones do $n^{\log_2 2} = n^1$ work.
        *   *Battle:* $n^1$ vs $\log n$. Clones completely crush the log! (Case 1).
        *   *Result:* **$\Theta(n)$**.
        *   *Why is this bound best possible?* Any algorithm that processes an input of size $n$ MUST look at the data at least once or output $n$ elements. You physically cannot process an array of size $n$ in less than $\Theta(n)$ time. It is the absolute lower bound for reading the input!

#### 1(b) Identify D&C Algorithms
**The Problem:** Which of these use Divide & Conquer? State subproblem counts and sizes.
*   **The Connection:** Just recall the recursive structures from the Study Guide!
*   **The Solution:**
    *   **Kruskal:** No (Greedy).
    *   **FFT:** **Yes.** Splits into Even/Odd. $\to$ **2 Teilprobleme, Größe $n/2$**.
    *   **Dijkstra:** No (Greedy/DP).
    *   **Ford-Fulkerson:** No (Flow/Augmenting Paths).
    *   **Karatsuba:** **Yes.** Uses algebraic tricks to avoid 4 multiplications. $\to$ **3 Teilprobleme, Größe $n/2$**.
    *   **Median-Of-Five (QuickSelect):** **Yes.** Splits the array to find medians of chunks, then recurses. $\to$ **1 Teilproblem Größe $n/5$** (to find the pivot), and **1 Teilproblem maximale Größe $7n/10$** (the actual partition search).
    *   **Counting Sort:** No (Linear Array Mapping).

#### 1(c) Karatsuba Trace ($12 \times 31$)
**The Problem:** Multiply 12 by 31 using Karatsuba.
*   **The Trap:** Do not just write 372. You must trace the 3 specific multiplications ($z_0, z_1, z_2$) that Karatsuba uses to save time!
*   **The Solution:**
    *   Split the numbers: $x = 12 \to x_1 = 1, x_0 = 2$. And $y = 31 \to y_1 = 3, y_0 = 1$.
    *   *Mult 1:* $z_2 = x_1 \cdot y_1 = 1 \times 3 = \mathbf{3}$.
    *   *Mult 2:* $z_0 = x_0 \cdot y_0 = 2 \times 1 = \mathbf{2}$.
    *   *Mult 3 (The Combo):* $z_1 = (x_1 + x_0) \cdot (y_1 + y_0) - z_2 - z_0$
        $z_1 = (1 + 2) \cdot (3 + 1) - 3 - 2 = (3 \times 4) - 5 = 12 - 5 = \mathbf{7}$.
    *   *Assembly:* $z_2 \cdot 10^2 + z_1 \cdot 10^1 + z_0 = 300 + 70 + 2 = \mathbf{372}$.

---

### Aufgabe 2: Coole Personen (Stabile Paarungen)
**The Problem:** $n$ men, $n$ women. $k$ are "cool". Everyone strictly prefers cool people over uncool people. Prove Gale-Shapley never pairs a cool man with an uncool woman.

*   **The Connection:** This is the EXACT SAME logic as Aufgabe 3 in the Probeklausur! Instead of Cool People and Cool Jobs, it is Cool Men and Cool Women. 
*   **The Solution:**
    1.  **Assume the opposite:** Suppose a Cool Man ($M_{cool}$) is paired with an Uncool Woman ($W_{uncool}$).
    2.  Because there are exactly $k$ cool men and $k$ cool women, if $M_{cool}$ is taken by someone uncool, there MUST be at least one Cool Woman ($W_{cool}$) who is forced to pair with an Uncool Man ($M_{uncool}$).
    3.  **Check for Blocking Pairs:** 
        *   $M_{cool}$ is paired with $W_{uncool}$. But he strictly prefers $W_{cool}$.
        *   $W_{cool}$ is paired with $M_{uncool}$. But she strictly prefers $M_{cool}$.
    4.  Because they both prefer each other over their current partners, $(M_{cool}, W_{cool})$ forms a **Blockierendes Paar (Blocking Pair)**.
    5.  Gale-Shapley guarantees a matching with ZERO blocking pairs. Contradiction! Thus, the assumption is false. 

---

### Aufgabe 3: Mengensumme (Algorithmenentwurf)
**The Problem:** You have two sets of numbers $A$ and $B$. You want to find all possible sums $a+b$. Do this in $O(N^2)$ and then $O(N \log N)$. 

#### 3(a) The $O(N^2)$ Approach
*   **The Solution:** Just use two nested `for`-loops. Loop through every element $a \in A$, and inside that, loop through every element $b \in B$. Add them together and store the result in a Hash Set (to remove duplicates). Since $A$ and $B$ have at most $N$ elements, $N \times N = O(N^2)$.

#### 3(b) The $O(N \log N)$ FFT Magic Trick
*   **The Trap:** How do you add numbers using an algorithm designed to multiply? Look at the hint: $x^a \cdot x^b = x^{a+b}$. 
*   **The Solution:** We translate the sets into **Polynomials**!
    1.  Create a polynomial for Set A: For every number $a \in A$, set the coefficient of $x^a$ to 1. (e.g., if $A = \{0, 1, 4\}$, then $P_A(x) = 1x^0 + 1x^1 + 0x^2 + 0x^3 + 1x^4$).
    2.  Create a polynomial for Set B the exact same way: $P_B(x)$.
    3.  Multiply the two polynomials: $P_{Result}(x) = P_A(x) \cdot P_B(x)$. 
    4.  Whenever $x^a$ multiplies with $x^b$, it creates $x^{a+b}$! If the final polynomial $P_{Result}$ has a coefficient $>0$ for $x^k$, it means $k$ is a valid sum in $A+B$!
    5.  **The Speedup:** Multiplying polynomials normally takes $O(N^2)$. But if we use the **Fast Fourier Transform (FFT)**, we can multiply them in exactly **$O(N \log N)$** time!

---

### Aufgabe 4: Nervensäge (Rod Cutting DP)
**The Problem:** You have a metal rod of length $n$. You can cut it into integer pieces. Piece length $i$ sells for price $p_i$. Maximize profit.

#### 4(a) The Example
*   **The Trap:** Don't just assume the biggest piece or the biggest cut is best. Test combinations!
*   **Prices:** length 1 = $5, length 2 = $8, length 3 = $7, length 4 = $9, length 5 = $10.
*   **The Solution:** The optimal strategy is to cut the rod into five pieces of length 1! Profit = $5 \times p_1 = 5 \times 5 = \mathbf{25}$. (Selling it whole only gives 10!).

#### 4(b) Top-Down Recursion
*   **The Solution:**
    ```python
    def Cut(n):
        if n == 0: return 0
        max_profit = 0
        # Try cutting off a piece of length i, and recursively solve the rest
        for i from 1 to n:
            profit = p[i] + Cut(n - i)
            if profit > max_profit:
                max_profit = profit
        return max_profit
    ```
    *Correctness:* It exhaustively tries every possible valid "first cut" (length 1 through $n$), asks the recursion for the absolute best way to cut the remaining rod, and returns the maximum combination. 

#### 4(c) Bottom-Up Polynomial Time (DP)
*   **The Trap:** The recursion in (b) is exponential $O(2^n)$ because it recalculates the same sub-rods. We need to use a DP array (Memoization).
*   **The Solution:**
    ```python
    def DP_Cut(n):
        OPT = new Array of size n+1 (filled with 0s)
        OPT[0] = 0
        
        for j from 1 to n:          # Solve for rod of length j
            max_profit = 0
            for i from 1 to j:      # Try all cuts on rod j
                max_profit = max(max_profit, p[i] + OPT[j - i])
            OPT[j] = max_profit     # Write sticky note
            
        return OPT[n]
    ```
    *Runtime:* We have an outer loop running $n$ times. Inside, an inner loop runs up to $n$ times. $n \times n = \mathbf{O(n^2)}$. 

---

### Aufgabe 5: Dynamische Arrays (Amortisierte Analyse)
**The Problem:** Prove the amortized time of Dynamic Arrays using a Potential Function $\Phi$. 
*   Insert: Array doubles when full ($m_i = 2m_{i-1}$).
*   Delete: Array shrinks to 2/3 size when exactly 1/3 full.
*   Formula to prove: $\hat{c}_i = c_i + \Phi_i - \Phi_{i-1} \in O(1)$.

#### 5(a) Insert (When Full)
*   **The Setup:** Before insert, the array is perfectly full: $n_{i-1} = m_{i-1}$. 
    *   Because it's full ($n \ge m/2$), $\Phi_{i-1} = 2n_{i-1} - m_{i-1} = 2m_{i-1} - m_{i-1} = m_{i-1}$.
*   **The Action:** We double the array: $m_i = 2m_{i-1}$. We add one element: $n_i = n_{i-1} + 1 = m_{i-1} + 1$. 
*   **The Cost:** We must copy $m_{i-1}$ elements and insert 1. Actual cost $c_i = m_{i-1} + 1$.
*   **The New Potential:** $n_i$ is $\approx m_i/2$. So $\Phi_i = 2n_i - m_i = 2(m_{i-1} + 1) - 2m_{i-1} = 2$.
*   **The Math:** $\hat{c}_i = c_i + \Phi_i - \Phi_{i-1}$
    $\hat{c}_i = (m_{i-1} + 1) + (2) - (m_{i-1})$
    $\hat{c}_i = 3$. 
    Since 3 is a constant, the amortized cost is strictly **$O(1)$**. 

#### 5(b) Delete (When Shrinking)
*   **The Setup:** Before delete, array is 1/3 full + 1: $n_{i-1} = m_{i-1}/3 + 1$. 
    *   Because it's less than half full, we use the other formula: $\Phi_{i-1} = m_{i-1} - 2n_{i-1} = m_{i-1} - 2(m_{i-1}/3 + 1) = \frac{1}{3}m_{i-1} - 2$.
*   **The Action:** We delete 1 element: $n_i = m_{i-1}/3$. The array triggers a shrink to 2/3 its old size: $m_i = \frac{2}{3}m_{i-1}$. 
*   **The Cost:** We must copy the remaining elements. Actual cost $c_i = n_i = m_{i-1}/3$.
*   **The New Potential:** Let's check $n_i$ vs $m_i$. $n_i$ is exactly half of $m_i$. The formula is $\Phi_i = 2n_i - m_i = 2(m_i/2) - m_i = 0$. (The potential drops to 0!).
*   **The Math:** $\hat{c}_i = c_i + \Phi_i - \Phi_{i-1}$
    $\hat{c}_i = (m_{i-1}/3) + (0) - (\frac{1}{3}m_{i-1} - 2)$
    $\hat{c}_i = 2$.
    Since 2 is a constant, the amortized cost is strictly **$O(1)$**. 

---

### Aufgabe 6 & 7: Heap & Prim (Tracing on Paper)
*(Note: These are purely visual tracing problems. Here is exactly what the grader looks for!)*

*   **For the Heap (ExtractMin):** Always take the very last element in the array, move it to the Root, and delete the last slot. Then, carefully `trickle-down` by swapping it with the **smallest** of its two new children until it stops. 
*   **For the Heap (DecreaseKey):** Change the number, then `trickle-up` by swapping it with its parent if it is smaller than the parent. 
*   **For Prim's Algorithm:** 
    1. Start at Node A. The "Cut" is all edges touching A. 
    2. Pick the cheapest edge touching A. Add that new node to your empire.
    3. Update your list of available edges. You now look at all edges touching A *and* the new node. 
    4. Pick the cheapest one that leads to an unvisited node. 
    5. *Exam Hack:* Draw the graph and literally highlight the edges as you pick them. Write down a list of nodes in the exact order you added them to the tree.
 
    6. ---

## 📝 Probeklausur 2022/2023

### Aufgabe 1: Divide-and-Conquer (Kurze Fragen)

#### 1(a) Master Theorem Bounds
*   **1. $T(n) = 2T(n/2) + cn$**
    *   $a=2$, $b=2$, $f(n)=cn$. Clones do $n^{\log_2 2} = n^1$ work.
    *   *Battle:* $n^1$ vs $n^1$. Tie! (Case 2).
    *   *Result:* **$\Theta(n \log n)$**.
*   **2. $T(n) = 7T(n/2) + cn^3$**
    *   $a=7$, $b=2$, $f(n)=cn^3$. Clones do $n^{\log_2 7} \approx n^{2.81}$ work.
    *   *Battle:* $n^{2.81}$ vs $n^3$. Toll fee wins! (Case 3).
    *   *Result:* **$\Theta(n^3)$**.
*   **3. $T(n) = 2T(n/2) + c\sqrt[3]{n}$**
    *   $a=2$, $b=2$, $f(n)=c \cdot n^{1/3}$. Clones do $n^1$ work.
    *   *Battle:* $n^1$ vs $n^{0.33}$. Clones completely crush the toll fee! (Case 1).
    *   *Result:* **$\Theta(n)$**.

#### 1(b) Identifying D&C and Subproblems
*   **Kruskal:** No. (Greedy).
*   **Karatsuba:** **Yes.** 3 Teilprobleme, Größe $n/2$.
*   **Floyd-Warshall:** No. (Dynamic Programming).
*   **FFT:** **Yes.** 2 Teilprobleme, Größe $n/2$ (Even and Odd parts).
*   **Ford-Fulkerson:** No. (Flow / Augmenting Paths).
*   **Counting Sort:** No. (Linear Mapping).
*   **Rekursiver Top-Down Heap Build:** **Yes.** 2 Teilprobleme, Größe $n/2$ (Recursively building the left and right subtrees before running `trickle-down` on the root).

#### 1(c) Identifying Greedy Algorithms
*   **Kruskal:** **Yes.** (Always picks the cheapest edge).
*   **Matrix-Kettenmultiplikation:** No. (DP - tests all splits).
*   **Dijkstra:** **Yes.** (Always seals the closest envelope).
*   **Quicksort:** No. (Divide & Conquer).
*   **Huffman:** **Yes.** (Always merges the two smallest frequencies).

#### 1(d) Calculate the DFT Manually
**The Problem:** Find the DFT of polynomial $A(x) = 2 + ix - 4x^2 + x^3$.
*   **The Connection:** For 4 items ($n=4$), the "Roots of Unity" are always exactly: **$1, i, -1, -i$**. We just plug these 4 numbers into $x$! *(Remember: $i^2 = -1$, and $i^3 = -i$)*.
*   **The Math:**
    *   $A(1) = 2 + i(1) - 4(1)^2 + (1)^3 \to 2 + i - 4 + 1 = \mathbf{-1 + i}$
    *   $A(i) = 2 + i(i) - 4(i)^2 + (i)^3 \to 2 + i^2 - 4(-1) - i \to 2 - 1 + 4 - i = \mathbf{5 - i}$
    *   $A(-1) = 2 + i(-1) - 4(-1)^2 + (-1)^3 \to 2 - i - 4 - 1 = \mathbf{-3 - i}$
    *   $A(-i) = 2 + i(-i) - 4(-i)^2 + (-i)^3 \to 2 - i^2 - 4(1) + i \to 2 - (-1) - 4 + i \to 2 + 1 - 4 + i = \mathbf{-1 + i}$ *(Wait, let's double check $A(-i)$)*
        *   $i \cdot (-i) = -i^2 = -(-1) = 1$.
        *   $(-i)^2 = i^2 = -1$.
        *   $(-i)^3 = -i^3 = -(-i) = i$.
        *   $2 + (1) - 4(-1) + (i) \to 2 + 1 + 4 + i = \mathbf{7 + i}$.
*   **The Result Vector:** `(-1+i, 5-i, -3-i, 7+i)`.

#### 1(e) Pointwise DFT Multiplication
**The Problem:** What does multiplying $A(x) \cdot B(x)$ have to do with $DFT(A) \cdot DFT(B)$?
*   **The Solution:** The Fast Fourier Transform translates polynomials from "coefficient mode" into "frequency/point mode". In point mode, we can multiply the two polynomials together *pointwise* (straight across) in $O(n)$ time. If we then run the Inverse-DFT on the resulting points, it transforms back into coefficients, giving us the exact coefficients of $A(x) \cdot B(x)$ in $O(n \log n)$ total time!

---

### Aufgabe 2: Die Anti-Traumfrau (Stabile Paarungen)
**The Problem:** Prove that after Gale-Shapley, *at most one man* ends up paired with his absolutely least preferred woman (the bottom of his list). 

*   **The Trap:** Do not try to trace the algorithm. Think about what it actually means for a man to reach the bottom of his list. 
*   **The "Chewed" Logic / Proof:**
    1.  **The Trigger:** What happens the *exact moment* the very first man (let's call him Mark) gets down to the very last woman on his list?
    2.  **The History:** If Mark is proposing to his last choice, it means he already proposed to the other $n-1$ women, and *every single one of them rejected him*.
    3.  **The Rule of Rejection:** In Gale-Shapley, a woman ONLY rejects a man if she is already engaged to someone she likes better.
    4.  **The State of the World:** This means those $n-1$ women are currently engaged to $n-1$ *other* men. 
    5.  **The Conclusion:** If $n-1$ men are engaged to $n-1$ women, there is exactly 1 man left (Mark), and exactly 1 woman left (his least preferred). When Mark proposes to her, she is single and accepts. 
    6.  **The Fatal Blow:** Now everyone is paired! The Gale-Shapley algorithm terminates *immediately*. Because the algorithm ends the exact millisecond the first man hits the bottom of his list, it is physically impossible for a *second* man to ever reach the bottom of his list. **Q.E.D.**

---

### Aufgabe 3: Ein subquadratischer Sortieralgorithmus (2/3 Sort)
**The Problem:** We have a recursive algorithm that sorts the first 2/3, then sorts the last 2/3, then sorts the first 2/3 again. We want to replace *one* of those three steps with an $O(n)$ Linear-time algorithm, keep the overall algorithm working, and improve the runtime.

*   **The Trap:** You have to understand *why* the original algorithm works. Step 1 puts the biggest elements of the first chunk in the middle. Step 2 grabs those middle elements and pushes the absolute global maximums to the final 1/3 of the array. The final 1/3 is now permanently sorted and finished! Step 3 just cleans up the rest. 
*   **3(a) The Linear-Time Replacement:**
    *   *The Insight:* After Step 1 (first 2/3 sorted) and Step 2 (last 2/3 sorted), look at the first 2/3 of the array. It is made of two blocks: The first 1/3, and the middle 1/3. 
    *   Because of the previous sorting steps, the first 1/3 is internally sorted. The middle 1/3 is also internally sorted!
    *   *The Fix:* We don't need to recursively "Sort" the first 2/3 again in Step 3! We just need to take those two already-sorted blocks and run a standard **Merge** operation on them. 
    *   Merging two sorted lists takes exactly **$O(n)$** linear time. We replace Step 3 with `Merge(first_third, middle_third)`.
*   **3(b) The New Runtime:**
    *   We now only have TWO recursive calls instead of three.
    *   $T(n) = 2T(2n/3) + O(n)$.
    *   *Master Theorem:* $a=2, b=1.5$. Clones do $n^{\log_{1.5} 2} \approx n^{1.7}$ work. Toll fee is $n^1$. 
    *   *Result:* Clones win (Case 1). The new runtime is **$O(n^{\log_{1.5} 2}) \approx O(n^{1.71})$**. (We successfully dropped the runtime!).
*   **3(c) Pushing it to $O(n \log n)$:**
    *   *The Problem:* We can do even better! How do we avoid overlapping 2/3 chunks entirely?
    *   *The Solution:* Before doing any sorting, run the $O(n)$ **QuickSelect** algorithm to find the element that belongs exactly at the $2/3$ mark. **Partition** the array so the smallest $2/3$ are on the left, and the biggest $1/3$ are on the right. 
    *   Now, just recursively sort the left chunk, and recursively sort the right chunk!
    *   $T(n) = T(2n/3) + T(n/3) + O(n)$. This is a standard Divide & Conquer tree that sums to **$O(n \log n)$**. 

---

### Aufgabe 4: Dynamische Arrays (Amortisierte Analyse)
**The Problem:** Array expands by 50% ($m_i = 1.5 m_{i-1}$) when completely full. Array shrinks by 25% ($m_i = 0.75 m_{i-1}$) when less than half full. Prove Amortized time $O(1)$. 
*   Potential: $\Phi_i = 3n_i - 2m_i$ (if $n \ge \frac{2}{3}m$) OR $2m_i - 3n_i$ (if $n < \frac{2}{3}m$).

*   **The Trap:** Don't panic at the fractions. Just pick one case (e.g., the Expansion case), plug the math in, and watch it cancel. 
*   **The Solution (Insert / Expand Case):**
    1.  *Before Insert:* Array is exactly full. $n_{i-1} = m_{i-1}$. 
        *   Potential uses top formula: $\Phi_{i-1} = 3(m_{i-1}) - 2(m_{i-1}) = m_{i-1}$.
    2.  *The Action:* We expand. $m_i = \frac{3}{2}m_{i-1}$. We insert 1 item: $n_i = m_{i-1} + 1$. 
    3.  *The Cost:* We copy $m_{i-1}$ items, plus 1 insert. $c_i = m_{i-1} + 1$.
    4.  *New Potential:* Is $n_i \ge \frac{2}{3}m_i$? Let's check: $\frac{2}{3}(\frac{3}{2}m_{i-1}) = m_{i-1}$. Yes, $m_{i-1}+1$ is $\ge m_{i-1}$. Use top formula!
        *   $\Phi_i = 3(m_{i-1} + 1) - 2(\frac{3}{2}m_{i-1}) = 3m_{i-1} + 3 - 3m_{i-1} = 3$. 
    5.  *The Math:* $\hat{c}_i = c_i + \Phi_i - \Phi_{i-1}$
        *   $\hat{c}_i = (m_{i-1} + 1) + (3) - (m_{i-1})$
        *   $\hat{c}_i = 4$. 
    6.  *Conclusion:* Because 4 is a constant, expanding the array has an amortized cost of **$O(1)$**. 

---

### Aufgabe 5: Maximaler Fluss (Tracing on Paper)
*(Note: As this is a visual Ford-Fulkerson trace, here is the exam strategy.)*

*   **The Method:**
    1.  Find ANY path from Source ($s$) to Sink ($t$) where all forward arrows have capacity remaining ($c > f$).
    2.  Find the **Bottleneck** (the smallest remaining space on that path). Let's call it $\Delta$.
    3.  Add $\Delta$ to the flow ($f$) of every edge on that path.
    4.  Draw the **Residual Network (The Undo Graph)**: 
        *   Draw forward arrows with weight = `capacity - new flow`.
        *   Draw backward arrows with weight = `new flow`.
    5.  Repeat until absolutely no forward paths from $s$ to $t$ exist in the Residual graph. The total flow you pushed is the Maximum Flow.

### different prof exams

---

## 📝 Klausur 2024/2025

### Aufgabe 1: Kurze Aufgaben (40 Punkte - Rapid Fire!)

This section was a massive 40 points of rapid-fire logic traps.

#### 1(a) $\log_2 n \in O(\sqrt{n})$ ?
*   **The Solution:** **Wahr (True).** Polynomials/Roots *always* grow faster than Logarithms eventually. If you use the Limit Method (L'Hôpital's rule) on $\frac{\log n}{n^{0.5}}$, the derivative drops the log into the denominator, sending the limit to 0. The bottom (root) wins.

#### 1(b) $2^{2n} \in O(2^n)$ ?
*   **The Trap:** It looks like they have the same base. But do the algebra!
*   **The Solution:** **Falsch (False).** $2^{2n} = (2^2)^n = 4^n$. 
    $4^n$ grows astronomically faster than $2^n$. It is strictly $\omega(2^n)$. 

#### 1(c) $T_1(n) = 4T_1(n/2) + kn$
*   **The Solution:** $a=4, b=2, f(n)=kn$. Clones do $n^{\log_2 4} = n^2$. The toll fee is $n^1$. The clones win (Case 1). Constant $k$ is ignored. **$\Theta(n^2)$**.

#### 1(d) $T_2(n) = k \cdot T_2(n/2) + n^{\log_2 k}$
*   **The Trap:** They put variables in the Master Theorem! Don't panic, just follow the formula exactly.
*   **The Solution:** $a=k, b=2, f(n)=n^{\log_2 k}$. Clones do $n^{\log_b a} = n^{\log_2 k}$.
    *   *Battle:* Clones do $n^{\log_2 k}$. Toll fee is exactly $n^{\log_2 k}$. It's a perfect tie! (Case 2).
    *   *Result:* Multiply by $\log n$. **$\Theta(n^{\log_2 k} \cdot \log n)$**.

#### 1(e) The Fake Mergesort (Subtracting, not Dividing)
*   **The Trap:** Look at the python code: `split = 100`. It calls recursion on `L[0:100]` and `L[100:n]`. 
*   **The Solution:** Is this $O(n \log n)$? **Falsch (False).** 
    It is NOT cutting the array in half! It is biting off exactly 100 elements at a time. The recurrence is $T(n) = T(n-100) + O(n)$. This is a standard arithmetic sum ($n + (n-100) + (n-200) \dots$), which results in a triangle of work. The true runtime is **$O(n^2)$**.

#### 1(f) The Weird Mergesort (Irregular Splits)
*   **The Trap:** `split = n // 5`. It calls recursion on $1/5$ of the array, and $4/5$ of the array.
*   **The Solution:** Is this $O(n \log n)$? **Wahr (True).**
    This is exactly the Recursion Tree trap from our Study Guide (Topic 11). 
    $T(n) = T(n/5) + T(4n/5) + O(n)$. 
    Because the fractions add up to exactly 1 ($1/5 + 4/5 = 1$), the total work on *every single level* of the recursion tree is exactly $n$. The tree depth is logarithmic. Total work is strictly **$\Theta(n \log n)$**.

#### 1(g) Flow Networks: Even Capacities = Even Max Flow?
*   **The Solution:** **Wahr (True).** In Ford-Fulkerson, the Bottleneck ($\Delta$) is calculated using simple subtraction ($c - f$). If you start with 0, and all $c$ are even, the bottleneck will *always* be an even number. You only ever add even numbers together. Thus, the final Max Flow must be even.

#### 1(h) Flow Networks: Odd Capacities = Odd Max Flow?
*   **The Trap:** If even+even = even, then odd capacities make odd flows, right? 
*   **The Solution:** **Falsch (False).** Imagine a Source connected to a Sink by TWO separate parallel pipes, each with a capacity of exactly 1 (an odd number). You push 1 through the top pipe, and 1 through the bottom pipe. The total Max Flow is $1 + 1 = \mathbf{2}$ (an even number!). 

#### 1(j) Where is Expected/Amortized time better than Worst-Case?
*   **The Connection:** You just have to know your data structure quirks.
*   **The Solution:**
    *   **Hash-Tabelle:** Expected time for Insert/Search/Delete is **$O(1)$** (Universal Hashing), but Worst-Case is $O(n)$ (if every single item collides on the same hook).
    *   **Dynamisches Array:** Amortized time for Insert is **$O(1)$** (doubling the array is rare), but Worst-Case is $O(n)$ (the specific moment you have to actually copy the array).

---

### Aufgabe 2: (Mindestens) fünf Freunde
**The Problem:** Given an unsorted array of length $n$, find an interval $[a, b]$ with the absolute minimum width so that it contains at least 5 numbers from the array. Runtime must be $o(n^2)$.

*   **The Trap:** Checking every possible pair takes $O(n^2)$. You need the "Two-Pointer/Sliding Window" trick.
*   **The Solution:** 
    1.  **Sort** the array in ascending order. (Takes **$O(n \log n)$**).
    2.  If the array is sorted, any 5 consecutive elements represent a valid interval. For any starting index $i$, the 5th element is at index $i+4$. 
    3.  Loop $i$ from $0$ to $n-5$. Calculate the width of the interval: `width = A[i+4] - A[i]`. 
    4.  Keep track of the minimum width found. 
    5.  **Runtime:** $O(n \log n)$ for sorting + $O(n)$ for the single loop sweep. Total runtime is $O(n \log n)$, which is strictly faster than $n^2$ ($o(n^2)$). 

---

### Aufgabe 3: Vergessliche Suche (Randomized Search)
**The Problem:** Array length $10n$. Contains $9n$ zeros, $n$ ones. You pick random indices until you hit a 1. What is the expected runtime?

*   **The Connection:** Basic probability (Geometric Distribution).
*   **The Solution:**
    1.  What is the probability $p$ of hitting a 1 on any single random draw? $p = \frac{n}{10n} = \frac{1}{10}$ (10%).
    2.  If an event has a probability $p$, the *Expected Number of Trials* until it happens is exactly $\frac{1}{p}$. 
    3.  $\frac{1}{1/10} = 10$. 
    4.  We expect to pick an index exactly 10 times. Since picking a random number and checking an array index takes $O(1)$ time, doing it 10 times takes $10 \times O(1) = \mathbf{O(1)}$. The expected runtime is strictly Constant!

---

### Aufgabe 4: AVL mit weniger Balance
**The Problem:** Same as AVL, but height diff can be **2** instead of 1. Write the recurrence $f(h)$ for the *minimum* number of nodes to reach height $h$.

*   **The Logic / Solution:**
    *   **4(a) The Recurrence:** To make a tree as tall as possible with as few nodes as possible, we want the subtrees to be as imbalanced as legally allowed. 
        *   Standard AVL: One side is $h-1$, the other is $h-2$. 
        *   New Rule: One side is $h-1$, the other can be **$h-3$**!
        *   Equation: $f(h) = 1 + f(h-1) + f(h-3)$. (The $1$ is the root).
    *   **4(b) Prove $f(h) \ge (\frac{4}{5})^h$:**
        *   *The Trap:* Wait... $4/5 = 0.8$. And $0.8^h$ gets *smaller* and approaches 0 as $h$ grows. But a tree obviously grows! Because $f(h) \ge 1$ for any tree with height $>0$, the statement $f(h) \ge 0.8^h$ is mathematically, trivially true. (This was likely a typo on the exam for $1.xx^h$, but if asked, just state: "Da $f(h) \ge 1$ und $(4/5)^h < 1$, gilt die Aussage trivialerweise.")
    *   **4(c) Upper Bound for Height:**
        *   If we know $n \ge c^h$ (where $c$ is the growth constant of the sequence, similar to Fibonacci's $\approx 1.6$), we can take the logarithm of both sides.
        *   $\log_c n \ge h$. Thus, the maximum height $h$ is strictly bounded by **$O(\log n)$**. 

---

### Aufgabe 5: Kanten aufräumen
**The Problem:** You have a multigraph (multiple edges between the same two nodes). Strip it down to a simple graph by keeping only the cheapest edge between any pair. Graph has nodes $0$ to $n-1$.

#### 5(a) Expected Laufzeit $O(m)$
*   **The Connection:** "Expected $O(1)$" is the universal code word for Hash Tables.
*   **The Solution:** 
    1. Initialize an empty **Hash-Tabelle**.
    2. Loop through all $m$ edges in the graph. For an edge between $u$ and $v$ with weight $w$:
    3. Create a unique key for the pair (e.g., `key = min(u,v) + "-" + max(u,v)`). 
    4. If the key is NOT in the Hash Table, add it: `Hash[key] = w`.
    5. If the key IS in the Hash Table, update it if the new weight is cheaper: `Hash[key] = min(Hash[key], w)`.
    6. Since Hash Table lookups/inserts are *Expected $O(1)$*, processing $m$ edges takes **Expected $O(m)$**.

#### 5(b) Worst-Case (Deterministic) Laufzeit $O(m)$
*   **The Trap:** Hash tables fail in the worst case (collisions). Comparison sorting takes $O(m \log m)$. How do we sort in linear time? 
*   **The Connection:** The nodes are integers from $0$ to $n-1$. We can use **Radix Sort** (Topic 12)!
*   **The Solution:**
    1. Treat every edge $(u, v)$ as a 2-digit number in base-$n$. 
    2. Sort the list of all $m$ edges using **Radix Sort** based on the endpoints $(u,v)$. This perfectly groups all parallel edges together in the array!
    3. Radix Sort on numbers up to $n$ takes $O(m + n)$ time. 
    4. *Crucial Graph Math:* Because the graph is connected, $n$ cannot be larger than $m+1$. Therefore, $O(m + n)$ simplifies strictly to **$O(m)$**.
    5. Finally, do a single $O(m)$ linear scan over the sorted array. For each grouped chunk of identical $(u,v)$ pairs, just find the minimum weight and throw the rest away. Total strict worst-case time: **$O(m)$**.

---

### Aufgabe 6(e): Routenplanung (Das Bürgerbüro)
**The Problem:** Given a road network $G=(V,E)$ with positive times. Find a city $x$ that minimizes the *maximum* shortest path to any other city. Find an algorithm that runs in exactly $O(|V|^{2.1} + |V||E|)$ time.

*   **The Trap:** Don't let the weird exponent $2.1$ confuse you. It's just a sloppy upper bound to give you wiggle room. 
*   **The Solution:**
    1. We need to know the shortest path from *every* city to *every* other city. 
    2. Since all weights are positive, we can run **Dijkstra's Algorithm**. 
    3. But Dijkstra only does SSSP (Single-Source). So, we put Dijkstra inside a `for`-loop and run it starting from *every single node $v \in V$*!
    4. For each run, we look at the array of distances and record the absolute maximum distance. We keep track of whichever city had the smallest maximum.
    5. **Runtime Analysis:** One run of Dijkstra (using a Min-Heap) takes $O(|E| + |V| \log |V|)$. 
    6. Running it $|V|$ times takes: $|V| \times (|E| + |V| \log |V|) = \mathbf{O(|V||E| + |V|^2 \log |V|)}$.
    7. Because $\log |V|$ grows incredibly slowly, it is mathematically strictly smaller than $|V|^{0.1}$. Therefore $|V|^2 \log |V| \in O(|V|^{2.1})$. The runtime bound holds perfectly!

---

### Aufgabe 7: Kantenzusammenhang (3-Edge-Connected)
**The Problem:** Is there an algorithm to check if there are 3 completely separate (edge-disjoint) paths between node $u$ and node $v$? It must run in $O(|E| + |V|)$.

*   **The Connection:** Multiple disjoint paths = Maximum Flow! (Menger's Theorem).
*   **The Solution:**
    1. Model the undirected graph as a **Flow Network**. Set node $u$ as the Source ($s$), and $v$ as the Sink ($t$). 
    2. Set the capacity of *every single edge* in the graph to exactly **1**. 
    3. Run the **Ford-Fulkerson** algorithm. 
    4. Because every edge has capacity 1, an active flow path consumes the edge entirely. If the final Max Flow value is $\ge 3$, it proves there are at least 3 completely independent paths!
    5. **Runtime Proof:** Ford-Fulkerson finds one augmenting path using BFS/DFS, which takes $O(|E| + |V|)$ time. Since we only care if the flow reaches 3, we stop the algorithm after at most 3 paths. Running a linear search 3 times is $3 \times O(|E| + |V|)$, which drops the constant and equals exactly **$O(|E| + |V|)$**.

---

## 📝 Klausur 2021/2022

*(Note: Aufgabe 1(a) through 1(g), and Aufgabe 2 (AVL Tree) were completely recycled in the Probeklausur 25/26. We will skip them here since they are solved above!)*

### Aufgabe 1(h): Scenario: Counting Letters
**The Problem:** You need to count the occurrences of every letter (a, b, ..., z) in Goethe's *Faust*. Do it as efficiently as possible.

*   **The Trap:** Do not use a complex Hash Map or BST for this. The alphabet is strictly limited to 26 characters!
*   **The Solution:** Use an **Array** of size 26. 
    *   Map 'a' to index 0, 'b' to index 1, etc. (using their ASCII value: `char_code - 97`).
    *   Sweep through the book letter by letter. For each letter, increment the integer at that index: `Count[char_code - 97] += 1`.
    *   **Laufzeit:** $O(N)$, where $N$ is the total number of characters in the book. Since the array size is a constant (26), the space complexity is strictly **$O(1)$**.

---

### Aufgabe 3: Ausflug in den Wasserpark (Graph DP)
**The Problem:** Water slides (W) connect platforms (P) going downwards. Each slide has a fun-score $s$. Start at $p_1$, end at $p_n$. Find the path with the maximum total fun-score.

#### 3(a) Graph Model & Finiteness
*   **The Problem:** Model it mathematically and explain why paths are finite.
*   **The Solution:** This is a **Directed Acyclic Graph (DAG)**. 
    *   Nodes = Platforms. Edges = Slides. Edge Weights = Fun Scores.
    *   Because water only flows *downwards* to lower platforms, it is physically impossible to form a cycle (you can't slide back up!). 
    *   Because there are no cycles, every possible path must eventually reach the bottom and terminate. Thus, all paths are finite.

#### 3(b) The Dynamic Programming Algorithm
*   **The Trap:** You can't just use Dijkstra. Dijkstra finds *minimums* and doesn't work well with finding maximums (Longest Path). But because it is a DAG, we can use DP!
*   **The Solution / Algorithm:**
    1.  **Topological Sort:** Sort all platforms so that all slides point left-to-right. (Takes $O(|V| + |E|)$).
    2.  **DP Array:** Create an array $OPT[v]$ which stores the maximum fun to reach platform $v$. Initialize all values to $-\infty$, except $OPT[p_1] = 0$.
    3.  **The Recurrence:** Iterate through the topologically sorted nodes. For each node $v$, look at all incoming slides from some node $u$.
        $$OPT[v] = \max_{\text{incoming } (u,v)} \Big( OPT[u] + s(u,v) \Big)$$
    4.  **Runtime:** We visit each node once and check each edge once. Total time: **$O(|V| + |E|)$**.

#### 3(c) Finding the actual route
*   **The Solution:** Add a `Parent` array. Whenever you update $OPT[v]$ with a new maximum, save the node $u$ that gave you that maximum into `Parent[v] = u`. At the end, start at $p_n$ and trace the `Parent` pointers backward to $p_1$ to reconstruct the exact path!

---

### Aufgabe 4: Kowalski, (amortisierte) Analyse!
**The Problem:** We have a weird data structure with a sorted list $L$ (size $k$) and an unsorted list $R$ (size $n-k$).
*   `insert`: Add to $R$.
*   `delete`: If $L$ has items, pop the largest. If $L$ is empty, grab the $\sqrt{n}$ smallest items from $R$, sort them, put them in $L$, and pop the largest.

#### 4(a) Find $\sqrt{n}$ smallest in Linear Time
*   **The Connection:** Topic 12 (QuickSelect)!
*   **The Solution:** Use the **QuickSelect (Median-of-Medians)** algorithm to find the exact element at index $\lfloor\sqrt{n}\rfloor$ in expected **$O(n)$** time. Then, use that element as a pivot to partition $R$. All elements smaller than the pivot form the new list $L$. 

#### 4(b) Worst-Case Laufzeiten
*   **The Solution:**
    *   `insert(x)`: Just appending to the end of array $R$. Worst-case: **$O(1)$**.
    *   `delete()`: In the worst-case, $L$ is empty. We must do QuickSelect on $R$ ($O(n)$ time), then sort the $\sqrt{n}$ elements using MergeSort/HeapSort. Sorting $\sqrt{n}$ elements takes $O(\sqrt{n} \log \sqrt{n})$. Because $O(n)$ grows faster than $O(\sqrt{n} \log n)$, the partitioning dominates. Worst-case is **$O(n)$**.

#### 4(c) Why is $k \le \sqrt{n}$ always true?
*   **The Solution:** The variable $k$ is the size of $L$. The ONLY time elements are ever added to $L$ is during a `delete` when $L$ is completely empty. We explicitly move exactly $\lfloor\sqrt{n}\rfloor$ elements into $L$. Then, the `delete` operation immediately pops 1 element off! So $L$ instantly drops to $\sqrt{n} - 1$. Therefore, $k$ can never exceed $\sqrt{n}$.

#### 4(d) Amortized Analysis Proof
**The Problem:** Prove `insert` is amortized $O(1)$, and `delete` is amortized $O(\sqrt{n})$ using the potential $\Phi = n_{total} - k^2$.
*(Let $N$ be the total items in the structure. $N = k + |R|$).*

*   **The Insert Proof:**
    *   *Action:* $N$ increases by 1. $k$ stays the same. Cost $c_i = 1$. 
    *   *Math:* $\Delta \Phi = \Phi_{after} - \Phi_{before} = \big((N+1) - k^2\big) - \big(N - k^2\big) = \mathbf{1}$.
    *   *Amortized Cost:* $\hat{c}_i = c_i + \Delta \Phi = 1 + 1 = \mathbf{2}$. Since 2 is a constant, `insert` is amortized **$O(1)$**.
*   **The Delete Proof (When $L$ is empty / Heavy computation):**
    *   *Setup Before:* $k = 0$. $\Phi_{before} = N - 0^2 = N$.
    *   *Action:* We find $\sqrt{N}$ items, sort them, put them in $L$, and delete 1. 
    *   *Cost:* $c_i = N$ (for the QuickSelect partition).
    *   *Setup After:* Total items dropped by 1 $\to (N-1)$. The list $L$ now has $\sqrt{N}-1$ items $\to k = \sqrt{N}-1$.
    *   *New Potential:* $\Phi_{after} = (N-1) - (\sqrt{N}-1)^2 = N - 1 - (N - 2\sqrt{N} + 1) = \mathbf{2\sqrt{N} - 2}$.
    *   *Math:* $\Delta \Phi = (2\sqrt{N} - 2) - (N)$.
    *   *Amortized Cost:* $\hat{c}_i = c_i + \Delta \Phi = N + 2\sqrt{N} - 2 - N = \mathbf{2\sqrt{N} - 2}$.
    *   *Conclusion:* The big $N$ canceled out perfectly! The amortized cost is strictly bounded by **$O(\sqrt{N})$**.

---

### Aufgabe 5: Minimum Spanning Trees (Borůvka’s Algorithm)
**The Problem:** Initially, every node is its own component. In each round, EVERY component simultaneously picks its cheapest outgoing edge and marks it. Marked edges merge components. Repeat until 1 component is left. Prove it works.

#### 5(a) Why distinct weights? (The Counterexample)
*   **The Trap:** What happens if weights tie? 
*   **The Solution:** If weights aren't unique, cycles can form! Imagine a triangle graph (A, B, C) where every edge has weight 1. In round 1, Node A picks edge A-B. Node B picks edge B-C. Node C picks edge C-A. Because they all act simultaneously, we just marked a closed triangle! A Spanning Tree cannot have cycles. Distinct weights prevent this mathematical tie. 

#### 5(b) The Cut Property
*   **The Problem:** Prove the cheapest edge $e_X$ leaving component $X$ is in the MST.
*   **The Solution:** This is the literal definition of the **Schnitt-Eigenschaft (Cut Property)**! If we define a "Cut" that separates all nodes in $X$ from all nodes outside of $X$ ($V \setminus X$), any spanning tree MUST contain at least one edge crossing this cut to connect the graph. If we swap any heavier crossing edge with the absolute cheapest one ($e_X$), the tree becomes cheaper. Thus, $e_X$ must be in the optimal MST.

#### 5(c) & 5(d) Why marked edges form a Spanning Tree
*   **The Solution:** 
    *   By 5(b), every edge we mark is guaranteed to be a valid part of the true MST. We never mark "bad" edges.
    *   Because edge weights are strictly unique, the "clashing" arrows will never form a cycle (e.g., A points to B, but B points to A, they just merge into one block). 
    *   Because the algorithm runs until exactly 1 component is left, it guarantees that all nodes are connected. Connected + Acyclic = Spanning Tree.

#### 5(e) Why $\le \log |V|$ iterations?
*   **The Logic:** In every single round, *every* component picks an edge to connect to another component. This means every component merges with at least one other component.
*   **The Math:** If every component pairs up, the total number of independent components in the graph is cut in half (or better) every single round. If you start with $|V|$ items and repeatedly halve them, it takes exactly **$\log_2 |V|$** steps to reach 1. 

#### 5(f) Total Runtime $O(|E| \log |V|)$
*   **The Solution:** In each of the $\log |V|$ rounds, we have to find the cheapest outgoing edge for every component. We can do this by sweeping through the entire edge list $E$ once, comparing edge weights to the current minimums of the components they touch. 
*   One sweep takes $O(|E|)$ time.
*   We do this sweep $\log |V|$ times.
*   Total time = **$O(|E| \log |V|)$**.

---

## 📝 Nachklausur 2020/2021

### Aufgabe 1: Asymptotisches Wachstum (Sorting Functions)
**The Problem:** Sort these 6 functions by Big-O growth ($f_i \in O(f_j)$).
1.  $f_1(n) = \log_5(n)$
2.  $f_2(n) = (2 - \sin(n))n^3$
3.  $f_3(n) = n^{\sqrt{n}}$
4.  $f_4(n) = \log_3(\sqrt{n})$
5.  $f_5(n) = \log_3(n!)$
6.  $f_6(n) = 4^{\log_2(n)}$

*   **The Trap (The Simplification Phase):** Never try to compare them raw. Simplify the algebra first!
    *   $f_4(n) = \log_3(n^{0.5}) = 0.5 \cdot \log_3(n) = \mathbf{\Theta(\log n)}$.
    *   $f_1(n) = \log_5(n) = \mathbf{\Theta(\log n)}$. (Base doesn't matter for Big-O limits). Thus, **$f_1 \equiv f_4$**.
    *   $f_5(n) = \log(n!)$. Using Stirling's approximation (or the standard rule), $\log(n!) = \mathbf{\Theta(n \log n)}$. 
    *   $f_6(n) = 4^{\log_2 n} = (2^2)^{\log_2 n} = (2^{\log_2 n})^2 = n^2$. So $f_6 = \mathbf{\Theta(n^2)}$.
    *   $f_2(n) = (2 - \sin(n))n^3$. Since $\sin(n)$ just wobbles between -1 and 1, the multiplier is just a constant oscillating between 1 and 3. Constants drop! $f_2 = \mathbf{\Theta(n^3)}$.
    *   $f_3(n) = n^{\sqrt{n}}$. This has a variable in the exponent. It grows faster than any fixed polynomial (like $n^{100}$), but slower than a true exponential (like $2^n$).
*   **The Final Order:**
    $\mathbf{f_1 = f_4 \prec f_5 \prec f_6 \prec f_2 \prec f_3}$
    *(Log $\prec$ Linearithmic $\prec$ Quadratic $\prec$ Cubic $\prec$ Variable-Exponent)*

---

### Aufgabe 2: Eine fehlt (Binary Search Hack)
**The Problem:** You have a strictly sorted array of length $n-1$. It contains unique numbers from $0$ to $n-1$. Exactly *one* number is missing. Find it in strictly **$o(n)$** time. 

*   **The Trap:** $o(n)$ (Little-o) means it MUST be strictly faster than linear time. You are not allowed to use a `for` loop to scan the array!
*   **The Connection:** Fast searching in a sorted array $\to$ **Binary Search** ($O(\log n)$). 
*   **The Solution / Logic:**
    1.  Because the numbers start at $0$ and are perfectly sorted, if no numbers were missing, the value at index $i$ would equal exactly $i$ (`a[i] == i`). 
    2.  The moment a number goes missing, all numbers to the right of it shift one spot to the left. So after the missing number, the condition becomes `a[i] > i`. 
    3.  **The Algorithm:** Do a Binary Search. Look at the middle index `mid`. 
        *   If `a[mid] == mid`, everything on the left is perfectly normal. The missing number MUST be in the right half.
        *   If `a[mid] > mid`, the shift already happened! The missing number MUST be at `mid` or somewhere in the left half. 
    4.  **Runtime:** Binary search cuts the array in half each time. Runtime is exactly $O(\log n)$. Since $\log n \in o(n)$, the requirement is perfectly fulfilled. 
    5.  **Can we do better?** No. To find a specific piece of information in an array of size $N$ using only comparisons, you must follow a decision tree. The mathematical lower bound of a decision tree with $N$ possible answers is $\Omega(\log n)$.

---

### Aufgabe 4: Mitgezählt (Frequency Counting)
**The Problem:** A stream of numbers arrives one by one. The numbers are $B$-bit integers (where $B$ is a constant parameter, e.g., 64). For each new number $f_i$, you must output how many times $f_i$ has appeared so far.
Part A: Worst-case must be $o(i)$.
Part B: Expected runtime must be $O(1)$. 

*   **The Solutions:**
    *   **4(a) The $o(i)$ Worst-Case Solution:** Use an **AVL-Baum (Binary Search Tree)**. 
        *   Every node in the tree stores a number and a `count` variable. 
        *   When $f_i$ arrives, search for it in the AVL tree. If it exists, `node.count += 1`. If not, insert it with `count = 1`. 
        *   Since there are at most $i$ elements currently in the tree, AVL search/insert takes strictly worst-case $O(\log i)$. Since $\log i$ is strictly smaller than $i$, this satisfies $o(i)$.
    *   **4(b) The $O(1)$ Expected Solution:** Use a **Hash-Tabelle**.
        *   Store the number as the Key, and the frequency as the Value. 
        *   When $f_i$ arrives, lookup the key. If it exists, increment the value. If not, add it.
        *   Using a Universal Hash Family, Hash Table lookups and inserts are proven to take Expected $O(1)$ time. 

---

### Aufgabe 5: Min-Max-Spannbaum (The Bottleneck Tree)
**The Problem:** Given a weighted graph. Find a spanning tree $T$ that minimizes the *maximum* edge weight inside it. Runtime must be $O(|E| \log |E|)$.

*   **The Trap:** Professor Hirnriss is trying to trick you! He invented a fancy name "Min-Max-Spannbaum". But minimizing the maximum edge is *literally* one of the fundamental mathematical properties of a standard Minimum Spanning Tree!
*   **The Solution:**
    1.  **The Algorithm:** Just use **Kruskal's Algorithmus**! 
        *   Sort all edges in ascending order (Takes $O(|E| \log |E|)$).
        *   Use a Union-Find data structure to add edges from cheapest to most expensive, skipping those that form cycles.
    2.  **Correctness Proof:** By the "Cycle Property" (Kreiseigenschaft), if you look at any cycle in a graph, the absolute heaviest edge in that cycle can NEVER be part of the optimal Spanning Tree. Kruskal perfectly follows this rule by processing edges from cheapest to heaviest, naturally avoiding the heavy bottleneck edges. Therefore, the resulting MST inherently minimizes the maximum edge weight.
    3.  **Laufzeit:** Sorting takes $O(|E| \log |E|)$. Union-Find operations take almost $O(1)$ amortized. The sorting heavily dominates, fulfilling the $O(|E| \log |E|)$ requirement. 

---

### Aufgabe 7: Geordneter Stapel (Ordered Stack)
**The Problem:** We create an `OrderedPush(x)` function. It uses standard Stack operations. It removes everything smaller than $x$ from the top of the stack, and then pushes $x$. 
Part A: Implement it. 
Part B: Worst case for $n$ operations?
Part C: Prove Amortized is $O(1)$. 

#### 7(a) The Implementation
```python
def OrderedPush(S, x):
    # Pop elements as long as they are smaller than x
    while not S.is_empty():
        temp = Pop(S)
        if temp >= x:
            Push(S, temp) # Put the big one back!
            break
            
    Push(S, x) # Finally, push x
```

#### 7(b) Worst-Case Runtime for 1 Operation
*   **The Problem:** What is the worst-case time for a *single* `OrderedPush` if $n$ operations have happened?
*   **The Solution:** Imagine you did $n-1$ normal pushes of the numbers `[10, 9, 8, 7, 6]`. The stack has size $n-1$. Then you do `OrderedPush(100)`. The `while` loop has to `Pop` every single item off the entire stack before pushing 100. This requires $n-1$ loops. Thus, the strict worst-case for one specific operation is **$O(n)$**.

#### 7(c) Amortized $O(1)$ Proof
*   **The Connection:** This is exactly the same logic as the "Queue made of two Stacks" problem from the Präsenzübungen! We use the Aggregate Method (or Potential).
*   **The Solution (Aggregate Method):**
    *   Look at the entire life cycle of any single integer $x$.
    *   An element $x$ can physically only be `Push`ed onto the stack **exactly 1 time**. 
    *   An element $x$ can physically only be `Pop`ped off the stack **exactly 1 time** (either permanently destroyed by a bigger number, or popped at the very end). 
    *   Therefore, across a sequence of $n$ total operations, the absolute maximum number of `Pop`s that can *possibly* happen is bounded by the total number of items inserted ($\le n$). 
    *   Total work for $n$ operations $\le 2n$ (max $n$ pushes, max $n$ pops). 
    *   Average work per operation = $2n / n = 2$.
    *   Since 2 is a constant, the amortized runtime is strictly **$O(1)$**.
 
---

## 📝 Klausur 2020/2021

### Aufgabe 1: Asymptotisches Wachstum (Sorting Functions)
**The Problem:** Sort these functions. *(Note: The PDF conversion was a bit messy, but based on standard exam patterns, here are the functions)*:
1. $f_1(n) = n^{1/2} = \sqrt{n}$
2. $f_2(n) = 12n$
3. $f_3(n) = 2^{\sqrt{n}}$
4. $f_4(n) = n \log_3 n$
5. $f_5(n) = \log_2(n \log_3 n)$
6. $f_6(n) = n^{1.5} = n \sqrt{n}$

*   **The Logic / Solution:**
    *   **Simplify $f_5$:** By log rules, $\log(A \cdot B) = \log A + \log B$. So $f_5(n) = \log_2(n) + \log_2(\log_3 n)$. The dominant term is just $\log_2 n$. This is the slowest growing function.
    *   **The Roots:** $f_1 = n^{0.5}$. This grows faster than any log, but slower than $n^1$. 
    *   **The Linear & Linearithmic:** $f_2 = 12n \in \Theta(n)$. $f_4 = n \log n$. $f_4$ is strictly slower than $n^{1.5}$.
    *   **The Polynomial:** $f_6 = n^{1.5}$.
    *   **The Exponential:** $f_3 = 2^{\sqrt{n}}$. Any base $>1$ raised to a variable exponent will eventually crush any polynomial. 
*   **The Final Order:**
    $\mathbf{f_5 \prec f_1 \prec f_2 \prec f_4 \prec f_6 \prec f_3}$

---

### Aufgabe 2: Asymptotisches Wachstum (The Sum Proof)
**The Problem:** Prove mathematically that $\sum_{i=1}^n \frac{1}{\sqrt{i}} \in \Theta(\sqrt{n})$.

*   **The Trap:** Do not try to use the Master Theorem on a sum! You have to prove both $O$ (Upper Bound) and $\Omega$ (Lower Bound). 
*   **The Solution:**
    1.  **Lower Bound ($\Omega$):** In the sum $1/\sqrt{1} + 1/\sqrt{2} \dots + 1/\sqrt{n}$, every single fraction is $\ge$ the very last fraction ($1/\sqrt{n}$).
        *   So, $\sum_{i=1}^n \frac{1}{\sqrt{i}} \ge \sum_{i=1}^n \frac{1}{\sqrt{n}} = n \cdot \frac{1}{\sqrt{n}} = \mathbf{\sqrt{n}}$.
        *   This proves $\Omega(\sqrt{n})$.
    2.  **Upper Bound ($O$):** The easiest way to bound a decreasing sum is using an **Integral**.
        *   $\sum_{i=1}^n \frac{1}{\sqrt{i}} \le \int_0^n x^{-1/2} dx$
        *   The antiderivative of $x^{-1/2}$ is $2x^{1/2}$ (which is $2\sqrt{x}$).
        *   Evaluating from $0$ to $n$: $2\sqrt{n} - 0 = \mathbf{2\sqrt{n}}$.
        *   Because it is bounded by $2\sqrt{n}$, we drop the constant 2, proving $O(\sqrt{n})$.
    3.  **Conclusion:** Since it is bounded by both $O$ and $\Omega$, it is exactly **$\Theta(\sqrt{n})$**.

---

### Aufgabe 3: Algorithmenentwicklung (The Sliding Window)
**The Problem:** You have $n$ unsorted real numbers between $[0, 1]$. Find an interval exactly $0.5$ wide ($b - a = 0.5$) that captures the *maximum* possible numbers. Runtime must be $o(n^2)$. 

*   **The Connection:** This is the exact inverse of the "Mindestens fünf Freunde" problem from the 24/25 exam! Instead of a fixed amount of numbers and finding the min width, we have a fixed width and want to find the max numbers!
*   **The Solution:**
    1.  **Sort:** Sort the array $X$ in ascending order. Runtime: **$O(n \log n)$**.
    2.  **Two-Pointer (Sliding Window):** Setup two pointers, `Left = 0` and `Right = 0`. Also `Max_Count = 0`. 
    3.  **The Sweep:** 
        *   While `Right < n`:
        *   Check the distance: `diff = X[Right] - X[Left]`.
        *   If `diff <= 0.5`: This is a valid window! Calculate the number of items inside it (`Right - Left + 1`). If this is $> Max\_Count$, update $Max\_Count$. Then expand the window by moving `Right += 1`.
        *   If `diff > 0.5`: The window is too wide! Shrink it from the back by moving `Left += 1`. 
    4.  **Runtime:** Sorting is $O(n \log n)$. The sweep moves the Left and Right pointers strictly forward a maximum of $n$ times each, taking exactly $O(n)$ time. Total runtime: $O(n \log n) \in \mathbf{o(n^2)}$.

---

### Aufgabe 5: Dynamische Programmierung (The Bookshelf Problem)
**The Problem:** You have a sorted sequence of numbers $a_1 \dots a_n$. You must split them into exactly $k$ contiguous chunks. You want to make sure the *heaviest* chunk is as light as possible (Minimize the Maximum). 
*DP State:* $OPT[i, l]$ = Minimum possible maximum chunk when splitting the first $i$ elements into exactly $l$ chunks.

#### 5(a) Explain the Recurrence Formula
**Formula:** $OPT[i, l] = \min_{j=1 \dots i-1} \Big( \max \{ OPT[j, l-1], \quad a_{j+1} + \dots + a_i \} \Big)$
*   **The Explanation:** 
    *   To split $i$ items into $l$ chunks, we have to decide where to place the *very last divider*. Let's say we place it exactly after index $j$.
    *   This leaves us with two things:
        1. A final chunk on the right containing items $a_{j+1}$ to $a_i$. Its weight is simply the sum of those items.
        2. A left section of $j$ items that must be optimally split into the remaining $l-1$ chunks. We already know the optimal answer for this from our DP table: $OPT[j, l-1]$.
    *   Because we want the "Maximum" of any chunk in the whole system, the heaviest chunk for this specific guess $j$ is the `max` of the left side's heaviest chunk AND the right side's sum. 
    *   We try every possible splitting point $j$, and greedily pick the one that gives us the lowest (`min`) maximum!

#### 5(b) Base Cases (Anfangsbedingungen)
*   **The Logic:**
    1.  $OPT[i, 1] = \sum_{m=1}^i a_m$. *(If you only have 1 chunk, you can't split it. The weight is just the sum of all elements).*
    2.  $OPT[1, l] = a_1$. *(If you only have 1 element, but you are asked to make $l$ chunks, you just put $a_1$ in the first chunk, and leave the others empty (sum=0). The max chunk is $a_1$).*

#### 5(c) Algorithm & Runtime
*   **The Trap:** The naive runtime is filling an $n \times k$ table. For every cell, we loop $j$ up to $n$, and inside that we calculate a sum. This takes $O(n^3 k)$. We need to optimize the sum!
*   **The Optimization:** Pre-calculate a **Prefix-Sum Array** $P$. $P[x] = a_1 + \dots + a_x$. 
    *   Now, the sum $a_{j+1} \dots a_i$ can be calculated instantly in $O(1)$ time by doing $P[i] - P[j]$.
*   **The Runtime:** We fill a DP table of size $n \times k$. For each cell, we run a loop $j$ from $1$ to $i$ (which takes $O(n)$ steps). Inside the loop, the math is $O(1)$. Total runtime is exactly **$O(n^2 k)$**.

---

### Aufgabe 6: Kürzeste Wege (Supermarket vs Fire Station)

#### 6(a) The Supermarket (Minimize the Sum)
**The Problem:** Find a node $s$ that minimizes the *sum* of distances to all other nodes. Runtime $O(|V|^3)$.
*   **The Connection:** We need a full distance matrix. $O(|V|^3)$ is the universal code word for Floyd-Warshall!
*   **The Solution:**
    1. Run the **Floyd-Warshall** algorithm to compute the All-Pairs Shortest Path (APSP) matrix in $O(|V|^3)$ time. 
    2. Add a variable `global_min = Infinity` and `best_node = None`. 
    3. Loop through each row of the matrix (each node $v$). Sum up all the distances in that row. 
    4. If the row's sum is $< global\_min$, update it!
    5. Because summing the rows takes $O(|V|^2)$, the $O(|V|^3)$ Floyd-Warshall completely dominates the runtime.

#### 6(b) The Fire Station (Minimize the Maximum)
**The Problem:** Find a node $f$ that minimizes the *maximum* distance to any other node. Runtime $O(|V|^{2.5} + |V||E|)$.
*   **The Connection:** This is exactly Aufgabe 6e from the 24/25 Exam!
*   **The Solution:**
    1. We want APSP, but the runtime bound tells us Floyd-Warshall ($O(|V|^3)$) is too slow!
    2. Instead, run **Dijkstra's Algorithm** (with a Min-Heap) starting from *every single node* $u \in V$. 
    3. For each run of Dijkstra, look at the output array and find the `max` distance. Keep track of the node that produced the smallest `max`.
    4. **Runtime Proof:** Dijkstra takes $O(|E| + |V| \log |V|)$. Running it $|V|$ times takes $O(|V||E| + |V|^2 \log |V|)$. Because $\log |V| \le |V|^{0.5}$ asymptotically, $|V|^2 \log |V|$ fits perfectly inside $O(|V|^{2.5})$.

       ---

## 📝 Unbekannte Probeklausur (JGU Mainz)

### Aufgabe 1: Ja-Nein-Fragen (Rapid Fire)

*   **1. Ist $f \in \Omega(n^3)$ und $f(n)>0$, dann ist $\frac{n}{f} \in O(n^2)$.**
    *   *The Logic:* If $f(n)$ grows at least as fast as $n^3$, then dividing $n$ by $f(n)$ means you are doing $n / n^3$, which simplifies to $1/n^2$. As $n$ gets huge, $1/n^2$ shrinks towards 0. Since $0$ is strictly bounded by $O(n^2)$, this is mathematically **Wahr (True)**.
*   **2. Radixsort sorts a set $S \subset \mathbb{N}$ in $O(|S| \log(\max S))$ time.**
    *   *The Logic:* Radixsort's runtime is $O(d \cdot n)$, where $d$ is the number of digits. How many digits does the maximum number have in binary? Exactly $\approx \log_2(\max S)$. So the runtime is indeed $O(n \cdot \log(\max S))$. **Wahr (True)**.
*   **3. Skiplist expected search is $O(\log n)$, but expected space is $\Omega(n \log n)$.**
    *   *The Trap:* Does a Skiplist really take $n \log n$ space? No! Every time a node is inserted, it flips a coin to decide if it grows a level. The expected number of pointers per node is 2 (a geometric series: $1 + 1/2 + 1/4 \dots = 2$). Total expected space is $O(n)$. **Falsch (False)**.
*   **4. 2-universal Hashing worst-case search is $O(1)$.**
    *   *The Trap:* Universal Hashing guarantees *expected* $O(1)$. If you get insanely unlucky and the random function puts every single item in the same bucket, the worst-case is still $O(n)$. **Falsch (False)**.
*   **5. The given AVL-Tree needs no rebalancing.**
    *   *The Logic:* You just have to calculate the balance factors (Left Height - Right Height). 
        *   Node 13: Left is 1 (height 0), Right is empty. Balance = +1. 
        *   Node 60: Left is empty, Right is 68 (height 0). Balance = -1.
        *   Node 54: Left is 50 (height 0), Right is 60 (height 1). Balance = -1. 
        *   Node 25 (Root): Left subtree (13) has height 1. Right subtree (54) has height 2. Balance = -1. 
    *   Since no node has a balance of 2 or -2, the AVL rule is perfectly maintained! **Wahr (True)**.

---

### Aufgabe 2: O-Notation und Rekurrenzen
*   **2(a) Proofs:**
    *   $2^{n+1} \in O(2^n)$: **Wahr.** $2^{n+1} = 2 \cdot 2^n$. The 2 is a constant and drops.
    *   $2^{2n} \in O(2^n)$: **Falsch.** $2^{2n} = (2^2)^n = 4^n$. 4^n grows astronomically faster than 2^n.
    *   $\log(n!) \in O(n \log n)$: **Wahr.** Stirling's approximation proves $\log(n!) \approx n \log n - n$. 
*   **2(b) Recurrences:**
    *   $T_1(n) = 2 T_1(n/2) + n \log n$. 
        *   *The Trap:* Master Theorem fails here! Clones do $n^1$. Toll fee is $n \log n$. They are too close!
        *   *The Extension:* When the toll fee is exactly the clones' work multiplied by $\log^k n$, you just add another $\log$ to the result. 
        *   *Result:* **$\Theta(n \log^2 n)$**. 
    *   $T_2(n) = 16 T_2(n/4) + 3n^2$. 
        *   $a=16, b=4$. Clones do $n^{\log_4 16} = n^2$. Toll fee is $3n^2$. 
        *   *Battle:* $n^2$ vs $n^2$. Perfect tie (Case 2). 
        *   *Result:* **$\Theta(n^2 \log n)$**.

---

### Aufgabe 4: DSEA sucht den Superstar! (The Celebrity Problem)
**The Problem:** There are $n$ people at a party. A "Star" is someone who is known by EVERYONE, but knows NOBODY. You can only ask "Does Person A know Person B?". Find the Star in $O(n)$ questions.

*   **The Trap:** If you ask everyone about everyone, that's $O(n^2)$. 
*   **The Logic / Solution (The Elimination Game):**
    1.  Pick any two people, A and B. Ask: "Does A know B?"
    2.  *Case 1 (Yes):* A knows B. A Star knows nobody! Therefore, **A cannot be the Star.** Eliminate A. 
    3.  *Case 2 (No):* A does not know B. Everyone knows the Star! Therefore, **B cannot be the Star.** Eliminate B.
    4.  **The Sweep:** Put all $n$ people in a line. Compare person 1 and 2. Eliminate one. Compare the survivor with person 3. Eliminate one.
    5.  Every single question you ask permanently eliminates exactly 1 person. After $n-1$ questions, exactly 1 person remains! This is your "Star Candidate".
    6.  **The Verification:** Just because they survived doesn't mean a Star actually exists at the party! Take the Candidate and ask the remaining $n-1$ people: "Do you know the Candidate?" and ask the Candidate "Do you know them?". If they pass all verification checks (takes $2n$ questions), they are the Star. 
    7.  Total questions: $(n-1) + 2n = \mathbf{O(n)}$.

---

### Aufgabe 5: Amortisierte Analyse (Queue via 2 Stacks)
**The Problem:** Implement a Queue (First-In, First-Out) using two Stacks (First-In, Last-Out) $S$ and $T$. Prove that `add` and `del` have an amortized cost of $O(1)$. 
*Code:* 
`add(k)`: just pushes to $S$.
`del()`: If $T$ is empty, pop everything from $S$ and push it into $T$. Then pop from $T$. 

*   **5(a) Trace it:**
    *   `add(1), add(2)` $\to$ S: [1, 2], T: []
    *   `del()` $\to$ T is empty. Pop S to T: S is [], T is [2, 1]. Return T.pop() $\to$ Returns 1. T is now [2].
    *   `add(3), add(4)` $\to$ S: [3, 4], T: [2].
    *   `del()` $\to$ T is not empty! Just return T.pop() $\to$ Returns 2. T is now [].
*   **5(b) Amortized $O(1)$ Proof:**
    *   *The Trap:* `del()` looks like $O(n)$ because of the `while` loop!
    *   *The Aggregate Proof:* Track the complete lifespan of a single element $x$.
    *   When $x$ is `add`ed, it is **pushed onto S (Cost: 1)**.
    *   Eventually, a `del` operation triggers a transfer. $x$ is **popped from S (Cost: 1)** and immediately **pushed onto T (Cost: 1)**.
    *   Eventually, $x$ reaches the top of T and is **popped from T and returned (Cost: 1)**. 
    *   Throughout its entire life in the data structure, $x$ undergoes exactly 4 stack operations. 
    *   If you do $n$ operations, the absolute maximum total work done across the entire system is $4n$. 
    *   Average cost per operation = $4n / n = 4$. Since 4 is a constant, the amortized cost is strictly **$O(1)$**. 

---

### Aufgabe 7: Suchen (The Oil Pipeline)
**The Problem:** You have $n$ oil wells at different $(x,y)$ coordinates. You want to build a horizontal pipeline (East-West). Every well needs a vertical pipe connecting to it. Where do you place the horizontal pipe to minimize the total length of the vertical pipes? Find it in expected $O(n)$ time. 

*   **The Logic / Solution:**
    1.  Because the pipeline is horizontal, the $x$-coordinates of the oil wells are completely irrelevant! We only care about their $y$-coordinates.
    2.  We want to find a $y_{pipe}$ that minimizes $\sum |y_i - y_{pipe}|$. 
    3.  *The Statistical Fact:* The mathematical value that minimizes the sum of absolute differences is ALWAYS the **Median**! (Not the average/mean). 
    4.  *The Algorithm:* We just need to find the Median of the $y$-coordinates. 
    5.  Extract all $y$-coordinates into an array. Run the **QuickSelect (Median-of-Medians)** algorithm. This finds the exact median element in expected **$O(n)$** time!
 
    6. ### Erstklausur 25/26 Gedächnisprotokoll + Example exercises included
 
    7. ---

## 🚨 THE "FIRST EXAM" INTEL (Prep for the Retake)

Based on intelligence gathered from the first exam round, these 5 exact problem types were the core focus. Here is a custom practice exercise and "Chewed" solution for each one.

### 1. Huffman Kodierung (von einem Wort)
**The Problem:** Erstelle den optimalen Huffman-Code für das Wort **"TATTOO"**.

*   **The Solution (Step-by-Step):**
    1.  **Count Frequencies:** T: 3, O: 2, A: 1.
    2.  **Create the Queue (Sorted):** `[A:1]`, `[O:2]`, `[T:3]`.
    3.  **Merge 1:** Grab the two smallest (`A:1` and `O:2`). Merge them into a new parent node `(AO):3`.
    4.  **Update Queue:** The remaining items are `[T:3]` and `(AO):3`. 
    5.  **Merge 2:** Grab the last two and merge them into the Root node `(T-AO):6`. 
    6.  **Draw the Tree:**
        ```text
               Root(6)
              /      \
            T(3)    (AO)(3)
                    /    \
                  A(1)  O(2)
        ```
    7.  **Assign Bits:** Left = `0`, Right = `1`. 
        *   Path to T: Left $\to$ **0**
        *   Path to A: Right, Left $\to$ **10**
        *   Path to O: Right, Right $\to$ **11**
    8.  **Final Code for "TATTOO":** `0` `10` `0` `0` `11` `11` (010001111).

---

### 2. AVL Baum (Einfügen mit Operationen)
**The Problem:** Fügen Sie die Zahlen `[10, 20, 30, 25]` nacheinander in einen leeren AVL-Baum ein. Zeichnen Sie die Rotationen.

*   **The Solution (Step-by-Step Trace):**
    1.  **Insert 10, 20:** `10 -> R:20`. (Balance OK).
    2.  **Insert 30:** `10 -> R:20 (R:30)`. 
        *   *Imbalance:* Node 10 has Left=0, Right=2. Balance is -2.
        *   *Case:* Right-Right (Straight line).
        *   *Fix:* **Left-Rotation at 10**. 20 moves up to Root. 10 drops to its left.
        *   *Tree:* `20 -> L:10, R:30`.
    3.  **Insert 25:** `20 -> L:10, R:30 (L:25)`.
        *   *Imbalance:* Node 20 has Left=1, Right=2 (Wait, balance is +1, that's fine). 
        *   Let's check Node 30: Left=1, Right=0. Balance +1. That's fine too!
        *   *Wait, let's insert **28** instead of 25 to force a double rotation for practice!*
    4.  **Insert 28:** (Assume tree is `20 -> L:10, R:30 (L:25)` and we insert 28).
        *   28 goes to the Right of 25. 
        *   *Tree:* `20 -> L:10, R:30 (L:25 (R:28))`.
        *   *Imbalance:* Check Node 30! Left height is 2, Right is 0. Balance = +2. 
        *   *Case:* Left-Right "Dog-leg" (Heavy on 30's Left child, but that child is heavy on its Right).
        *   *Fix:* **Double Rotation (Left-Right)**.
            1. Left rotate child (25). 28 moves up, 25 drops left. 
            2. Right rotate parent (30). 28 moves up to replace 30. 30 drops right.
        *   *Resulting Subtree:* `28 -> L:25, R:30`. 

---

### 3. Der "Magische" MergeSort-Schritt (Speicherplatz $n/2$)
**The Problem:** Standard MergeSort requires a temporary array of size $n$. Explain an algorithm that merges two sorted halves of an array in $O(n)$ time using only $n/2$ extra memory.

*   **The Solution (The Logic & Example):**
    *   *Setup:* Imagine an array of size 6: `A = [3, 8, 9 | 1, 4, 7]`. The left half and right half are already sorted internally. 
    *   *Step 1:* Allocate a `Temp` array of size exactly $n/2$ (size 3). 
    *   *Step 2:* Copy ONLY the left half into `Temp`. 
        *   `Temp = [3, 8, 9]`. 
        *   Now, indices 0, 1, and 2 in `A` are considered "empty space"!
    *   *Step 3:* Set up 3 pointers. 
        *   `p1` starts at `Temp[0]` (value 3).
        *   `p2` starts at the right half `A[3]` (value 1).
        *   `write` starts at `A[0]`.
    *   *Step 4 (The Merge):* Compare `Temp[p1]` and `A[p2]`. Write the smaller one into `A[write]`.
        *   1 is smaller than 3. Write 1 to `A[0]`. Move `p2` and `write` forward. 
        *   3 is smaller than 4. Write 3 to `A[1]`. Move `p1` and `write` forward.
    *   *Why it's safe:* Because the `write` pointer starts at index 0 and only moves right, and `p2` starts at index 3 and only moves right, the `write` pointer can **never mathematically overtake** `p2`. You will never accidentally overwrite data in the right half before you've safely processed it!

---

### 4. Hash Map Aufgabe (mit Linear Probing)
**The Problem:** Gegeben ist eine Hash-Tabelle der Größe $m=7$. Fügen Sie die Schlüssel `[10, 24, 17, 31]` nacheinander ein. 
*Hash-Funktion:* $h(k) = (2k + 1) \pmod 7$. 
*Kollisionsbehandlung:* Linear Probing (Gehe +1 Schritt nach rechts).

*   **The Solution (Step-by-Step Trace):**
    *   *Empty Table:* `[ ][ ][ ][ ][ ][ ][]` (Indices 0 to 6)
    *   **Insert 10:**
        *   $h(10) = (2(10) + 1) \pmod 7 = 21 \pmod 7 = \mathbf{0}$.
        *   Index 0 is empty. Insert 10 at **Index 0**.
    *   **Insert 24:**
        *   $h(24) = (2(24) + 1) \pmod 7 = 49 \pmod 7 = \mathbf{0}$.
        *   *Collision!* Index 0 is taken by 10. Linear Probing says: check Index 1. 
        *   Index 1 is empty. Insert 24 at **Index 1**. 
    *   **Insert 17:**
        *   $h(17) = (2(17) + 1) \pmod 7 = 35 \pmod 7 = \mathbf{0}$.
        *   *Collision!* Index 0 is taken. Probing: Check 1 (taken). Check 2.
        *   Index 2 is empty. Insert 17 at **Index 2**.
    *   **Insert 31:**
        *   $h(31) = (2(31) + 1) \pmod 7 = 63 \pmod 7 = \mathbf{0}$.
        *   *Collision!* Indices 0, 1, 2 are taken. Probing: Check 3.
        *   Index 3 is empty. Insert 31 at **Index 3**.
    *   *Final Table:* `[10][24][17][31][ ][ ][ ]`
    *   *(Exam Hack: Always write down the exact math $21 \pmod 7 = 0$ so the grader sees your work if you make an arithmetic error!)*

---

### 5. Greedy Algorithmus (Interval Scheduling)
**The Problem:** Sie haben 4 Meetings mit Start- und Endzeiten. Finden Sie die maximale Anzahl an Meetings, die in einem Raum stattfinden können, ohne sich zu überschneiden. 
*   M1: Start 9:00, End 10:30
*   M2: Start 10:00, End 11:00
*   M3: Start 10:45, End 12:00
*   M4: Start 8:00, End 11:30

*   **The Trap:** If you sort by "Shortest Meeting" or "Earliest Start Time", the Greedy algorithm fails.
*   **The Solution (The "Earliest End Time" Rule):**
    1.  **Sort all meetings purely by their END time:**
        *   M1 (Ends 10:30)
        *   M2 (Ends 11:00)
        *   M4 (Ends 11:30)
        *   M3 (Ends 12:00)
    2.  **The Greedy Sweep:** Always pick the first meeting on the sorted list. Then, cross out any meetings that overlap with it. 
        *   Pick **M1**. The room is busy until 10:30. 
        *   Look at M2 (Starts 10:00). *Overlap!* Cross it out.
        *   Look at M4 (Starts 8:00). *Overlap!* Cross it out. 
        *   Look at M3 (Starts 10:45). *Fits perfectly!* Pick **M3**. The room is busy until 12:00.
    3.  **Result:** The optimal maximum number of meetings is 2 (M1 and M3).
    4.  

    
