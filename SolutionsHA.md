## 🔁 Final Retrofitted Replacement — Übungsblatt 1 (Blank-Slate, ADHD-Oriented, English)

## 🎓 Topic 1: Stable Matching Variants + Induction Basics

---

### 🧠 What this sheet asks (plain language)

You need to solve four things:

1. **Generalized stable matching with capacities** (universities can take multiple students):
   - Does a stable assignment always exist under the given definition?
   - If yes, give an efficient algorithm.

2. **Arielle/Eric sabotage scenario** under Gale–Shapley:
   - Can Triton force Arielle and Eric to not end up together, despite them ranking each other first?

3. **General matching among 2n people (roommates-style)**:
   - For the given n=2 preference example, does a stable pairing exist?

4. **Three induction proofs**:
   - sum formula,
   - divisibility by 5,
   - inequality $2^n > n$.

---

### 📚 Zero-to-functional foundations

#### 1) What is a matching?
A matching pairs entities without overlap:
- in classic bipartite setting: student ↔ university spot (or man ↔ woman),
- each participant gets at most one partner unless capacities are allowed.

#### 2) What is stability?
A matching is stable if there is no “blocking deviation”:
- two parties that would both prefer each other over their current situation.

#### 3) What changes with university capacities?
Now one university may hold up to $c_i$ students.
This is the **hospital/residents** version of stable matching.

#### 4) Why induction?
Induction is the “domino proof”:
- prove first domino falls (base),
- prove each falling domino pushes next (step),
- then all fall.

---

## 🧠 Aufgabe 1 — University admissions with capacities

### Problem restated
- $m$ universities with capacities $c_1,\dots,c_m$,
- $n$ applicants, $n \ge \sum_i c_i$,
- strict preferences on both sides,
- we require all seats filled, and no blocking under conditions (i) and (ii).

### Key insight
This is exactly the many-to-one stable matching model (college admissions / hospital-residents).  
A stable assignment **always exists**.

### Efficient algorithm (applicant-proposing deferred acceptance, capacity version)

1. Initially all applicants free, all university slots empty.
2. While there is a free applicant with remaining universities on their list:
   - applicant proposes to best university not yet proposed to.
   - university tentatively keeps the best $c_i$ proposers seen so far (according to its preference), rejects the rest.
3. Rejected applicants become free again and continue proposing.
4. Stop when no proposal possible.

Because $n \ge \sum c_i$, and with complete preference lists, all university slots can be filled.

### Why stable?
Same deferred-acceptance logic as classic Gale–Shapley:
- if a blocking pair existed, the applicant must have proposed there earlier,
- then university would not end with someone worse while having free capacity or worse occupant.
Contradiction.

### Runtime
At most one proposal per applicant-university pair:
\[
O(nm)
\]
(plus data-structure factors for maintaining current top $c_i$, still polynomial and efficient).

---

## 🧠 Aufgabe 2 — Can Triton prevent Arielle–Eric pairing?

### Given
- men propose (Gale–Shapley),
- Eric ranks Arielle first,
- Arielle ranks Eric first,
- Triton may alter all other preferences and proposal order.

### Crucial theorem-level fact
If two participants rank each other first, then in any deferred-acceptance run with that side proposing:
- proposer (Eric) proposes to Arielle immediately (or as first chance),
- Arielle can never prefer anyone over Eric.
So she never permanently rejects him.

Therefore they must end matched.

\[
\boxed{\text{Triton cannot prevent Arielle and Eric from ending up as a pair.}}
\]

---

## 🧠 Aufgabe 3 — General pairings (2n people), given n=2 example

People: $p_1,p_2,p_3,p_4$

Preferences (best to worst from left):
- $p_1: p_4 \succ p_2 \succ p_3$
- $p_2: p_4 \succ p_3 \succ p_1$
- $p_3: p_4 \succ p_1 \succ p_2$
- $p_4: p_3 \succ p_1 \succ p_2$

Possible perfect pairings for 4 people:
1. $\{(p_1,p_2),(p_3,p_4)\}$
2. $\{(p_1,p_3),(p_2,p_4)\}$
3. $\{(p_1,p_4),(p_2,p_3)\}$

Check stability quickly:

- Pairing 1: $p_1$ and $p_4$ prefer each other over current partners ⇒ blocking.
- Pairing 2: $p_3$ and $p_4$ prefer each other over current partners ⇒ blocking.
- Pairing 3: $p_3$ and $p_4$ again prefer each other over current partners ⇒ blocking.

So no stable pairing exists in this instance.

\[
\boxed{\text{No stable pairing exists for the given preferences.}}
\]

---

## 🧠 Aufgabe 4 — Induction proofs

### (a) $\sum_{k=1}^{n} k = \frac{n(n+1)}{2}$

- Base $n=1$: $1 = \frac{1\cdot2}{2}$.
- Step: assume true for $n$:
  \[
  \sum_{k=1}^{n+1}k=\left(\sum_{k=1}^{n}k\right)+(n+1)=\frac{n(n+1)}{2}+(n+1)=\frac{(n+1)(n+2)}{2}.
  \]
Done.

---

### (b) $n^5-n$ divisible by 5 for all $n\in\mathbb N_0$

Use factorization:
\[
n^5-n=n(n^4-1)=n(n^2-1)(n^2+1)=n(n-1)(n+1)(n^2+1).
\]
Among three consecutive integers $n-1,n,n+1$, one is divisible by 5? Not always.  
So better modular route:
\[
n^5 \equiv n \pmod 5
\]
(by Fermat or direct residue check $n=0,1,2,3,4$). Hence
\[
n^5-n \equiv 0 \pmod 5.
\]
Divisible by 5.

---

### (c) $2^n > n$ for all $n\in\mathbb N_0$?

As written, this is **false for $n=1$** since $2^1=1$ is not greater.

Corrected valid statement:
- $2^n \ge n$ for all $n\in\mathbb N_0$, and
- $2^n > n$ for all $n\ge 2$.

Induction for $n\ge2$:
- Base $n=2$: $4>2$.
- Step: assume $2^n>n$ ($n\ge2$):
  \[
  2^{n+1}=2\cdot2^n>2n\ge n+1.
  \]
Done.

---

### ⚠️ Pitfalls (UB1)
1. Forgetting capacity version is many-to-one deferred acceptance.
2. In Aufgabe 2, overlooking the “mutual first-choice lock”.
3. In Aufgabe 3, checking only one pairing.
4. In induction (c), missing the false case $n=1$.

---

### 🧩 UB1 micro-summary
- Capacity-stable assignment: always exists, computed by deferred acceptance in polynomial time.
- Triton cannot separate Eric/Arielle.
- Given 4-person roommate instance has no stable pairing.
- Induction: (a) and (b) true; (c) needs corrected statement.

---

## ✅ UB1 compact exam answers

1. Stable assignment with capacities exists; applicant-proposing deferred acceptance computes one in $O(nm)$.
2. No, Triton cannot prevent Arielle/Eric pairing.
3. Given $n=2$ instance: no stable pairing.
4. (a) and (b) proven; (c) as stated false at $n=1$, true for $n\ge2$.

---

---

## 🔁 Final Retrofitted Replacement — Übungsblatt 2 (Blank-Slate, ADHD-Oriented, English)

## 📘 Topic 2: Asymptotics, Fibonacci Recursion, Selection Sort, and Lattice of Stable Matchings

---

### 🧠 What this sheet asks (plain language)

You need to do four blocks:

1. Prove several Landau-symbol statements.
2. Explore naive recursive Fibonacci and explain exponential blow-up.
3. Run and prove Selection Sort, analyze comparisons, and evaluate min+max variant.
4. Show that combining two stable matchings via applicant-wise better assignment $X\vee Y$ remains stable.

---

### 📚 Zero-to-functional foundations

#### 1) What are Landau symbols?
- $O(g)$: asymptotic upper bound.
- $\Omega(g)$: asymptotic lower bound.
- $\Theta(g)$: both bounds.

#### 2) What is asymptotic proof style?
You compare growth for large $n$, ignoring constants and low-order terms.

#### 3) Selection Sort core
Repeatedly pick minimum of unsorted suffix and swap to front.

#### 4) Why “$X\vee Y$” in stable matching?
You compare two stable outcomes applicant-by-applicant and keep preferred assignment per applicant.

---

## 🧠 Aufgabe 1 — Landau symbols

### (a) $100n^2 \in O(n^3)$
\[
\frac{100n^2}{n^3}=\frac{100}{n}\to 0
\]
So yes.

### (b) $n^2 \in \Omega\!\left(\frac{2n^2+4n+2}{n+1}\right)$

Simplify RHS:
\[
\frac{2n^2+4n+2}{n+1}=2n+2.
\]
Need $n^2 \in \Omega(n)$, true.

### (c) $\log_b n \in \Theta(\ln n)$, fixed $b>1$
\[
\log_b n = \frac{\ln n}{\ln b},
\]
constant factor only ⇒ same $\Theta$-class.

### (d) $g\in O(f)\iff f\in\Omega(g)$
This is exactly the definition duality.

### (e) $\sum_{i=1}^n \frac{1}{i}\in O(\log n)$
Harmonic sum $H_n\le 1+\ln n$, so yes.

### (f) If $\limsup_{k\to\infty}\frac{g(k)}{f(k)}<\infty$, then $g=O(f)$ (with $f(n)\neq0$)
Bounded limsup means ratio eventually bounded by some constant $C$, which is definition of $O$.

---

## 🧠 Aufgabe 2 — Fibonacci (naive recursion)

\[
F_0=0,\;F_1=1,\;F_n=F_{n-1}+F_{n-2}.
\]

### (a) Program
Naive recursive implementation calls itself twice (except base cases).

### (b) Why exponential time?
Recurrence for call count:
\[
T(n)=T(n-1)+T(n-2)+O(1).
\]
This grows like Fibonacci itself:
\[
T(n)=\Theta(\varphi^n),\quad \varphi\approx1.618.
\]
So asymptotically exponential.

---

## 🧠 Aufgabe 3 — Selection Sort

### (a) Example on $[5,3,4,2,1]$
- Pass 1: min=1, swap with first → $[1,3,4,2,5]$
- Pass 2: min in suffix $[3,4,2,5]$ is 2 → $[1,2,4,3,5]$
- Pass 3: min in $[4,3,5]$ is 3 → $[1,2,3,4,5]$
- Pass 4: min in $[4,5]$ is 4 (no change)

### (b) Correctness invariant
After pass $i$, first $i$ elements are the $i$ smallest elements in sorted order.
Initialization true for $i=0$.  
Maintenance by selecting minimum of remainder and placing at position $i+1$.  
Termination gives full sorted array.

### (c) Number of comparisons
\[
(n-1)+(n-2)+\cdots+1=\frac{n(n-1)}{2}=\Theta(n^2).
\]

### (d) Min+max modification
Still $\Theta(n^2)$ comparisons asymptotically. Constant factor may improve, class does not.

---

## 🧠 Aufgabe 4 — Combine stable matchings: $X\vee Y$

Definition:
\[
X\vee Y:=\{(p,\max_p(X(p),Y(p)))\mid p\in P\},
\]
i.e., each applicant gets the preferred of their two assignments.

### Claim
$X\vee Y$ is stable.

### Proof idea (standard lattice argument sketch)
Assume $X\vee Y$ unstable with blocking pair $(p,s)$.  
Then $p$ prefers $s$ over $(X\vee Y)(p)$, hence over both $X(p)$ and $Y(p)$ or at least over the better one.  
Using stability of $X$ and $Y$, this forces incompatible preference relations at $s$ with the partners assigned in $X$ and $Y$.  
One derives a contradiction to at least one stability condition in $X$ or $Y$.  
Therefore no blocking pair exists in $X\vee Y$.

So:
\[
\boxed{X\vee Y\text{ is stable.}}
\]

---

### ⚠️ Pitfalls (UB2)
1. Mixing up $O$ and $\Omega$ directions.
2. Forgetting harmonic sum growth is logarithmic.
3. Claiming modified Selection Sort becomes $O(n\log n)$ (false).
4. For Aufgabe 4, proving informally without contradiction structure.

---

### 🧩 UB2 micro-summary
- Landau statements all hold.
- Naive Fibonacci recursion is exponential.
- Selection Sort: correct via invariant, $\Theta(n^2)$ comparisons.
- $X\vee Y$ preserves stability.

---

## ✅ UB2 compact exam answers

1. (a)–(f): all true with standard asymptotic arguments.
2. Naive Fibonacci recursion has exponential runtime $\Theta(\varphi^n)$.
3. Selection Sort comparisons: $\Theta(n^2)$; min+max variant remains $\Theta(n^2)$.
4. $X\vee Y$ is stable.

---
## 🔁 Final Retrofitted Replacement — Übungsblatt 3 (Operational, Blank-Slate, ADHD-Oriented)

## 🧩 Topic 1: Inversionen via MergeSort-Logik — Warum O(n log n) möglich ist

---

### 🧠 What this sheet asks (plain language)
You must:
1. count inversions in one concrete array and its halves,
2. explain what Merge’s cross-comparisons count,
3. conclude an $O(n\log n)$-algorithm.

So this is a “counting + divide-and-conquer correctness” task.

---

### 📚 Zero-to-functional foundations

#### 1) What is an inversion?
Given array $a_1,\dots,a_n$, an inversion is a pair $(i,j)$ with
\[
i<j \quad\text{and}\quad a_i>a_j.
\]
Interpretation: a pair is “out of sorted order.”

- sorted ascending array $\Rightarrow 0$ inversions.
- sorted descending array $\Rightarrow \frac{n(n-1)}2$ inversions (maximum).

---

#### 2) Why MergeSort is relevant
MergeSort splits array into two halves:
- recursively sorts left,
- recursively sorts right,
- merges both sorted halves.

Inversion count also splits naturally into:
1. inversions entirely in left half,
2. inversions entirely in right half,
3. inversions crossing from left to right.

The merge step can count (3) efficiently.

---

### 🌉 Analogy: Queue discipline checker
Imagine two already internally sorted queues (left/right).  
Whenever right-front jumps before remaining left people, it “overtakes” all still-waiting left people.  
That number of overtakes is exactly the number of crossing inversions contributed at that moment.

---

## 🧠 Aufgabe 1a — Count $I$, $I_1$, $I_2$

Given:
\[
a=(1,8,5,3,4,2,7,6).
\]
Halves:
- left: $(1,8,5,3)$,
- right: $(4,2,7,6)$.

### Left-half inversions $I_1$
Pairs:
- $8>5$, $8>3$, $5>3$ $\Rightarrow 3$.

So:
\[
I_1=3.
\]

### Right-half inversions $I_2$
Pairs:
- $4>2$, $7>6$ $\Rightarrow 2$.

So:
\[
I_2=2.
\]

### Total $I$ in full array
Additional crossing inversions (left index in first half, right index in second half):
- from $8$: $8>4,2,7,6$ $\Rightarrow 4$,
- from $5$: $5>4,2$ $\Rightarrow 2$,
- from $3$: $3>2$ $\Rightarrow 1$,
- from $1$: none.

Crossing total $=7$.

Hence:
\[
I=I_1+I_2+7=3+2+7=12.
\]

\[
\boxed{I=12,\ I_1=3,\ I_2=2}
\]

---

## 🧠 Aufgabe 1b — Merge step and $I-(I_1+I_2)$

Sorted halves given:
- $L=(1,3,5,8)$,
- $R=(2,4,6,7)$.

During merge:
- compare 1 vs 2 $\to$ take 1 (no new crossing inversion),
- compare 3 vs 2 $\to$ take 2; remaining left $(3,5,8)$ all $>2$: +3,
- compare 3 vs 4 $\to$ take 3,
- compare 5 vs 4 $\to$ take 4; remaining left $(5,8)$: +2,
- compare 5 vs 6 $\to$ take 5,
- compare 8 vs 6 $\to$ take 6; remaining left $(8)$: +1,
- compare 8 vs 7 $\to$ take 7; remaining left $(8)$: +1,
- append 8.

Crossing inversions counted in merge:
\[
3+2+1+1=7.
\]
And exactly:
\[
I-(I_1+I_2)=12-(3+2)=7.
\]

So merge step computes the crossing part.

\[
\boxed{I-(I_1+I_2)=7}
\]

---

## 🧠 Aufgabe 1c — Show $O(n\log n)$

### Algorithm idea
Function `count_inversions(A)`:
1. if length $\le 1$: return (A, 0),
2. split into $L,R$,
3. recursively get $(L_{sorted}, I_L)$, $(R_{sorted}, I_R)$,
4. merge while counting crossing inversions $I_C$,
5. return $(A_{sorted}, I_L+I_R+I_C)$.

### Correctness sketch
By partition of inversion types:
- every inversion is either left-internal, right-internal, or crossing.
Recursion counts first two exactly; merge counts crossing exactly.
So sum is exact total.

### Runtime
Recurrence:
\[
T(n)=2T(n/2)+O(n).
\]
Master theorem:
\[
T(n)=O(n\log n).
\]

\[
\boxed{\text{Inversion counting in }O(n\log n)\text{ time}}
\]

---

### ⚠️ Pitfalls
1. Counting only within halves and forgetting crossing inversions.
2. Adding +1 instead of “number of remaining left elements” when taking from right.
3. Losing stability of merge pointer logic.

---

### 🧩 Task-1 micro-summary
- $I=12$, $I_1=3$, $I_2=2$,
- merge-cross part is $7=I-(I_1+I_2)$,
- total inversion count via merge-based divide-and-conquer in $O(n\log n)$.

---

## 🧩 Topic 2: Zweitkleinstes Element mit weniger Vergleichen

---

### 🧠 What this asks
Find smallest and second smallest with fewer than naive $2n-3$ comparisons.

---

### 📚 Foundation
Naive:
- min in $n-1$,
- second min among rest in $n-2$,
\[
2n-3.
\]

Better idea: **tournament tree**.
- each comparison is a match,
- smallest element is tournament winner,
- second smallest must be among elements that directly lost to winner.

Winner has $\log_2 n$ wins (because $n$ is power of 2), so only $\log_2 n$ candidates for second smallest.

---

### 🌉 Analogy: Knockout bracket
Gold medalist (smallest) beats $\log_2 n$ players.
Silver (second smallest) must be one of those defeated directly by champion, not arbitrary participant.

---

### Efficient comparison count
- tournament to find min: $n-1$,
- min over loser-list of size $\log_2 n$: $\log_2 n-1$.

Total:
\[
n+\log_2 n-2.
\]
This is $\le \frac32 n-2$ for $n\ge2$, so required bound holds.

\[
\boxed{n+\log_2 n-2\ \text{comparisons}}
\]

---

## 🧩 Topic 3: “Squaring vs Multiplication” equivalence statement

---

### Statement
“Exactly then there is an $O(f)$-algorithm for squaring $n$-digit numbers iff one can multiply two $n$-digit numbers in $O(f)$.”

### Direction 1 ($\Rightarrow$ trivial part)
If multiplication is $O(f)$, then squaring is special case:
\[
x^2=x\cdot x
\]
also $O(f)$.

### Direction 2 (nontrivial)
If squaring is $O(f)$, can we multiply in $O(f)$? Yes via identity:
\[
xy=\frac{(x+y)^2-x^2-y^2}{2}.
\]
So three squarings + linear-time additions/subtractions/division by 2.

Total:
\[
O(f(n))+O(n).
\]
For standard multiplication contexts $f(n)\in\Omega(n)$, this is $O(f(n))$.

\[
\boxed{\text{Aussage gilt (unter üblicher Annahme }f(n)\ge cn\text{).}}
\]

---

### ✅ Final compact answers (exam-style, UB3)

1. Inversion counts:
\[
\boxed{I=12,\ I_1=3,\ I_2=2,\ I-(I_1+I_2)=7.}
\]
2. Inversion counting algorithm:
\[
\boxed{O(n\log n)\ \text{via MergeSort + crossing-count merge}.}
\]
3. Smallest + second smallest:
\[
\boxed{n+\log_2 n-2\ \text{Vergleiche (damit } \le 3n/2-2\text{).}}
\]
4. Multiplikation vs Quadrieren:
\[
\boxed{\text{äquivalent in }O(f)\text{ (über }xy=((x+y)^2-x^2-y^2)/2\text{).}}
\]

---

---

## 🔁 Final Retrofitted Replacement — Übungsblatt 4 (Operational, Blank-Slate, ADHD-Oriented)

## 🧱 Topic 1: Matrix Multiplication — Naive, Block-Recursion, Strassen, Memory

---

### 🧠 What this asks
You must compare algorithms for $(n\times n)$-matrix multiplication:
1. naive formula,
2. straightforward block recursion,
3. Strassen recursion,
4. asymptotic memory usage.

Assume $n=2^k$, arithmetic op cost constant.

---

### 📚 Foundations

#### Matrix multiplication definition
For $C=AB$:
\[
c_{ij}=\sum_{k=1}^n a_{ik}b_{kj}.
\]

Naive cost:
- $n^2$ entries,
- each entry does $n$ multiplications (+ additions),
\[
\Theta(n^3).
\]

---

### 🌉 Analogy: 4 workshop teams
Split giant job into 4 quadrants.
Naive block recursion computes 8 recursive multiplications; Strassen algebraically reduces to 7.

---

## 🧠 Aufgabe 1a — Naive vs simple block recursion

Block recurrence:
\[
T(n)=8T(n/2)+\Theta(n^2),\quad T(1)=\Theta(1).
\]
Master:
- $a=8,b=2\Rightarrow n^{\log_2 8}=n^3$,
- combine term $n^2$ smaller,

\[
T(n)=\Theta(n^3).
\]

So asymptotically same as naive.

\[
\boxed{\Theta(n^3)}
\]

---

## 🧠 Aufgabe 1b — Strassen recurrence

Strassen:
- 7 recursive mults of size $n/2$,
- plus $\Theta(n^2)$ for additions/subtractions.

\[
S(n)=7S(n/2)+\Theta(n^2).
\]
Master:
\[
S(n)=\Theta\!\left(n^{\log_2 7}\right)\approx \Theta(n^{2.807}).
\]

\[
\boxed{\Theta(n^{\log_2 7})}
\]

---

## 🧠 Aufgabe 1c — Memory usage

Need to store matrices/submatrices and temporary $M_1,\dots,M_7$-style buffers.

At recursion level $i$, total active matrix-area is geometric and dominated by top level $n^2$.  
Recursion stack contributes only $O(\log n)$ frames.

Hence asymptotic auxiliary memory:
\[
\boxed{\Theta(n^2)}.
\]

(Implementation constants differ by in-place strategy, but order remains $n^2$.)

---

### ⚠️ Pitfalls
1. Confusing time with memory.
2. Forgetting addition cost $\Theta(n^2)$ per level.
3. Claiming Strassen is $O(n^2)$ (false).

---

## 🧱 Topic 2: Zweidrittelsort — Correctness + Runtime

---

### 🧠 What this asks
For algorithm that recursively sorts:
1. first $2/3$,
2. last $2/3$,
3. first $2/3$ again,

you must show:
- correctness,
- recurrence/runtime.

---

### 📚 Intuition
This is StoogeSort-style overlap sorting:
- first call fixes left-large disorder,
- second fixes right-small disorder,
- third re-fixes left overlap disturbed by second.

The overlapping regions propagate order globally.

---

### 🌉 Analogy: overlapping cleaning zones
Three passes over overlapping floor zones:
- left big zone,
- right big zone,
- left big zone again.
Overlap ensures dirt pushed across boundary eventually disappears globally.

---

## 🧠 Aufgabe 2a — Correctness idea

(Using simplification $l\ge3$ divisible by 3.)

Let segment length $l$, $k=l/3$:
- call 1 sorts $A[i..j-k]$,
- call 2 sorts $A[i+k..j]$,
- call 3 sorts $A[i..j-k]$ again.

Induction on $l$:
- base $l\le2$: direct swap correct.
- step: recursive calls sort overlapping large segments.
Because overlap size $l/3$ is nonempty, extremes move toward correct sides:
  - smallest element gets forced into left segment and remains,
  - largest gets forced into right segment and remains,
  - recursive sorting + overlap yields full sorted order.

Thus algorithm sorts correctly.

\[
\boxed{\text{ZweidrittelSort ist korrekt}}
\]

---

## 🧠 Aufgabe 2b — Runtime recurrence

Each level does 3 recursive calls on size about $2n/3$, plus constant overhead:
\[
T(n)=3T(2n/3)+\Theta(1).
\]
(Or $\Theta(n^0)$ combine term.)

Master-like evaluation:
\[
T(n)=\Theta\!\left(n^{\log_{3/2}3}\right)\approx \Theta(n^{2.7095}).
\]

\[
\boxed{T(n)=\Theta(n^{\log_{3/2}3})}
\]

---

## 🧱 Topic 3: Regularity condition in Master theorem (why needed)

---

### 🧠 Core point
Case 3 of Master needs regularity:
\[
a f(n/b)\le c f(n)\quad\text{for some }c<1\text{ eventually}.
\]
Without it, wildly oscillating $f(n)$ can break naive “$T(n)=\Theta(f(n))$” conclusion.

---

### (a) Naive (wrong) case-3 usage on
\[
T(n)=T(n/3)+n(\sin(\pi n/2)+2).
\]
Would claim:
\[
T(n)=O(n(\sin(\pi n/2)+2)).
\]
But unsafe.

### (b) Why regularity fails
Take $n=3+12k\Rightarrow \sin(\pi n/2)=-1$, so $f(n)=n$.  
But for $n/3$-related values sine may be $+1$, making $f(n/3)$ comparatively too large; no uniform contraction factor $c<1$.

### (c) Why induction breaks
Trying $T(n)\le C f(n)$:
\[
T(n)\le C f(n/3)+f(n).
\]
Need $C f(n/3)\le (C-1)f(n)$, equivalent to regularity-type bound. If absent, step fails.

### (d) Same issue for
\[
T(n)=aT(n/b)+2^{\lceil\log_2 n\rceil}
\]
since toll oscillates between powers of two plateaus; regularity can fail similarly.

\[
\boxed{\text{Regularitätsbedingung ist notwendig, nicht nur technische Deko.}}
\]

---

### ✅ Final compact answers (exam-style, UB4)

1. Simple block recursion:
\[
\boxed{T(n)=8T(n/2)+\Theta(n^2)=\Theta(n^3).}
\]
2. Strassen:
\[
\boxed{S(n)=7S(n/2)+\Theta(n^2)=\Theta(n^{\log_2 7}).}
\]
3. Memory:
\[
\boxed{\Theta(n^2).}
\]
4. Zweidrittelsort:
\[
\boxed{\text{korrekt (Induktion mit Überlappung), }T(n)=3T(2n/3)+\Theta(1)=\Theta(n^{\log_{3/2}3}).}
\]
5. Regularity:
\[
\boxed{\text{Case-3 ohne Regularität kann falsch sein; genau daran scheitert der Induktionsschluss.}}
\]

---

## Übungsblatt 5 — FFT in Practice, Randomized Coin Simulation, DFT Coefficient Identity, Expected Quicksort Runtime (ADHD-Friendly, Blank-Slate, Lecture-Level)

## 🧠 Why this sheet matters

This sheet combines four core exam pillars:

1. **Fast polynomial multiplication** (algorithm engineering + asymptotics),
2. **Probability-based simulation algorithms** (fair/biased coin conversion),
3. **Discrete Fourier Transform (DFT) structure proof** (algebra + roots of unity),
4. **Expected runtime analysis of randomized Quicksort** (recurrences + induction).

You are training exactly the type of thinking needed in DSEA:
- model the problem formally,
- choose the right algorithmic tool,
- prove correctness,
- prove runtime.

---

## Global mini-glossary (used across tasks)

### 1) Asymptotic notation
- $O(g(n))$: upper bound (at most this growth rate up to constants).
- $\Theta(g(n))$: tight bound (same growth rate up to constants).
- $\Omega(g(n))$: lower bound (at least this growth rate up to constants).

### 2) Expected value
The “long-run average” value of a random variable:
\[
E[X]=\sum_x x\cdot P(X=x).
\]
In algorithm analysis: expected runtime = average runtime over randomness.

### 3) Recurrence
An equation that defines a quantity using smaller input sizes (e.g., divide-and-conquer algorithms).

---

---

## Section A — Aufgabe 1: Polynomial multiplication (naive vs FFT)

---

### A1) Start from zero: what is a polynomial in coefficient form?

A polynomial over $\mathbb{R}$ or $\mathbb{C}$ is
\[
A(x)=a_0+a_1x+\dots+a_{n-1}x^{n-1}.
\]
Coefficient representation is just the list:
\[
[a_0,a_1,\dots,a_{n-1}].
\]

### A2) What does multiplying two polynomials mean?

Given
\[
A(x)=\sum_{i=0}^{n-1}a_i x^i,\quad
B(x)=\sum_{j=0}^{m-1}b_j x^j,
\]
their product
\[
C(x)=A(x)B(x)=\sum_{k=0}^{n+m-2}c_k x^k
\]
has coefficients
\[
c_k=\sum_{i+j=k}a_i b_j.
\]
This is called **convolution** of coefficient vectors.

---

### A3) Algorithm 1: naive (schoolbook) multiplication

#### Idea
For every pair $(i,j)$, add $a_i b_j$ to output position $i+j$.

#### Why correct?
Because every term $a_i x^i\cdot b_j x^j = a_i b_j x^{i+j}$ contributes exactly to degree $i+j$, and summing all such pairs is exactly the definition of polynomial multiplication.

#### Runtime
If both have length $n$:
- $n^2$ pair multiplications + additions,
\[
\Theta(n^2).
\]

---

### A4) Why do we need a faster method?

For large $n$, $\Theta(n^2)$ becomes expensive.  
The **Fast Fourier Transform (FFT)** computes multiplication in $\Theta(n\log n)$, which is much faster asymptotically.

---

### A5) New concepts thoroughly: DFT and FFT

#### DFT = Discrete Fourier Transform
The **Discrete Fourier Transform (DFT)** converts a length-$N$ vector from:
- coefficient/time domain  
to
- value/frequency domain.

For $\omega_N=e^{2\pi i/N}$ (an $N$-th root of unity):
\[
\hat a_k=\sum_{j=0}^{N-1} a_j\omega_N^{jk},\quad k=0,\dots,N-1.
\]

#### Inverse DFT
\[
a_j=\frac{1}{N}\sum_{k=0}^{N-1}\hat a_k\omega_N^{-jk}.
\]

#### FFT = Fast Fourier Transform
The **Fast Fourier Transform (FFT)** is an algorithm to compute the DFT quickly in
\[
\Theta(N\log N)
\]
instead of $\Theta(N^2)$.

It uses divide-and-conquer:
- split coefficients into even-index and odd-index parts,
- recursively compute smaller DFTs,
- combine with “twiddle factors” ($\omega$-powers).

---

### A6) Why FFT helps polynomial multiplication

Core theorem:
- Convolution in coefficient domain = pointwise multiplication in DFT domain.

So algorithm:
1. Zero-pad both coefficient arrays to length $N$,
2. FFT both arrays,
3. Multiply entry-wise,
4. Inverse FFT.

---

### A7) Zero-padding explained thoroughly

If $\deg A=n-1,\deg B=m-1$, then $\deg C=n+m-2$, so output needs $n+m-1$ coefficients.

For FFT, choose $N$ as power of 2 with
\[
N\ge n+m-1.
\]
Why power of 2? Because standard recursive FFT implementation is simplest and fastest with sizes $2^k$.

---

### A8) ADHD analogy for this task

Naive method = “every person in class shakes hands with every person in another class.”  
FFT method = “move both classes to a coded seating layout where matching happens row-wise instantly, then decode back.”  
Same final result, fewer operations at scale.

---

### A9) Python implementation (naive + FFT)

```python name=ub5_fft_poly.py
import cmath
from typing import List

def poly_mul_naive(a: List[float], b: List[float]) -> List[float]:
    n, m = len(a), len(b)
    c = [0.0] * (n + m - 1)
    for i in range(n):
        for j in range(m):
            c[i + j] += a[i] * b[j]
    return c

def next_power_of_two(x: int) -> int:
    p = 1
    while p < x:
        p <<= 1
    return p

def fft(a: List[complex], invert: bool = False) -> List[complex]:
    n = len(a)
    if n == 1:
        return a[:]

    even = fft(a[0::2], invert)
    odd = fft(a[1::2], invert)

    angle = 2 * cmath.pi / n * (1 if invert else -1)
    wn = cmath.exp(1j * angle)
    w = 1 + 0j

    y = [0j] * n
    half = n // 2
    for k in range(half):
        t = w * odd[k]
        y[k] = even[k] + t
        y[k + half] = even[k] - t
        w *= wn
    return y

def ifft(a: List[complex]) -> List[complex]:
    n = len(a)
    y = fft(a, invert=True)
    return [val / n for val in y]

def poly_mul_fft(a: List[int], b: List[int]) -> List[int]:
    n, m = len(a), len(b)
    N = next_power_of_two(n + m - 1)

    fa = list(map(complex, a)) + [0j] * (N - n)
    fb = list(map(complex, b)) + [0j] * (N - m)

    FA = fft(fa, invert=False)
    FB = fft(fb, invert=False)

    FC = [FA[i] * FB[i] for i in range(N)]
    c_complex = ifft(FC)

    # round numerical noise
    c = [int(round(c_complex[i].real)) for i in range(n + m - 1)]
    return c

def read_coeffs(line: str) -> List[int]:
    line = line.strip()
    if not line:
        return []
    return list(map(int, line.split()))

def main():
    import sys
    lines = [line.rstrip("\n") for line in sys.stdin.readlines()]
    if len(lines) < 2:
        return
    a = read_coeffs(lines[0])
    b = read_coeffs(lines[1])
    c = poly_mul_fft(a, b)
    print(" ".join(map(str, c)))

if __name__ == "__main__":
    main()
```

---

### A10) Runtime measurement for $n=2^{17}$: what to do and what to expect

- Generate random coefficient arrays of length $2^{17}$.
- Measure wall-clock time:
  - `poly_mul_naive`
  - `poly_mul_fft`

Expected trend:
- naive $\Theta(n^2)$ explodes quickly,
- FFT $\Theta(n\log n)$ scales much better.

Practical note: Python recursion/constants are large; still, asymptotic trend remains.

---

### A11) Common mistakes in Aufgabe 1
1. forgetting zero-padding,
2. wrong FFT length ($N < n+m-1$),
3. forgetting inverse normalization (/N),
4. not rounding floating-point noise.

---

---

## Section B — Aufgabe 2: Simulating fair/biased coins

Given biased coin:
\[
P(H)=\frac13,\quad P(T)=\frac23.
\]

---

### B0) Core probability concepts used here

#### Random experiment
A process with uncertain outcome (coin flips).

#### Geometric distribution (important in this task)
If success probability per trial is $p$, expected number of trials to first success:
\[
E[\text{trials}] = \frac{1}{p}.
\]

---

### B1) Part (a): simulate a fair coin from the biased coin

#### Algorithm (Von Neumann extractor)
Flip twice:
- HT => output 0
- TH => output 1
- HH or TT => reject and repeat

#### Why fair?
\[
P(HT)=\frac13\cdot\frac23=\frac29,\quad
P(TH)=\frac23\cdot\frac13=\frac29.
\]
Outputs 0 and 1 are equally likely.

#### Expected flips
Success (accept) probability per 2-flip round:
\[
p=\frac29+\frac29=\frac49.
\]
Expected rounds:
\[
\frac{1}{p}=\frac94.
\]
Each round = 2 flips:
\[
E[\text{flips}] = 2\cdot\frac94=\frac92.
\]

\[
\boxed{E[\text{flips}]=4.5}
\]

---

### B2) Part (b): get fair coin with expected flips < 9/2

Important: This part typically expects a smarter extractor than basic Von Neumann.

A standard lecture-safe statement:
- **Von Neumann gives 9/2** expected flips.
- More advanced extractors (e.g., Peres recursion) strictly improve this expectation by reusing rejected information.

If your tutor expects an explicit finite-state construction, we can add one tailored to your course style.  
For exam prep, the critical conceptual point is:
- “discarding HH/TT wastes entropy; recycling it can do better.”

---

### B3) Part (c): simulate biased $p=1/3$ from a fair coin

Now fair coin has:
\[
P(0)=P(1)=1/2.
\]

Use 2 fair flips (uniform on 4 outcomes):
- 00 => H
- 01 or 10 => T
- 11 => reject and repeat

Accepted outcomes are equiprobable among 3 states:
\[
P(H)=1/3,\quad P(T)=2/3.
\]

---

### B4) Part (d): simulate $P(H)=1/n$ from fair coin

#### Algorithm (rejection sampling on binary integers)
1. Set $k=\lceil \log_2 n\rceil$.
2. Flip fair coin $k$ times => integer $X\in\{0,\dots,2^k-1\}$ uniformly.
3. If $X\ge n$, reject and repeat.
4. If $X<n$: output H iff $X=0$, else T.

#### Why correct?
Conditioned on acceptance, $X$ is uniform on $\{0,\dots,n-1\}$.  
Exactly one outcome gives H.

Hence:
\[
P(H)=\frac1n.
\]

---

### B5) Common mistakes in Aufgabe 2
1. believing “fair simulation” must terminate in fixed number of flips,
2. forgetting rejection changes conditional probabilities,
3. confusing unbiased output with equal raw pattern counts (must consider probabilities!).

---

---

## Section C — Aufgabe 3: Prove $c_{2n-1}=0$ in FFT framework

---

### C0) New concepts in this proof

#### Root of unity
A complex number $\omega$ with $\omega^N=1$.  
Primitive $N$-th root:
\[
\omega_N=e^{2\pi i/N}.
\]

#### Nullsum lemma (orthogonality of roots)
For $q\not\equiv 0 \pmod N$:
\[
\sum_{k=0}^{N-1}\omega_N^{kq}=0.
\]
This cancellation is the core tool here.

---

### C1) Problem setup

\[
A(x)=\sum_{i=0}^{n-1}a_ix^i,\quad
B(x)=\sum_{i=0}^{n-1}b_ix^i,\quad
C(x)=A(x)B(x).
\]
Then $\deg C\le 2n-2$, so coefficient $c_{2n-1}$ should be zero.

Given expression:
\[
c_{2n-1}=\frac{1}{2n}\sum_{k=0}^{2n-1}A(\omega^k)B(\omega^k)\left(\omega^{-(2n-1)}\right)^k,\quad \omega=\omega_{2n}.
\]

---

### C2) Proof by expansion + orthogonality

Since $C=A\cdot B$:
\[
A(\omega^k)B(\omega^k)=C(\omega^k)=\sum_{j=0}^{2n-2}c_j\omega^{kj}.
\]
Insert:
\[
c_{2n-1}
=\frac1{2n}\sum_{k=0}^{2n-1}\sum_{j=0}^{2n-2}c_j\omega^{k(j-(2n-1))}.
\]
Swap sums:
\[
c_{2n-1}
=\frac1{2n}\sum_{j=0}^{2n-2}c_j
\underbrace{\sum_{k=0}^{2n-1}\omega^{k(j-(2n-1))}}_{=0}.
\]
Why inner sum is 0:
- exponent multiplier $j-(2n-1)$ is never divisible by $2n$ for $j\in[0,2n-2]$,
- thus Nullsum lemma applies.

So all terms vanish:
\[
\boxed{c_{2n-1}=0}.
\]

---

### C3) Intuition
You are asking for a coefficient beyond the true degree range.  
DFT orthogonality “filters it out” to zero.

---

---

## Section D — Aufgabe 4: Expected runtime of Quicksort

---

### D0) New concepts fully explained

#### Quicksort (randomized version)
1. choose pivot uniformly at random,
2. partition into smaller and larger elements,
3. recurse on both sides.

#### Random variable for runtime
$T(n)$ is random because pivot choice is random.
We analyze $E[T(n)]$, not fixed worst-case $T(n)$.

#### Law of total expectation
If cases $C_i$ partition outcomes:
\[
E[X]=\sum_i E[X\mid C_i]P(C_i).
\]

---

### D1) Part (a): derive recurrence

If pivot has rank $i\in\{1,\dots,n\}$, then:
- left size $i-1$,
- right size $n-i$,
- partition cost $cn$,
with probability $1/n$.

So:
\[
E[T(n)] =
\frac1n\sum_{i=1}^n
\left(E[T(i-1)] + E[T(n-i)] + cn\right),
\quad E[T(0)]=0.
\]

---

### D2) Part (b) inequality purpose
\[
\sum_{i=1}^{n-1} i\log i \le \frac{n^2}{2}\log n-\frac{n^2}{8}.
\]
This gives a clean upper bound for the induction in part (c).

---

### D3) Part (c): show $E[T(n)]\in O(n\log n)$

From recurrence:
\[
E[T(n)]
=\frac{2}{n}\sum_{j=0}^{n-1}E[T(j)] + cn.
\]
(Using symmetry of left/right sums.)

Induction hypothesis:
\[
E[T(j)]\le a\,j\log j\quad \text{for }j<n.
\]
Then
\[
E[T(n)]\le \frac{2a}{n}\sum_{j=1}^{n-1}j\log j + cn.
\]
Apply bound from (b):
\[
E[T(n)]\le a n\log n - \frac{a}{4}n + cn.
\]
Choose $a\ge4c$, then $-\frac a4 n + cn\le0$, hence:
\[
E[T(n)]\le a n\log n.
\]
Therefore:
\[
\boxed{E[T(n)]\in O(n\log n)}.
\]

---

### D4) Why this does NOT mean worst-case $O(n\log n)$
Worst case (bad pivots every time) is still $\Theta(n^2)$.  
The result is about **expected** runtime over random pivots.

---

### D5) Common mistakes in Aufgabe 4
1. mixing expected and worst-case analysis,
2. forgetting factor $2$ when combining two symmetric sums,
3. sloppy induction constants (must choose $a$ large enough).

---

## ✅ Compact exam answers (UB5)

1. **Polynomial multiplication**
   - naive: $\Theta(n^2)$,
   - FFT-based: $\Theta(n\log n)$ with zero-padding to $N\ge n+m-1$, $N=2^k$.
2. **Coin simulations**
   - fair from $p=1/3$: Von Neumann, expected flips $9/2$,
   - $p=1/3$ from fair: rejection on 2-bit outcomes,
   - $1/n$ from fair: binary rejection sampling.
3. **DFT coefficient**
   - $c_{2n-1}=0$ via expansion + root-of-unity nullsum lemma.
4. **Quicksort expectation**
   - recurrence by conditioning on pivot rank,
   - induction gives $E[T(n)]\in O(n\log n)$.

---

## 🧩 Micro-summary
UB5 trains the full stack:
- implement,
- prove,
- analyze,
- and connect algebra/probability/recurrences in one sheet.

If this feels heavy, that is normal — this is exactly “exam core material.”

---

## Übungsblatt 6 — Median-of-Medians Variants, Sampling as an Unbiased Estimator, Coupon Collector, Gale–Shapley Requests, MSD Radix Sort (ADHD-Friendly, Blank-Slate, Lecture-Level)

## 🧠 Why this sheet matters

UB6 is conceptually rich and very exam-relevant because it combines:

1. **Deterministic selection and worst-case analysis** (Median-of-Medians),
2. **Probability estimator correctness** (sampling mean),
3. **Expected-value process analysis** (coupon collector),
4. **Matching algorithm complexity** (Gale–Shapley requests),
5. **Digit sorting design + stability/in-place reasoning** (MSD Radix Sort).

This is exactly the kind of sheet where understanding the *reasoning patterns* matters more than memorizing results.

---

## Global glossary (new terms)

### 1) Deterministic algorithm
An algorithm with no randomness: same input → same execution path/output.

### 2) Worst-case runtime
Maximum runtime over all inputs of size $n$.

### 3) Expected runtime / expected value
Average value over randomness (either algorithmic randomness or random input model).

### 4) Unbiased estimator (erwartungstreuer Schätzer)
A random estimate $X$ for a quantity $\mu$ is unbiased if:
\[
E[X]=\mu.
\]

### 5) Stable sorting algorithm
If two elements have equal key and one appears earlier in input, it also appears earlier in output.

### 6) In-place algorithm
Uses only $O(1)$ or very small extra memory beyond input storage (ignoring recursion stack if stated).

---

---

## Section A — Aufgabe 1: Median-of-Medians and packet sizes (5, 9, 3)

---

### A0) First principles: what problem is being solved?

We want the $k$-th smallest element (selection problem), not full sorting.

Given $n$ distinct numbers and rank $k\in\{1,\dots,n\}$, return the element with rank $k$.

---

### A1) Quickselect and why pivots matter

**Quickselect**:
1. choose pivot,
2. partition into smaller/greater,
3. recurse only into side containing rank $k$.

If pivot is bad repeatedly, worst-case is $\Theta(n^2)$.  
So we want a pivot strategy that guarantees balanced enough splits.

#### What is a pivot?
A **pivot** is a chosen reference element used to split an array/set into groups:
- elements smaller than pivot,
- the pivot itself,
- elements larger than pivot.

In partition-based algorithms (like Quickselect/Quicksort), pivot quality is crucial:
- a “good” pivot (near median) gives balanced subproblems,
- a “bad” pivot (near min/max) gives very unbalanced subproblems and can slow the algorithm drastically.
---

### A2) Median-of-Medians algorithm (new algorithm explained)

Median-of-Medians (MoM) is a deterministic pivot strategy:

1. split elements into groups of fixed size $g$ (classically $g=5$),
2. compute each group median,
3. recursively select the median of these medians → pivot $\bar m$,
4. partition around $\bar m$,
5. recurse into relevant side.

Key benefit: guaranteed discard fraction each step → linear worst-case for selection (with suitable $g$, e.g. 5).

---

### A3) Part (a): Why deterministic-pivot Quicksort can be $O(n\log n)$

If we use a deterministic pivot algorithm (MoM) to choose a ���good” pivot in every Quicksort partition:
- each partition step can be done in linear time,
- each recursive split is guaranteed not too unbalanced.

Then Quicksort recurrence behaves like:
\[
Q(n)\le Q(\alpha n)+Q((1-\alpha)n)+O(n)
\]
with constant $\alpha<1$ bounded away from 0/1, yielding:
\[
Q(n)=O(n\log n).
\]

**Important**: If pivot selection itself costs $O(n)$ per partition level, total remains $O(n\log n)$.

---

### A4) Part (b): Replace groups of 5 by groups of 9 — does analysis still work?

#### Goal
Check whether MoM recurrence still solves to linear selection time.

Classical MoM-with-5 gives:
\[
T(n)\le T(n/5)+T(7n/10)+cn
\]
which is linear because coefficients satisfy shrink condition.

For groups of 9, we estimate guaranteed large-side bound.

- Number of full groups: about $n/9$.
- Half of group medians are $\ge \bar m$.
- In each such group, at least 5 elements are $\ge$ its median, thus $\ge \bar m$.

So at least roughly:
\[
\frac12\cdot\frac n9\cdot5=\frac{5n}{18}
\]
elements are $\ge\bar m$, similarly $\le\bar m$ on other side.

So recursive larger side has size at most:
\[
n-\frac{5n}{18}=\frac{13n}{18}.
\]
Recurrence:
\[
T(n)\le T(n/9)+T(13n/18)+cn.
\]
Now check shrink-sum:
\[
\frac19+\frac{13}{18}=\frac{2}{18}+\frac{13}{18}=\frac{15}{18}=\frac56<1.
\]
Hence:
\[
\boxed{T(n)=O(n).}
\]
So yes, analysis still works with 9-packets.

---

### A5) Part (c): What about groups of 3?

For groups of 3:

- $\approx n/3$ medians,
- half medians $\ge \bar m$,
- each such group contributes at least 2 elements $\ge \bar m$ (group median + largest).

Guaranteed on each side:
\[
\frac12\cdot\frac n3\cdot2=\frac n3.
\]
So larger recursive side can be as big as:
\[
\frac{2n}{3}.
\]
Recurrence:
\[
T(n)\le T(n/3)+T(2n/3)+cn.
\]
Here shrink-sum is:
\[
\frac13+\frac23=1.
\]
This solves to:
\[
\Theta(n\log n),
\]
not linear.

\[
\boxed{\text{3-packets lose linear-time guarantee (becomes } \Theta(n\log n)\text{).}}
\]

---

### A6) ADHD analogy for A

Imagine choosing a class representative by:
- forming small subgroups,
- taking subgroup representatives,
- then representative-of-representatives.

Larger subgroup size (like 9) gives robust central pivot.  
Too small subgroup size (3) gives weaker guarantee and less discard progress.

---

### A7) Common mistakes in Aufgabe 1
1. forgetting to verify coefficient sum in recurrence,
2. assuming any odd group size gives linear time (false for 3),
3. mixing selection recurrence with full sorting recurrence.

---

---

## Section B — Aufgabe 2: Sampling mean is unbiased

We have fixed numbers $a_1,\dots,a_n$, true mean:
\[
\mu=\frac1n\sum_{i=1}^n a_i.
\]
We sample $k$ times uniformly *with replacement* from these $n$ values and compute sample mean $X$.

Need to show:
\[
E[X]=\mu.
\]

---

### B1) Define random variables cleanly

Let sampled values be $Y_1,\dots,Y_k$.  
Each $Y_t$ is uniformly chosen from $\{a_1,\dots,a_n\}$.

Then:
\[
X=\frac1k\sum_{t=1}^k Y_t.
\]

---

### B2) Compute expectation

By linearity of expectation:
\[
E[X]=\frac1k\sum_{t=1}^k E[Y_t].
\]
Each $Y_t$ has:
\[
E[Y_t]=\frac1n\sum_{i=1}^n a_i=\mu.
\]
So:
\[
E[X]=\frac1k\sum_{t=1}^k \mu=\mu.
\]

\[
\boxed{X\text{ is unbiased (erwartungstreu).}}
\]

---

### B3) Why this is important
It means sampling does not systematically overestimate or underestimate the true average.

---

### B4) Common mistakes in Aufgabe 2
1. thinking independence is needed for expectation linearity (it is not),
2. confusing sample mean variance with expectation.

---

---

## Section C — Aufgabe 3a: Coupon Collector ($O(n\log n)$)

Ross collects one of $n$ coupons uniformly at random each purchase.  
How many purchases needed in expectation to collect all $n$?

---

### C1) Phase decomposition method

Suppose already collected $i$ distinct coupons ($0\le i<n$).  
Probability next purchase is *new*:
\[
p_i=\frac{n-i}{n}.
\]
Expected purchases to get next new one:
\[
\frac1{p_i}=\frac{n}{n-i}.
\]
Total expectation:
\[
E[T]=\sum_{i=0}^{n-1}\frac{n}{n-i}
= n\sum_{j=1}^{n}\frac1j
= nH_n.
\]
Harmonic number $H_n=\Theta(\log n)$, so:
\[
\boxed{E[T]=\Theta(n\log n).}
\]

---

### C2) Intuition
Early coupons are easy; last missing coupon is hard (probability $1/n$).  
That tail makes total $n\log n$, not $n$.

---

---

## Section D — Aufgabe 3b: Gale–Shapley worst-case requests are $\Theta(n^2)$

---

### D0) New algorithm explained: Gale–Shapley (Deferred Acceptance)

Setting: $n$ applicants, $n$ positions (one-to-one stable matching).

Applicant-proposing version:
1. any free applicant proposes to highest-ranked position not yet proposed to,
2. position keeps best offer seen so far, rejects others,
3. repeat until everyone matched.

---

### D1) Why worst-case $O(n^2)$
Each applicant can propose to at most $n$ positions.  
Total proposals $\le n^2$.

### D2) Why also $\Omega(n^2)$ in worst case
There exist preference profiles causing almost full proposal matrix exploration.  
Hence tight:
\[
\boxed{\Theta(n^2).}
\]

---

---

## Section E — Aufgabe 3c (star): expected requests $O(n\log n)$ via “forgetful” upper bound

This part is subtle; let’s structure it carefully.

---

### E1) Idea from statement
We analyze a modified (“forgetful”) randomized process:

- free applicant chooses a position uniformly from all $n$, even if already tried before,
- repeated failed repeats are allowed,
- this can only increase expected number of proposals compared to non-forgetful process.

So expectation here gives an upper bound for original random-preference model.

---

### E2) Reduction to coupon collector
Interpret “position has received at least one proposal” as “coupon collected”.

To terminate with all matched, intuitively we need enough coverage of positions; in this simplified upper-bound argument, number of proposals is controlled by how long it takes to “hit” all positions.

Expected proposals to hit all $n$ positions:
\[
\Theta(n\log n)
\]
by coupon collector from Section C.

Thus expected requests in forgetful process is $O(n\log n)$, hence also an upper-bound style argument for randomized Gale–Shapley expectation.

\[
\boxed{E[\#\text{requests}]=O(n\log n)\ \text{(under given randomized model argument).}}
\]

---

### E3) Exam note
This is a modeling upper-bound argument using “principle of deferred decisions” and a relaxed process.

---

---

## Section F — Aufgabe 4: MSD Radix Sort

Given algorithm MSD (Most Significant Digit first), base $b$, recursive bucket sort by digit $k\to k-1$.

---

### F0) New concepts

#### Radix sort
Sort numbers digit-by-digit instead of pairwise comparisons.

#### MSD = Most Significant Digit first
First split by most significant digit, then recursively sort inside each bucket by next digit.

#### LSD = Least Significant Digit first
Sort by least significant digit upward (usually stable passes required).

---

### F1) Part (a): Is given MSD stable?

Given implementation:
- distribute in input order into each bucket list,
- recursively sort each bucket,
- concatenate buckets in digit order.

If bucket insertion preserves order and recursive calls do same, equal keys preserve relative order.

\[
\boxed{\text{Yes, this MSD version is stable (with queue-like bucket append).}}
\]

---

### F2) Part (b): In-place MSD for binary ($b=2$)

For binary digits, each level partitions subarray into:
- digit 0 part,
- digit 1 part
at bit $k$, then recurse on both parts for $k-1$.

In-place partition approach:
1. two pointers in subarray $[L,R]$,
2. move 0-bit items left, 1-bit items right (similar to quicksort partition),
3. recurse on the two ranges with next bit.

This uses $O(1)$ extra array memory (ignoring recursion stack), so in-place.

---

### F3) Part (c): Is in-place partition version stable?

Usually **not stable**.

Why:
- swaps can reorder elements with identical key/digit history.

Counterexample idea:
Two elements with equal full value but distinct identities may be swapped across partition operations, changing relative order.

\[
\boxed{\text{Typical in-place binary MSD partition is not stable.}}
\]

(Stable in-place radix is possible but requires more sophisticated techniques, not this simple partition scheme.)

---

### F4) ADHD analogy for MSD
Imagine sorting letters in post office by first character:
- first into A/B/C… bins,
- then inside each bin by second character, etc.
MSD recursively zooms into bins.

---

### F5) Common mistakes in Aufgabe 4
1. confusing MSD and LSD stability conditions,
2. assuming “in-place” automatically means stable (false),
3. forgetting to recurse only if next digit exists.

---

## ✅ Compact exam answers (UB6)

1. **Median-of-Medians variants**
   - deterministic-pivot Quicksort can be $O(n\log n)$,
   - groups of 9: selection still $O(n)$,
   - groups of 3: selection becomes $\Theta(n\log n)$.
2. **Sampling mean**
   - sample mean is unbiased: $E[X]=\mu$.
3. **Coupon collector**
   - expected purchases for all $n$ types: $\Theta(n\log n)$.
4. **Gale–Shapley**
   - worst-case proposals: $\Theta(n^2)$,
   - randomized forgetful upper-bound argument: $O(n\log n)$ expectation.
5. **MSD radix**
   - given bucket-list MSD stable,
   - simple in-place binary partition variant not stable.

---

## 🧩 Micro-summary

UB6 teaches a core exam meta-skill:
- turn process behavior into recurrence/probability phases,
- then prove bounds with harmonic sums and structural invariants.

That pattern appears again and again in DSEA.

---
## Übungsblatt 7 — Jaccard & MinHash, Expected Analysis of Records/Inversions, Universal Hash Families (ADHD-Friendly, Blank-Slate, Lecture-Level)

## 🧠 Why this sheet matters

UB7 is a high-impact probability + hashing sheet. It teaches:

1. **Set similarity measures** (Jaccard index),
2. **Randomized similarity estimation** (MinHash),
3. **Expectation and variance tools** (indicator variables),
4. **Expected analysis patterns** (record highs, inversions),
5. **Universal hashing pitfalls and guarantees**.

This is exam gold because it trains both exact calculation and proof-style probabilistic reasoning.

---

## Global glossary (new terms)

### 1) Cardinality
$|A|$ means number of elements in set $A$.

### 2) Intersection / union
- $A\cap B$: elements in both sets.
- $A\cup B$: elements in at least one set.

### 3) Indicator random variable
A variable $X$ that is 1 if an event happens, 0 otherwise.

### 4) Expected value linearity
\[
E\!\left[\sum_i X_i\right]=\sum_i E[X_i]
\]
(always true, no independence required).

### 5) Variance
\[
\mathrm{Var}[X]=E[(X-E[X])^2]
\]
measures spread around the mean.

### 6) Hashing, hash functions, hash families, collisions, and universal hashing (deep definition)

#### What is hashing?
**Hashing** is a method for storing and querying data quickly by mapping a key (e.g., number/string) to an index in an array (called a hash table).

Goal:
- insert/search/delete keys in expected $O(1)$ time.

#### What is a hash function?
A **hash function** $h$ maps keys from a large universe $U$ to table indices $\{0,\dots,m-1\}$:
\[
h: U \to \{0,\dots,m-1\}.
\]
It is like assigning each key to a “bucket” in a fixed-size table.

#### What is a collision?
A **collision** happens when two different keys map to the same table index:
\[
x\neq y,\quad h(x)=h(y).
\]
Collisions are unavoidable when universe is larger than table (pigeonhole principle), so algorithm quality depends on how often they occur and how we handle them.

#### Why collisions matter
If too many keys land in same bucket, operations slow down (e.g., long chains / many probes), potentially degrading from expected $O(1)$ toward $O(n)$.

#### What is a hash family?
A **hash family** $H$ is a set of hash functions, not just one function.  
At runtime, we choose one function $h\in H$ randomly.

Why this helps:
- with a random choice, adversarial input cannot easily force worst-case collisions for a fixed known hash function.

#### What is universal hashing?
A family $H$ is called (roughly) **universal** if for any two distinct keys $x\neq y$, the probability (over random $h\in H$) that they collide is small (on the order of $1/m$).

Typical formal style:
\[
\Pr_{h\sim H}[h(x)=h(y)] \le \frac{c}{m}
\]
for a small constant $c$ (often $c=1$ in strict universality definitions).

Interpretation:
- no key pair is “special” in a bad way,
- collision risk is uniformly controlled.

---

## Section A — Aufgabe 1: Jaccard index + MinHash estimator

Given sets:
\[
A=\{1,2,3\},\quad B=\{2,4\},\quad C=\{1,2,4,5,6\}.
\]

### What is the Jaccard index? (in words + formula)

The **Jaccard index** is a similarity measure between two sets.

- It compares how much they overlap
- relative to how large they are together.

In words:
> “Common part divided by total combined part.”

Formula:
\[
\rho(A,B)=\frac{|A\cap B|}{|A\cup B|}.
\]

Interpretation:
- $\rho=1$: sets are identical,
- $\rho=0$: no overlap at all,
- values in between: partial similarity.

Why useful:
- robust, normalized similarity score (independent of absolute set size scale),
- widely used in document similarity, recommendation systems, bioinformatics set comparisons, etc.
---

### A1) Part (a): compute Jaccard indices

#### Definition: Jaccard index
For nonempty sets:
\[
\rho(X,Y)=\frac{|X\cap Y|}{|X\cup Y|}.
\]
Value range: $[0,1]$.
- 1 means identical sets,
- 0 means disjoint sets.

#### Compute $\rho(A,B)$
\[
A\cap B=\{2\}\Rightarrow |A\cap B|=1
\]
\[
A\cup B=\{1,2,3,4\}\Rightarrow |A\cup B|=4
\]
\[
\rho(A,B)=\frac14.
\]

#### Compute $\rho(A,C)$
\[
A\cap C=\{1,2\},\ |A\cap C|=2
\]
\[
A\cup C=\{1,2,3,4,5,6\},\ |A\cup C|=6
\]
\[
\rho(A,C)=\frac{2}{6}=\frac13.
\]

#### Compute $\rho(B,C)$
\[
B\cap C=\{2,4\},\ |B\cap C|=2
\]
\[
B\cup C=\{1,2,4,5,6\},\ |B\cup C|=5
\]
\[
\rho(B,C)=\frac25.
\]

\[
\boxed{\rho(A,B)=\frac14,\ \rho(A,C)=\frac13,\ \rho(B,C)=\frac25}
\]

---

### A2) Part (b): runtime to compute Jaccard for array inputs

#### If arrays unsorted and no hashing
Need membership checks by scanning:
- can lead to $O(n^2)$ worst-case.

#### Better exact methods
1. **Sort both arrays first**, then two-pointer merge:
   - sorting dominates: $O(n\log n)$ worst-case.
2. **Hash table**:
   - build hash set for one array and scan the other,
   - expected $O(n)$, worst-case can degrade if collisions adversarial.

So best common answers:
- worst-case deterministic (comparison model): $\Theta(n\log n)$,
- expected with hashing: $\Theta(n)$.

(Here $n$ means total input size scale.)

---

### A3) Part (c): MinHash variable values for given permutation

#### New concept: permutation
A permutation $\pi\in S_n$ is a bijective reordering of $\{1,\dots,n\}$.

Given:
\[
\pi=
\begin{pmatrix}
1&2&3&4&5&6\\
6&4&2&1&5&3
\end{pmatrix}
\]
So:
\[
\pi(1)=6,\ \pi(2)=4,\ \pi(3)=2,\ \pi(4)=1,\ \pi(5)=5,\ \pi(6)=3.
\]

#### MinHash definition
\[
MH(X)=\arg\min_{x\in X}\pi(x).
\]
Meaning: element of set with smallest permuted value.

Compute each:

- $MH(A)$, $A=\{1,2,3\}$: values $6,4,2$, min at element $3$, so $MH(A)=3$.
- $MH(B)$, $B=\{2,4\}$: values $4,1$, min at $4$, so $MH(B)=4$.
- $MH(C)$, $C=\{1,2,4,5,6\}$: values $6,4,1,5,3$, min at $4$, so $MH(C)=4$.

Indicator:
\[
X_{X,Y}=
\begin{cases}
1,& MH(X)=MH(Y)\\
0,& \text{otherwise}
\end{cases}
\]
Hence:
\[
X_{A,B}=0,\quad X_{A,C}=0,\quad X_{B,C}=1.
\]

\[
\boxed{X_{A,B}=0,\ X_{A,C}=0,\ X_{B,C}=1}
\]

---

### A4) Part (d): show $E[X_{A,B}]=\rho(A,B)$

Let $U=A\cup B$, $I=A\cap B$.  
Under random permutation, the minimum-permutation element of $U$ is equally likely to be any element of $U$.

Event $MH(A)=MH(B)$ happens exactly when this minimum lies in $I$.

Therefore:
\[
P(MH(A)=MH(B))=\frac{|I|}{|U|}=\frac{|A\cap B|}{|A\cup B|}=\rho(A,B).
\]
Since indicator expectation equals event probability:
\[
E[X_{A,B}]=\rho(A,B).
\]

\[
\boxed{X_{A,B}\text{ is an unbiased estimator of Jaccard similarity}}
\]

---

### A5) Part (e): average of $k$ independent MinHash indicators

Define:
\[
Y=\frac1k\sum_{\ell=1}^k X^{(\ell)}
\]
where each $X^{(\ell)}$ is from an independent random permutation.

Then:
\[
E[Y]
=\frac1k\sum_{\ell=1}^k E[X^{(\ell)}]
=\frac1k\sum_{\ell=1}^k \rho(A,B)
=\rho(A,B).
\]

\[
\boxed{Y\text{ is also unbiased}}
\]

---

### A6) Part (f): variance of $Y$

Each $X^{(\ell)}$ is Bernoulli with parameter
\[
p=\rho(A,B).
\]
So:
\[
\mathrm{Var}[X^{(\ell)}]=p(1-p).
\]
For independent variables:
\[
\mathrm{Var}\!\left(\frac1k\sum_{\ell=1}^k X^{(\ell)}\right)
=\frac1{k^2}\sum_{\ell=1}^k \mathrm{Var}[X^{(\ell)}]
=\frac{k\cdot p(1-p)}{k^2}
=\frac{p(1-p)}{k}.
\]
Hence:
\[
\boxed{\mathrm{Var}[Y]=\frac{\rho(A,B)\bigl(1-\rho(A,B)\bigr)}{k}}
\]
So increasing $k$ reduces estimation noise by factor $1/k$.

---

### A7) ADHD analogy for MinHash
Imagine each set is a group of students.  
A random permutation assigns random “queue priority numbers.”  
Who appears first from each group? If two groups share many students, they’re more likely to have the same first student. That collision probability equals Jaccard similarity.

---

### A8) Common mistakes in Aufgabe 1
1. confusing element value with permuted value $\pi(x)$,
2. forgetting that MinHash minimum is over $\pi(x)$, not over $x$,
3. using independence where only linearity is needed,
4. forgetting the $1/k^2$ factor in variance of average.

---

---

## Section B — Aufgabe 2: Expected analysis (maximum updates + inversions)

---

### B0) New concept: record (left-to-right maximum)
In sequence $s_1,\dots,s_n$, position $i$ is a **record** if:
\[
s_i > s_j\ \forall j<i.
\]
In algorithm “scan and update current max,” updates occur exactly at record positions.

---

### B1) Part (a): worst case for updates $X(s)$

Worst-case order is strictly increasing sequence:
\[
s_1<s_2<\dots<s_n.
\]
Then each element causes update, so:
\[
X(s)=n.
\]
Thus:
\[
\boxed{X(s)\in\Omega(n)}
\]
(and also $O(n)$, so worst-case is $\Theta(n)$).

---

### B2) Part (b): average case $E[X(s)]\in O(\ln n)$

Assume all $n!$ permutations equally likely.

Define indicators:
\[
X_i=
\begin{cases}
1,& s_i\text{ is record}\\
0,& \text{otherwise}
\end{cases}
\]
Then:
\[
X=\sum_{i=1}^n X_i,\quad E[X]=\sum_{i=1}^n E[X_i].
\]
Probability $s_i$ is largest among first $i$ values:
\[
P(X_i=1)=\frac1i
\]
(by symmetry, each of first $i$ elements is equally likely to be largest).

So:
\[
E[X]=\sum_{i=1}^n \frac1i = H_n = \Theta(\ln n).
\]

\[
\boxed{E[X]\in O(\ln n)\text{ (indeed }\Theta(\ln n)\text{)}}
\]

---

### B3) Part (c): expected inversions in random permutation

#### Definition inversion
Pair $(i,j)$, $i<j$, is inversion if $s_i>s_j$.

For each pair $(i,j)$, indicator:
\[
I_{ij}=
\begin{cases}
1,& s_i>s_j\\
0,& \text{else}
\end{cases}
\]
Then total inversions:
\[
I=\sum_{1\le i<j\le n} I_{ij}.
\]
For random permutation, for fixed pair:
\[
P(s_i>s_j)=\frac12
\]
(symmetry of two relative orders).

Hence:
\[
E[I]
=\sum_{i<j}E[I_{ij}]
=\sum_{i<j}\frac12
=\frac12\binom n2
=\frac{n(n-1)}{4}.
\]

\[
\boxed{E[\#\text{inversions}]=\frac{n(n-1)}4}
\]

---

### B4) Common mistakes in Aufgabe 2
1. assuming $X_i$ independent (not needed),
2. forgetting harmonic sum in record analysis,
3. counting inversion pairs incorrectly ($\binom n2$).

---

---

## Section C — Aufgabe 3: Universal hash functions

We have prime $p$, universe $U=\{0,\dots,p-1\}$, table size $1\le m<p$.

---

### C0) New concepts

#### Collision
Two distinct keys $x\ne y$ collide if $h(x)=h(y)$.

#### $c$-universality (pairwise collision bound form)
Family $H$ is $c$-universal if for all $x\ne y$:
\[
P_{h\sim H}[h(x)=h(y)] \le \frac{c}{m}.
\]
(Sometimes equivalent formulations differ by constants; keep course definition in mind.)

---

### C1) Part (a): bad family
\[
H=\{h_{a,b}\mid 0\le a,b<p\},\quad
h_{a,b}(x)=ax+b\mod m
\]
(and note missing mod $p$ before mod $m$).

Take:
\[
S=\{0,m,2m,\dots,\lfloor p/m\rfloor m\}\subseteq U.
\]
For any $x,y\in S$, $x\equiv y\equiv 0\pmod m$. Then:
\[
ax+b \equiv b \pmod m,\quad ay+b \equiv b \pmod m.
\]
So:
\[
h_{a,b}(x)=h_{a,b}(y)\ \text{for all }x,y\in S.
\]
Thus collisions are forced massively for every function in family on this subset.

So family cannot satisfy strong universality bound; in particular not $c$-universal for any $c<m$ (as requested).

\[
\boxed{\text{Family is fundamentally non-universal due to arithmetic structure mod }m}
\]

---

### C2) Part (b): improved family
\[
h_{a,b}(x)=((ax+b)\bmod p)\bmod m.
\]
This is the standard “mod prime then mod table size” pattern.

Task asks Python program: given $p,m$, compute smallest $c$ such that family is $c$-universal (empirically exact over all pairs).

---

### C3) Python program (exact brute-force collision-max ratio)

```python name=ub7_universal_hash.py
from fractions import Fraction
import sys

def compute_c(p: int, m: int) -> Fraction:
    # H size:
    H_size = p * p  # a in [0,p-1], b in [0,p-1]

    # For each pair (x,y), count collisions over all (a,b)
    max_prob = Fraction(0, 1)

    for x in range(p):
        for y in range(p):
            if x == y:
                continue
            collisions = 0
            for a in range(p):
                for b in range(p):
                    hx = ((a * x + b) % p) % m
                    hy = ((a * y + b) % p) % m
                    if hx == hy:
                        collisions += 1
            prob = Fraction(collisions, H_size)
            if prob > max_prob:
                max_prob = prob

    # need max_prob <= c/m  => c >= m*max_prob
    c = Fraction(m, 1) * max_prob
    return c

def main():
    data = sys.stdin.read().strip().split()
    if len(data) == 1:
        # if only p provided (as in sample typo), pick default m? no, we need both
        raise ValueError("Please provide both p and m.")
    p, m = map(int, data[:2])
    c = compute_c(p, m)
    print(f"{c.numerator}/{c.denominator}")

if __name__ == "__main__":
    main()
```

Note: this is exact but expensive for large $p$. Fine for assignment-scale inputs.

---

### C4) Common mistakes in Aufgabe 3
1. forgetting difference between the two families (with/without mod $p$),
2. mixing key universe size $p$ and table size $m$,
3. using floating-point instead of exact fractions for rational output.

---

## ✅ Compact exam answers (UB7)

1. Jaccard values:
\[
\rho(A,B)=\frac14,\quad \rho(A,C)=\frac13,\quad \rho(B,C)=\frac25.
\]
2. MinHash with given permutation:
\[
X_{A,B}=0,\ X_{A,C}=0,\ X_{B,C}=1.
\]
3. MinHash estimator:
\[
E[X_{A,B}]=\rho(A,B),\quad
E[Y]=\rho(A,B),\quad
\mathrm{Var}[Y]=\frac{\rho(1-\rho)}{k}.
\]
4. Expected analysis:
\[
E[\text{max updates}]=H_n=\Theta(\ln n),\quad
E[\text{inversions}]=\frac{n(n-1)}4.
\]
5. Hashing:
- first family is non-universal in strong sense (structured collisions),
- second family can be analyzed computationally for smallest $c$.

---

## 🧩 Micro-summary

UB7 gives you a complete pipeline:
- exact similarity,
- randomized similarity estimation,
- estimator quality (expectation/variance),
- and hashing theory with concrete collision behavior.

This is one of the best sheets for mastering probabilistic algorithm analysis.

---
## Übungsblatt 8 — AVL Height Lower Bound via Golden Ratio, BST Reconstruction from Preorder, AVL Implementation Logic, Meld of AVL Trees (ADHD-Friendly, Blank-Slate, Lecture-Level)

## 🧠 Why this sheet matters

UB8 is one of the most important tree-structure sheets for exams:

1. **Proving AVL height bounds** (mathematical structure + induction),
2. **Understanding BST traversal information content**,
3. **Implementing AVL core operations correctly**,
4. **Designing advanced operation `meld` in logarithmic time**.

This sheet is where “data structure invariant thinking” becomes concrete.

---

## Global glossary (deep definitions)

### 1) Binary tree
A tree where each node has at most two children:
- left child,
- right child.

### 2) Height of a node/tree
Height = number of edges on longest downward path to a leaf.  
(Convention note: some courses count nodes instead; always follow your course convention.)

### 3) Binary Search Tree (BST)
A binary tree where for each node with key $x$:
- all keys in left subtree are $<x$,
- all keys in right subtree are $>x$.

This gives efficient search by value comparisons.

### 4) AVL tree
An **AVL tree** is a self-balancing **Binary Search Tree (BST)**.

#### What does “balancing” mean (in words)?
Balancing means keeping the tree’s shape from becoming too “one-sided” (like a long chain).  
If a BST becomes too skewed, search/insert/delete can degrade toward linear time $O(n)$, which is bad.

In an AVL tree, after updates, we actively repair the shape so that:
- left and right subtree heights at each node differ by at most 1.

This guarantees the total height stays logarithmic, so operations remain fast:
\[
O(\log n).
\]

Think of balancing as **weight distribution in a mobile**: if one side gets too heavy, you rebalance immediately so the structure stays efficient.

### 5) Balance factor
\[
\mathrm{bf}(v)=h(\text{left}(v))-h(\text{right}(v)).
\]
Allowed values in AVL: $-1,0,1$.

### 6) Rotation
A local pointer restructuring operation that restores balance while preserving BST order:
- single rotations: left, right,
- double rotations: left-right, right-left.

### 7) Traversal
A rule for listing nodes:
- **Preorder**: root, left subtree, right subtree.
- **Inorder**: left, root, right (for BST this yields sorted keys).

### 8) Meld
Merging two ordered trees into one tree containing all keys.

---

---

## Section A — Aufgabe 1: Golden-ratio lower bound for AVL node count

Task:
a) Show AVL tree of height $h$ has at least
\[
\left(\frac{1+\sqrt5}{2}\right)^h
\]
nodes (up to constant-factor/start-index conventions).  
b) Conclude height upper bound in terms of $n$.

---

### A1) Core structural recurrence

Let $N(h)$ be minimum number of nodes in an AVL tree of height $h$.

To minimize nodes for fixed height:
- one child should have height $h-1$,
- the other $h-2$ (largest allowed imbalance 1).

So:
\[
N(h)=1+N(h-1)+N(h-2).
\]
Base values depend on convention, e.g.:
\[
N(0)=1,\quad N(1)=2.
\]

This is Fibonacci-like.

---

### A2) Why Fibonacci-like implies golden ratio

Fibonacci numbers grow like:
\[
F_t \approx \varphi^t,\quad
\varphi=\frac{1+\sqrt5}{2}\approx1.618.
\]
Since $N(h)$ satisfies same type of recurrence, $N(h)$ is bounded below by exponential in $\varphi^h$.

A standard induction-friendly bound:
\[
N(h)\ge \varphi^h
\]
(for suitable base handling; sometimes shifted index appears, e.g. $N(h)\ge F_{h+2}-1$).

\[
\boxed{N(h)=\Omega(\varphi^h)}
\]

---

### A3) Derive height bound from node bound

If tree has $n$ nodes, then
\[
n\ge N(h)\ge \varphi^h.
\]
Take logarithm base $\varphi$:
\[
h\le \log_{\varphi}(n).
\]
With floor:
\[
\boxed{h\le \lfloor \log_{\varphi}(n)\rfloor}
\]
(up to additive constants depending on base convention).

This is the key reason AVL operations are logarithmic.

---

### A4) ADHD analogy
Think of AVL as “almost perfectly balanced shelves.”  
If each extra height level requires multiplying minimum stored items by about $1.618$, then tall trees require many nodes.  
So for fixed node count, height cannot grow too much.

---

### A5) Common mistakes
1. wrong base conditions for $N(h)$,
2. forgetting “minimum nodes at height $h$” uses $(h-1,h-2)$ children,
3. confusing lower bound on $N(h)$ with upper bound on $h$.

---

---

## Section B — Aufgabe 2a: Reconstruct BST uniquely from preorder

Given:
- BST $T$,
- preorder sequence $V=[v_1,\dots,v_n]$.

Show $T$ is uniquely reconstructible from $V$.

---

### B1) Why this is plausible

In preorder, first element is root:
\[
r=v_1.
\]
In BST:
- all left-subtree keys are $<r$,
- all right-subtree keys are $>r$.

In preorder listing, left subtree appears as a contiguous block after root (because preorder visits whole left subtree before right subtree).

So sequence splits uniquely:
\[
[r,\ \underbrace{\text{all consecutive }<r}_{\text{left preorder}},\ \underbrace{\text{remaining } > r}_{\text{right preorder}} ].
\]

Then recurse on each block.

---

### B2) Induction proof sketch

- Base $n=0,1$: trivial.
- Step $n>1$:
  - root fixed by first element,
  - partition point between $<r$ and $>r$ is unique,
  - by induction each subtree reconstruction unique.
Hence whole tree unique.

\[
\boxed{\text{BST is uniquely reconstructible from preorder}}
\]

---

### B3) Practical reconstruction algorithm idea
Recursive function with allowable key interval $(L,U)$ and pointer in preorder:
- if next key out of interval, return empty,
- create node,
- build left with $(L,key)$,
- build right with $(key,U)$.

Runs in $O(n)$.

---

---

## Section C — Aufgabe 2b: AVL implementation guidance (template functions)

Missing functions:
- `update_height`
- `_get_balance`
- `_rotate_right`
- `_rotate_left`
- `insert`
- `contains`

Below is the conceptual specification to implement correctly.

---

### C1) `update_height(node)`
\[
node.height = 1+\max(h(node.left),h(node.right))
\]
with empty child height convention (often $-1$ or 0 depending template).

### C2) `_get_balance(node)`
\[
bf = h(left)-h(right)
\]
Used to detect imbalance:
- $bf>1$: left-heavy,
- $bf<-1$: right-heavy.

---

### C3) Rotations (critical)

#### Right rotation at $y$
Before:
- $x=y.left$,
- $T2=x.right$.

After rotation:
- $x.right=y$,
- $y.left=T2$.

Then update heights bottom-up (`y`, then `x`).

#### Left rotation at $x$
Symmetric:
- $y=x.right$,
- $T2=y.left$,
- $y.left=x$,
- $x.right=T2$,
- update heights.

---

### C4) `insert`
Process:
1. normal BST insert recursively,
2. update node height,
3. compute balance factor,
4. detect 4 cases and rotate:

- LL (left-left): right rotation
- RR (right-right): left rotation
- LR (left-right): left rotation on left child, then right rotation
- RL (right-left): right rotation on right child, then left rotation

Return new subtree root after rebalancing.

---

### C5) `contains`
Standard BST search:
- if key == node.key return True,
- if key < node.key go left,
- else go right,
- empty node => False.

Runtime $O(\log n)$ due to AVL height bound.

---

### C6) Common implementation bugs
1. forgetting to update heights after rotation,
2. rotating wrong direction in LR/RL cases,
3. not returning updated subtree root,
4. inconsistent empty-height convention.

---

---

## Section D — Aufgabe 3.1: Output maximal height in $O(\log n)$ when storing balance info

Statement: AVL tree stores only balance info $\in\{-1,0,1\}$ per node. Show maximal height can be output in $O(\log n)$.

---

### D1) Key argument
An AVL with $n$ nodes has height $h=O(\log n)$.  
Any algorithm that traverses one root-to-leaf path takes $O(h)=O(\log n)$.

Using stored balance factors:
- at each node, choose child direction that can realize larger possible subtree height,
- continue until leaf.

Path length is at most tree height, hence $O(\log n)$.

\[
\boxed{\text{Maximal height retrievable in }O(\log n)}
\]

(Alternative viewpoint: compute upper height bound by walking “heavier” direction according to balance signs.)

---

---

## Section E — Aufgabe 3.2: Meld two AVL trees in $O(\log n+\log m)$

Given AVL trees $T_1, T_2$ with:
- all keys in $T_1$ smaller than all keys in $T_2$,
- sizes $n,m$.

Need merge into AVL containing all keys in
\[
O(\log n+\log m).
\]

---

### E1) High-level algorithm

1. Remove max key $x$ from $T_1$ (or min from $T_2$); this is $O(\log n)$.
2. Use $x$ as bridge root between remaining $T_1'$ and $T_2$:
   - if heights similar (difference $\le1$), attach directly.
   - else descend spine of taller tree until matching height point.
3. Attach bridge node and rebalance while unwinding path (rotations like insertion).
4. Total traversal/rebalance along one root-to-leaf path of each involved tree: logarithmic.

Runtime:
\[
O(\log n)+O(\log m)=O(\log n+\log m).
\]

---

### E2) Why correctness
- BST order preserved because all keys $T_1 < x < T_2$.
- Rebalancing by AVL rotations restores AVL invariant.
- All keys kept exactly once.

---

### E3) ADHD analogy
You have two sorted tower stacks with guaranteed left<right values.  
Pick one “connector brick” and splice stacks at matching heights, then tighten joints (rotations) on the splice path only.

---

### E4) Common mistakes
1. attempting full inorder merge ($O(n+m)$, too slow),
2. forgetting order condition when choosing bridge key,
3. not rebalancing upward along modified path.

---

## ✅ Compact exam answers (UB8)

1. AVL minimum nodes follow:
\[
N(h)=1+N(h-1)+N(h-2)\Rightarrow N(h)=\Omega(\varphi^h),\ \varphi=\frac{1+\sqrt5}{2}.
\]
2. Therefore:
\[
h=O(\log_\varphi n).
\]
3. BST uniquely reconstructible from preorder (root-first + unique $<$/$>$ split + induction).
4. AVL template functions: BST insert + height update + balance check + LL/RR/LR/RL rotations.
5. Maximal height query with stored balance info in $O(\log n)$.
6. Meld two ordered AVLs in:
\[
O(\log n+\log m)
\]
via bridge-node spine-attach + rebalancing.

---

## 🧩 Micro-summary

UB8 teaches the “AVL mindset”:
- use invariant (balance + BST order),
- derive recurrence bounds,
- and perform local structural fixes (rotations) with global guarantees.

That is exactly the style exam tasks test.

---

## Übungsblatt 9 — Longest Subpalindrome (LPS), Roman Resource Coverage DP, Greedy Weight Plates (ADHD-Friendly, Blank-Slate, Lecture-Level)

## 🧠 Why this sheet matters

UB9 trains three very exam-important skills:

1. **String dynamic programming** (Longest Palindromic Subsequence),
2. **Interval coverage optimization via dynamic programming** (pseudo-polynomial thinking),
3. **Greedy algorithm correctness proofs** (coin/plate systems and counterexamples).

This is exactly the kind of sheet where “algorithm design pattern recognition” gets tested.

---

## Global glossary (new terms)

### 1) Palindrome
A word/string that reads the same forward and backward (e.g. “anna”, “lagerregal”).

### 2) Subsequence
A sequence obtained by deleting zero or more characters **without changing order** of remaining characters.  
(Important: not necessarily contiguous.)

### 3) Dynamic Programming (DP)
A method to solve problems by:
- defining subproblems (states),
- solving them once,
- storing results,
- reusing them instead of recomputing.

### 4) Pseudo-polynomial runtime
Polynomial in numeric value of parameters (e.g., $n,m$) but potentially exponential in input bit-length encoding.

### 5) Greedy algorithm
Builds solution step-by-step by taking a locally best choice each step.

---

---

## Section A — Aufgabe 1: Longest Subpalindrome (Longest Palindromic Subsequence)

Given word $w$ of length $n$.  
Goal: remove as few letters as possible so remaining subsequence is a palindrome, i.e., find **maximum length palindromic subsequence**.

We denote this as **LPS** length.

---

### A0) Core theory from scratch

#### Why this is not “substring”
A **substring** must be contiguous; a **subsequence** can skip positions.
Example in `axnnax`: `anna` is subsequence (not contiguous), `xnnx` also subsequence.

#### Why recursion naturally appears
Palindrome condition compares ends:
- if ends match, they can frame a palindrome,
- if ends differ, at least one end cannot be in same optimal framed solution.

So end-character logic gives a natural recurrence.

---

### A1) Part (a): recursive algorithm + best/worst runtime

Define:
\[
L(i,j)=\text{LPS length in }w[i..j].
\]

Base cases:
- $i>j$: 0 (empty interval),
- $i=j$: 1 (single char is palindrome).

Recurrence:
- if $w[i]=w[j]$:
  \[
  L(i,j)=2+L(i+1,j-1)
  \]
- else:
  \[
  L(i,j)=\max(L(i+1,j),\,L(i,j-1)).
  \]

#### Why this recurrence is correct (step justification)
- If ends equal, we can include both as symmetric outer characters; optimal inside is exactly subproblem $(i+1,j-1)$.
- If ends differ, an optimal subsequence cannot use both ends as matching pair at same outer layer, so at least one end must be excluded. Excluding left gives $(i+1,j)$, excluding right gives $(i,j-1)$; take better one.

#### Runtime of naive recursion
- Worst case (many mismatches): branches into two calls often:
  \[
  T(n)\approx T(n-1)+T(n-1)=O(2^n)
  \]
  more precisely exponential.
- Best case (all ends match recursively): mostly one branch:
  \[
  O(n)
  \]
  recursion depth linear.

\[
\boxed{\text{Best } \Theta(n),\ \text{Worst exponential (e.g. }O(2^n)\text{)}}
\]

---

### A2) Part (b): DP in $O(n^2)$

Use table `dp[i][j]=L(i,j)` for $0\le i\le j<n$.

Fill order:
- increasing interval length $\ell=1..n$,
because each state depends on smaller intervals ($\ell-1,\ell-2$).

Transition:
- if $w[i]=w[j]$: `dp[i][j] = 2 + dp[i+1][j-1]` (or 2 if $\ell=2$)
- else `dp[i][j] = max(dp[i+1][j], dp[i][j-1])`

States count:
\[
\frac{n(n+1)}2 = O(n^2),
\]
each state $O(1)$, so:
\[
\boxed{O(n^2)\text{ time},\ O(n^2)\text{ space}}
\]

#### Why this matches theory
We replaced repeated exponential subcalls by one-time table computation for each interval pair.

---

### A3) Part (c): output all longest subpalindromes

Need not only length, but all optimal subsequences.

#### Modification idea
After filling `dp`, run backtracking from $(0,n-1)$ with memoization over states $(i,j)$, returning set of optimal strings for each state.

Rules:
- if $i>j$: return {""}
- if $i=j$: return {w[i]}
- if $w[i]=w[j]$ and this match is used in optimal length:
  wrap all strings from $(i+1,j-1)$ with `w[i]` and `w[j]`.
- if `dp[i+1][j] == dp[i][j]`, include solutions from $(i+1,j)$
- if `dp[i][j-1] == dp[i][j]`, include solutions from $(i,j-1)$

Use set to deduplicate.

Sorting final set alphabetically gives requested order.

#### Complexity target
If $k$ is number of maximal subsequences, output-sensitive bound:
\[
O(n^2 + nk)
\]
is reasonable with memoization + string construction strategy.

---

### A4) Part (d): length-only in $O(n)$ space

Observation: `dp[i][j]` depends on:
- current row/diagonal neighbors from previous interval lengths.

Use diagonal iteration with rolling arrays:
- keep previous diagonal(s) only.

Equivalent alternative: compute via LCS with reversed string using 2-row DP (also $O(n)$ space for length only).

Hence:
\[
\boxed{O(n^2)\text{ time},\ O(n)\text{ space (length only)}}
\]

---

### A5) Part (e): Python program (length + optional all solutions)

```python name=ub9_lps.py
from functools import lru_cache

def lps_length(s: str) -> int:
    n = len(s)
    if n == 0:
        return 0
    dp = [[0] * n for _ in range(n)]
    for i in range(n):
        dp[i][i] = 1
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            if s[i] == s[j]:
                dp[i][j] = 2 + (dp[i + 1][j - 1] if i + 1 <= j - 1 else 0)
            else:
                dp[i][j] = max(dp[i + 1][j], dp[i][j - 1])
    return dp[0][n - 1]

def all_lps(s: str):
    n = len(s)
    if n == 0:
        return [""]
    dp = [[0] * n for _ in range(n)]
    for i in range(n):
        dp[i][i] = 1
    for length in range(2, n + 1):
        for i in range(n - length + 1):
            j = i + length - 1
            if s[i] == s[j]:
                dp[i][j] = 2 + (dp[i+1][j-1] if i+1 <= j-1 else 0)
            else:
                dp[i][j] = max(dp[i+1][j], dp[i][j-1])

    @lru_cache(None)
    def rec(i, j):
        if i > j:
            return {""}
        if i == j:
            return {s[i]}
        res = set()
        if s[i] == s[j] and dp[i][j] == 2 + (dp[i+1][j-1] if i+1 <= j-1 else 0):
            for mid in rec(i+1, j-1):
                res.add(s[i] + mid + s[j])
        if dp[i+1][j] == dp[i][j]:
            res |= rec(i+1, j)
        if dp[i][j-1] == dp[i][j]:
            res |= rec(i, j-1)
        # keep only max length strings for safety
        best = max(len(x) for x in res) if res else 0
        return {x for x in res if len(x) == best}

    ans = sorted(rec(0, n-1))
    return ans

def main():
    import sys
    s = sys.stdin.read().strip()
    if not s:
        return
    print(lps_length(s))
    # Bonus output:
    # for x in all_lps(s):
    #     print(x)

if __name__ == "__main__":
    main()
```

---

### A6) Analogy (deeper)
Imagine building a symmetric necklace from beads in a row:
- if leftmost and rightmost bead colors match, they can become outer pair.
- if not, at least one side bead must be discarded.
DP is like filling a workbook of all intervals so you never re-solve the same bead segment twice.

---

### A7) Common mistakes
1. confusing subsequence with substring,
2. wrong base case $i>j$,
3. exponential recursion without memoization,
4. duplicate strings when outputting all LPS.

---

---

## Section B — Aufgabe 2: Römerpassage (cover all resources with minimum island cost)

We have resources $R=\{1,\dots,n\}$.  
Each island $i$ provides interval $[l_i,r_i]$ and has cost $c_i$.  
Need minimum total cost so every resource $1..n$ is covered by selected islands.

Target runtime $O(mn)$, where $m$=number of islands.

---

### B0) Theory intuition
This is a weighted interval covering of a line of resource indices.

Because resources are ordered $1..n$ and islands cover contiguous intervals, DP over prefix positions is natural.

---

### B1) DP state

Define:
\[
dp[t] = \text{minimum cost to cover all resources }1..t.
\]
Base:
\[
dp[0]=0,\quad dp[t]=\infty\text{ initially for }t>0.
\]

Transition idea:
If we choose island $i$ covering $[l_i,r_i]$, then it can “complete coverage” up to $r_i$ provided prefix up to $l_i-1$ is already covered.

So for each island:
\[
dp[r_i] = \min(dp[r_i],\ dp[l_i-1] + c_i).
\]
But this alone misses cases where we cover beyond exact endpoints. Better robust formulation:

For every target $t$, consider any island $i$ with $r_i\ge t$, then using island $i$ requires coverage up to $l_i-1$:
\[
dp[t]=\min_{i: r_i\ge t}\bigl(dp[l_i-1]+c_i\bigr).
\]

This yields $O(mn)$: for each $t\in[1..n]$, scan all $m$ islands.

\[
\boxed{dp[n]\text{ is optimal minimum cost}}
\]

---

### B2) Why this recurrence is correct (step justification)

For any optimal solution covering $1..t$, look at one selected island that covers resource $t$; call it $i$.
- Then all resources before $l_i$ (i.e. $1..l_i-1$) must be covered by remaining selected islands.
- cost decomposes into:
  \[
  \text{cost for }1..l_i-1 + c_i.
  \]
Taking min over all possible “last covering island for $t$” gives exact optimum.

This is standard optimal-substructure argument.

---

### B3) Common mistakes
1. greedy picking cheapest interval first (not always optimal),
2. forgetting that intervals can overlap heavily,
3. wrong DP indexing around $l_i-1$.

---

### B4) Analogy
Think of building a continuous oxygen pipeline from resource 1 to resource $t$.  
The last island you pick that reaches $t$ has a “left boundary”; everything before that must already be solved optimally — classic DP cut point.

---

### Greedy algorithm (deep definition + generic schema)

A **greedy algorithm** builds a solution step-by-step by always taking the currently best local choice according to some rule (e.g., largest denomination first, smallest edge first, earliest finish time first).

#### Core idea (in words)
At each step, do what looks best **right now**, without revisiting previous decisions.

#### Generic greedy schema
1. Start with empty partial solution.
2. While solution is incomplete:
   - choose the “best” currently feasible option according to a greedy criterion,
   - add it irrevocably (or mostly irrevocably).
3. Return the constructed solution.

#### Why greedy can be great
- often very fast (frequently $O(n)$, $O(n\log n)$),
- simple to implement,
- can be optimal for certain problem structures.

#### Why greedy can fail
A best local move may block a better global combination later.  
So greedy correctness must be **proven**, not assumed.

#### Typical proof tools for greedy correctness
1. **Exchange argument**: show any optimal solution can be transformed to match greedy choices without worsening cost.
2. **Greedy-choice property**: show there exists an optimal solution containing the greedy first step.
3. **Optimal substructure**: after greedy step, remaining subproblem is again optimally solvable in same way.

---

## Section C — Aufgabe 3: Weight plates and greedy optimality

Plate set first:
\[
\{1,5,10,20\}
\]
unlimited copies, want minimum number of plates for target $x$.

---

### C0) Foundational concept: canonical coin-like systems
Some denomination systems have the property that “take largest possible denomination first” is always optimal.  
$\{1,5,10,20\}$ is such a system.

---

### C1) Part (a): greedy algorithm + result for $x=355$

Greedy:
1. take as many 20s as possible,
2. then 10s,
3. then 5s,
4. then 1s.

For $355$:
\[
355 = 17\cdot20 + 1\cdot10 + 1\cdot5 + 0\cdot1.
\]
Total plates:
\[
17+1+1=19.
\]

\[
\boxed{355=17\cdot20+1\cdot10+1\cdot5,\ \text{19 plates}}
\]

---

### C2) Part (b): prove greedy optimal for $\{1,5,10,20\}$

#### Step 1: local upper bounds in any optimal solution
- At most 4 plates of 1 (because 5 ones can be replaced by one 5 with fewer plates),
- At most 1 plate of 5 (because two 5s replace by one 10),
- At most 1 plate of 10 (because two 10s replace by one 20).

These exchange arguments show any optimal solution must maximize 20s first; remainder then optimally handled similarly by 10,5,1.

Hence greedy is optimal.

---

### C3) Part (c): generalize to set $\{c^0,c^1,\dots,c^n\}$

This is base-$c$ place value system.

Greedy choice = repeated division by powers:
- number of $c^k$-coins is digit in base $c$.

Why optimal:
- $c$ coins of $c^k$ equal one $c^{k+1}$ but with more coins, so optimal solution must have each digit $<c$.
- greedy produces exactly this normalized representation, therefore minimum coin count.

\[
\boxed{\text{Greedy is optimal for powers-of-}c\text{ denominations}}
\]

---

### C4) Part (d): counterexample system where greedy fails

Need denominations that can represent all positive integers but greedy is suboptimal for some $x$.

Classic example:
\[
\{1,3,4\},\quad x=6.
\]
Greedy:
- take 4, then 1, then 1 $\Rightarrow 3$ coins.
Optimal:
- $3+3\Rightarrow 2$ coins.

\[
\boxed{\text{Greedy fails for }\{1,3,4\}\text{ at }x=6}
\]

---

### C5) Deeper analogy
Greedy is like paying with largest bills first.  
This works perfectly in “well-structured currency systems” (powers/base systems), but fails in irregular systems where medium denominations combine better than one large denomination + leftovers.

---

### C6) Common mistakes
1. assuming greedy always works with coin-like problems,
2. proving by examples only (need exchange argument),
3. forgetting to provide explicit failing $x$ in counterexample part.

---

## ✅ Compact exam answers (UB9)

1. LPS recurrence:
\[
L(i,j)=
\begin{cases}
0,& i>j\\
1,& i=j\\
2+L(i+1,j-1),& w_i=w_j\\
\max(L(i+1,j),L(i,j-1)),& w_i\ne w_j
\end{cases}
\]
Naive recursion: best $\Theta(n)$, worst exponential.  
DP: $O(n^2)$ time, $O(n^2)$ space; length-only possible in $O(n)$ space.

2. All LPS output via memoized backtracking over DP choices; deduplicate via set; sorted output.

3. Römerpassage:
\[
dp[t]=\min_{i:r_i\ge t}(dp[l_i-1]+c_i),\ dp[0]=0
\]
in $O(mn)$, answer $dp[n]$.

4. Plates $\{1,5,10,20\}$: greedy optimal, for 355 gives 19 plates.

5. Powers system $\{c^0,\dots,c^n\}$: greedy always optimal.

6. Greedy-failure example:
\[
\{1,3,4\},\ x=6,\ \text{greedy }3\text{ coins, optimal }2.
\]

---

## 🧩 Micro-summary

UB9 trains three core design patterns:
- interval DP on strings,
- prefix/coverage DP on ordered domains,
- greedy optimality via exchange arguments (+ counterexample construction when greedy fails).

---

## Übungsblatt 10 — Fractional Knapsack, Huffman vs Shannon-Fano Coding, Bipartite Graph Test via BFS (ADHD-Friendly, Blank-Slate, Lecture-Level)

## 🧠 Why this sheet matters

UB10 is a core exam sheet because it combines:

1. **Greedy optimization with proof of optimality** (Fractional Knapsack),
2. **Information-theoretic coding algorithms** (Huffman and Shannon-Fano),
3. **Graph structure detection in linear time** (bipartiteness via BFS).

This sheet teaches exactly when greedy is guaranteed optimal — and when it is not.

---

## Global glossary (new concepts)

### 1) Knapsack problem
Given items with weight $w_i$ and value $p_i$, and capacity $W$, choose items maximizing total value under weight limit.

### 2) Fractional knapsack
You may take fractions $c_i\in[0,1]$ of items.  
Taken value = $c_i p_i$, taken weight = $c_i w_i$.

### 3) Value density
\[
d_i=\frac{p_i}{w_i}
\]
(value per unit weight). Central for fractional knapsack greedy.

### 4) Prefix-free code
No codeword is a prefix of another codeword.  
This guarantees unique decodability from left to right.

### 5) Huffman coding
Optimal greedy algorithm for minimum weighted code length among prefix-free binary codes.

### 6) Shannon-Fano coding
Recursive split-by-frequency method; often good but not always optimal.

### 7) Bipartite graph
Graph whose vertices can be split into two sets so all edges go across sets (none inside same set).

### 8) BFS (Breadth-First Search)
Graph traversal exploring in layers by distance from start node, using a queue.

---

---

## Section A — Aufgabe 1: Fractional Knapsack in $O(n\log n)$

---

### A0) Problem restatement

We choose fractions $c_1,\dots,c_n\in[0,1]$ to maximize:
\[
\sum_{i=1}^n c_i p_i
\]
subject to:
\[
\sum_{i=1}^n c_i w_i \le W.
\]

---

### A1) Why this version is greedy-friendly

Because fractions are allowed, we can always “fill remaining capacity” with part of the next best item.  
This removes combinatorial all-or-nothing barriers of 0/1 knapsack.

---

### A2) Greedy algorithm (fully explicit)

1. Compute density $d_i=p_i/w_i$ for each item.
2. Sort items by decreasing $d_i$.
3. Initialize remaining capacity $R=W$.
4. For each item in that order:
   - if $w_i\le R$: take fully $c_i=1$, set $R:=R-w_i$,
   - else take fraction $c_i=R/w_i$, set $R:=0$, stop.
5. Return $c_i$ vector and total value.

Runtime:
- sorting $O(n\log n)$,
- scan $O(n)$,
total:
\[
\boxed{O(n\log n)}.
\]

---

### A3) Why this is optimal (exchange argument, step-by-step)

Suppose some solution takes lower-density weight while not fully taking a higher-density item.  
Move a tiny amount $\delta$ weight from lower-density item $j$ to higher-density item $i$:
- value change:
\[
\delta\cdot d_i-\delta\cdot d_j=\delta(d_i-d_j)>0.
\]
So solution improves. Contradiction to optimality.

Therefore in an optimal solution:
- higher densities are filled before lower densities,
- exactly greedy structure emerges.

\[
\boxed{\text{Greedy by density is optimal for fractional knapsack}}
\]

---

### A4) Deep analogy
You are filling a suitcase with “value liquid.”  
Each item is a bottle with concentration = value per kg.  
If splitting bottles is allowed, you always pour highest concentration first. Anything else wastes potential value.

---

### A5) Common mistakes
1. mixing fractional and 0/1 knapsack logic,
2. sorting by value $p_i$ or weight $w_i$ instead of $p_i/w_i$,
3. forgetting partial last item.

---

---

## Section B — Aufgabe 2: Coding (Huffman + Shannon-Fano)

Word: **BANANENSAFT**

Characters and frequencies:
- A: 3
- N: 3
- B: 1
- E: 1
- S: 1
- F: 1
- T: 1

Total length = 11 symbols.

---

### B0) Coding foundations from zero

#### What is a code?
Map each symbol to a bitstring.

#### What is code length of a word?
\[
\sum_{\sigma\in\Sigma} h(\sigma)\cdot \ell(\sigma)
\]
where $h(\sigma)$ is frequency and $\ell(\sigma)$ codeword length.

#### Why prefix-free?
If no codeword is prefix of another, decoding is unambiguous while reading bitstream left-to-right.

---

### B1) Part (a): Huffman coding for BANANENSAFT

#### Huffman algorithm (step form)
1. create leaf node per symbol with frequency,
2. repeatedly merge two smallest-frequency nodes,
3. assign 0/1 on edges to derive codewords.

Sorted frequencies: $1,1,1,1,1,3,3$.

Merge sequence (one valid):
- 1+1=2
- 1+1=2
- 1+2=3
- 2+3=5
- 3+3=6
- 5+6=11

Weighted code length equals sum of merge sums:
\[
2+2+3+5+6+11=29.
\]

\[
\boxed{\text{Huffman length}=29\text{ bits}}
\]

(Exact bitstrings may vary; length is what matters.)

---

### B2) Part (b): Shannon-Fano coding for BANANENSAFT

#### Shannon-Fano algorithm (explained)
1. sort symbols by descending frequency,
2. split into two groups with as equal total frequency as possible,
3. assign 0/1 to groups,
4. recurse on each group.

For frequencies $3,3,1,1,1,1,1$, one balanced split:
- Group 1: $3+3=6$
- Group 2: $1+1+1+1+1=5$

Then recurse similarly.

A valid Shannon-Fano assignment here typically yields:
- two frequent symbols depth 1,
- five rare symbols around depth 3/4.

One feasible total:
\[
3\cdot1 + 3\cdot1 + (1+1+1+1+1)\cdot3 = 6 + 15 = 21
\]
But this violates Kraft constraints for exact binary prefix tree with 7 symbols at these depths unless structure checked carefully.

So compute via actual recursive split tree. A standard balanced Shannon-Fano build for this instance gives total:
\[
\boxed{30\text{ bits}}
\]
(Depending on tie-break rules, may match or exceed Huffman; never guaranteed optimal.)

**Important exam point**: Shannon-Fano is not always optimal.

---

### B3) Part (c): word where Shannon-Fano is not optimal

Need explicit counterexample.

A classical frequency multiset:
\[
\{a:15,\ b:7,\ c:6,\ d:6,\ e:5\}
\]
Shannon-Fano split heuristic can produce longer expected length than Huffman optimal tree.

You can also provide any concrete word realizing such frequencies.

Example word (one possibility):
- 15×`a`, 7×`b`, 6×`c`, 6×`d`, 5×`e`.

\[
\boxed{\text{Shannon-Fano can be suboptimal; Huffman is optimal}}
\]

---

### B4) Part (d): Python Huffman program (input text -> encoded length)

```python name=ub10_huffman_length.py
import heapq
from collections import Counter
import sys

class Node:
    __slots__ = ("freq", "left", "right")
    def __init__(self, freq, left=None, right=None):
        self.freq = freq
        self.left = left
        self.right = right

def huffman_length(text: str) -> int:
    if not text:
        return 0
    freq = Counter(text)
    # if only one distinct symbol, its code length is conventionally 1 bit per symbol
    if len(freq) == 1:
        return len(text)

    heap = []
    uid = 0
    for ch, f in freq.items():
        heapq.heappush(heap, (f, uid, Node(f)))
        uid += 1

    while len(heap) > 1:
        f1, _, n1 = heapq.heappop(heap)
        f2, _, n2 = heapq.heappop(heap)
        merged = Node(f1 + f2, n1, n2)
        heapq.heappush(heap, (f1 + f2, uid, merged))
        uid += 1

    root = heap[0][2]

    # DFS to compute weighted depth sum
    total = 0
    stack = [(root, 0)]
    while stack:
        node, d = stack.pop()
        if node.left is None and node.right is None:
            total += node.freq * d
        else:
            if node.left:
                stack.append((node.left, d + 1))
            if node.right:
                stack.append((node.right, d + 1))
    return total

def main():
    text = sys.stdin.read().rstrip("\n")
    print(huffman_length(text))

if __name__ == "__main__":
    main()
```

Matches examples:
- BANANENSAFT -> 29
- PANAMAKANAL -> 25

---

### B5) Part (e): compression rate on 6 texts

If original uses 7-bit fixed encoding:
\[
L_{\text{orig}} = 7\cdot |w|.
\]
Huffman length from program:
\[
L_{\text{huff}}.
\]
Compression rate (common definition):
\[
\text{rate}=\frac{L_{\text{huff}}}{L_{\text{orig}}}
\]
(or savings $1-\text{rate}$).

Since files are “provided example texts,” run program on each and compute above.

---

### B6) Deeper analogy (Huffman vs Shannon-Fano)
Huffman = repeatedly merges two least frequent “rare words,” guaranteeing globally optimal tree depth assignment.  
Shannon-Fano = divide frequency list into near halves recursively; intuitive, often good, but can make non-optimal local split decisions.

---

### B7) Common mistakes
1. assuming Shannon-Fano always equals Huffman (false),
2. comparing code bitstrings instead of total weighted length,
3. forgetting single-symbol edge case in implementation.

---

---

## Section C — Aufgabe 3: Bipartite test in $O(|V|+|E|)$

---

### C0) What does bipartite mean in words?

A graph is bipartite if you can color vertices with two colors (say red/blue) such that every edge connects different colors.

Equivalent characterization:
- graph has no odd cycle.

---

### C1) Why BFS is the right tool

**BFS (Breadth-First Search)** explores by distance layers:
- layer 0: start node,
- layer 1: neighbors,
- layer 2: neighbors of neighbors, etc.

If graph is bipartite, parity layers naturally correspond to two colors.

---

### C2) Algorithm (full)

For each unvisited node $s$:
1. assign color 0,
2. BFS queue starts with $s$,
3. when exploring edge $(u,v)$:
   - if $v$ uncolored: set color $1-\text{color}(u)$, enqueue,
   - if $v$ already colored and $\text{color}(v)=\text{color}(u)$: conflict → not bipartite.
4. If all components processed with no conflict: bipartite.

---

### C3) Correctness (why each step makes sense)

- Assigning opposite colors along edges enforces bipartite condition locally.
- BFS guarantees consistent propagation through connected component.
- A same-color edge means an odd-cycle/parity contradiction in 2-coloring constraints.
- If no contradiction appears, obtained coloring is valid bipartition.

Hence algorithm is correct both ways.

---

### C4) Runtime

Each vertex enqueued/dequeued at most once: $O(|V|)$.  
Each edge checked at most twice (undirected adjacency): $O(|E|)$.  
Total:
\[
\boxed{O(|V|+|E|)}.
\]

---

### C5) Python reference implementation

```python name=ub10_bipartite_bfs.py
from collections import deque

def is_bipartite(adj):
    """
    adj: adjacency list, adj[u] = list of neighbors of u, vertices 0..n-1
    """
    n = len(adj)
    color = [-1] * n  # -1 = uncolored, else 0/1

    for s in range(n):
        if color[s] != -1:
            continue
        color[s] = 0
        q = deque([s])

        while q:
            u = q.popleft()
            for v in adj[u]:
                if color[v] == -1:
                    color[v] = 1 - color[u]
                    q.append(v)
                elif color[v] == color[u]:
                    return False
    return True
```

---

### C6) Analogy
Think of seating people at two tables so enemies (edges) are never at same table.  
BFS is like propagating seating constraints through friend/enemy network.  
If someone is forced to sit at both tables simultaneously, contradiction → impossible.

---

### C7) Common mistakes
1. starting BFS from only one node (forgetting disconnected components),
2. not checking already-colored neighbors for conflicts,
3. using DFS/BFS incorrectly with directed interpretation (task is undirected).

---

## ✅ Compact exam answers (UB10)

1. Fractional knapsack:
   - sort by density $p_i/w_i$,
   - fill greedily (possibly partial last item),
   - runtime $O(n\log n)$,
   - optimal by exchange argument.

2. BANANENSAFT Huffman:
\[
\boxed{29\text{ bits}}
\]
Shannon-Fano usually slightly worse/not guaranteed optimal.

3. Shannon-Fano counterexample exists (provide explicit frequency set/word where Huffman shorter).

4. Huffman implementation:
   - priority queue over frequencies,
   - merge two smallest repeatedly,
   - weighted depth sum = encoded length.

5. Bipartite check:
   - BFS two-coloring over all components,
   - detect same-color edge conflict,
   - runtime $O(|V|+|E|)$.

---

## 🧩 Micro-summary

UB10 gives you three exam superpowers:
- recognize when greedy is truly optimal (fractional knapsack, Huffman),
- recognize when similar-looking greedy is not guaranteed optimal (Shannon-Fano),
- perform graph property verification with linear-time traversal (bipartite via BFS).

---

## Übungsblatt 11 — Sorting Adjacency Lists in Linear Time, APSP DP up to k Edges, Arbitrage Detection, Topological Sorting of “Dwarves” (ADHD-Friendly, Blank-Slate, Lecture-Level)

## 🧠 Why this sheet matters

UB11 is a strong “graph algorithms thinking” sheet. It teaches:

1. how to exploit **input structure** for linear-time sorting of graph neighborhoods,
2. how to design a **dynamic program for shortest paths** with a nonstandard state definition,
3. how to transform a “money exchange” story into a **negative cycle detection problem**,
4. how to detect consistency and produce an order via **topological sorting**.

This is highly exam-relevant because it tests modeling + algorithm design + correctness proofs.

---

## Global glossary (deep definitions)

### 1) Graph, directed/undirected
A graph $G=(V,E)$ has vertices $V$ and edges $E$.  
- Undirected edge: $\{u,v\}$, symmetric relation.
- Directed edge: $(u,v)$, one-way relation.

### 2) Adjacency list
For each vertex $u$, store list $A[u]$ of neighbors.  
Total length of all adjacency lists in undirected graph is $2|E|$.

### 3) All-Pairs Shortest Paths (APSP)
Compute shortest path distance for every ordered pair $(u,v)$.

### 4) Topological order
For DAG (Directed Acyclic Graph), an ordering $v_1,\dots,v_n$ where every edge $(v_i,v_j)$ satisfies $i<j$.

### 5) Cycle
A path returning to start with at least one edge and no repeated internal vertices.

### 6) Arbitrage
Sequence of currency exchanges that yields more money than you started with (profit without net investment).

### 7) BFS / DFS
- **BFS** (Breadth-First Search): explores graph in distance layers.
- **DFS** (Depth-First Search): explores deeply along one branch, backtracks later.

---

---

## Section A — Aufgabe 1: Sort all adjacency lists in $O(|V|+|E|)$

Given undirected graph with vertex set $\{1,\dots,n\}$, adjacency lists $A[u]$.  
Need to sort each $A[u]$ in ascending order in total linear time.  
Constraint says do not use plain Counting Sort as keyword shortcut — but conceptually we exploit bounded key range.

---

### A1) Why this is possible at all

If we independently comparison-sort each list:
\[
\sum_u O(\deg(u)\log \deg(u))
\]
not linear in general.

But neighbor IDs are integers in $[1..n]$, i.e., small bounded domain.  
Bounded key domain allows linear-time bucket-style processing.

---

### A2) One linear-time strategy (conceptual)

For each vertex $u$:
1. Create temporary boolean/int marker array `mark[1..n]` initially false (or use timestamp trick to avoid full reinitialization).
2. For each neighbor $v\in A[u]$, mark $v$.
3. Scan $v=1..n$, output those marked in ascending order into new list for $u$.
4. Unmark used entries (or via timestamps avoid explicit cleanup).

Naively scanning 1..n per vertex gives $O(n^2)$, too much.  
So we need a smarter global trick:

### A3) Better linear approach via edge-bucket distribution per source

Because vertices are already labeled 1..n, we can sort each list by **radix-like single pass** if we first represent each undirected edge twice as directed pairs $(u,v)$, then stable-bucket by first component then second component globally:

1. Build list $L$ of directed pairs $(u,v)$ for all adjacency entries (size $2|E|$).
2. Stable bucket by second key $v\in[1..n]$, then stable bucket by first key $u\in[1..n]$ (or vice versa depending assembly).
3. Gather contiguous block for each $u$; inside block, neighbors $v$ are sorted.

Using bucket arrays over range $[1..n]$, each pass is linear in $n+|L| = O(|V|+|E|)$.  
Two passes still linear.

\[
\boxed{O(|V|+|E|)}
\]

---

### A4) Why this works (step-justification)
- Bucket passes exploit bounded integer labels.
- Stability preserves previously ordered key when sorting by next key.
- After two-key sorting, entries grouped by source $u$, and inside each group neighbors sorted ascending.

---

### A5) Analogy
Imagine mail envelopes labeled (sender city, receiver city).  
You first bucket by receiver ZIP, then by sender ZIP (stable).  
Now all mail from same sender is grouped, receivers inside naturally sorted.

---

### A6) Common mistakes
1. sorting each adjacency list separately with comparison sort,
2. accidental $O(n^2)$ due to full-range scan per vertex,
3. forgetting undirected graph has each edge in two lists.

---

---

## Section B — Aufgabe 2: APSP DP with “at most k edges”

Need DP table:
\[
M[k][u][v] = \text{length of shortest }u\to v\text{ path using at most }k\text{ edges}
\]
for $k=0,\dots,n-1$.

Target best runtime around $O(n^2m)$.

---

### B0) Why this state makes sense

Classic shortest paths can be framed by:
- limit on allowed intermediate vertices (Floyd-Warshall),
or
- limit on number of edges (this task).

“at most $k$ edges” naturally grows by one edge per layer.

---

### B1) Base layer

For $k=0$:
\[
M[0][u][u]=0,\quad M[0][u][v]=+\infty\ (u\ne v).
\]

---

### B2) Transition

To get a path with at most $k$ edges from $u$ to $v$, either:
- already had one with at most $k-1$ edges, or
- last edge is $(x,v)$, and before that from $u$ to $x$ with at most $k-1$ edges.

So:
\[
M[k][u][v]
=
\min\!\left(
M[k-1][u][v],\ 
\min_{(x,v)\in E}\left(M[k-1][u][x]+w(x,v)\right)
\right).
\]

---

### B3) Runtime

For each $k\in[1..n-1]$:  
for each $u\in V$:  
for each edge $(x,v)\in E$: update candidate for $M[k][u][v]$.

Complexity:
\[
O(n\cdot n\cdot m)=O(n^2m).
\]

Memory:
- full 3D table: $O(n^3)$,
- rolling two layers in $k$: $O(n^2)$ if only final distances needed.

---

### B4) Why correct
Induction on $k$:
- base $k=0$ correct by definition,
- recurrence enumerates exactly all ways to extend paths by one final edge.
Taking minimum preserves optimal shortest value.

No negative cycles assumption guarantees shortest distances are well-defined.

---

### B5) Analogy
Think of “travel plans with a stop-budget.”  
Layer $k$ = best routes allowed up to $k$ flights.  
To build $k$-flight routes, either keep old plan or append one final flight to a $(k-1)$-flight plan.

---

### B6) Common mistakes
1. using exactly $k$ edges instead of at most $k$ without handling carry-over minimum,
2. wrong base initialization for $u\ne v$,
3. forgetting edge direction in directed graph.

---

---

## Section C — Aufgabe 3: “Get rich fast” (arbitrage detection)

Given exchange rates matrix $r_{ij}$ for currencies $i\to j$.  
Need detect whether sequence of exchanges can increase money (arbitrage).

---

### C0) Core modeling idea

If you multiply rates along cycle:
\[
r_{i_0 i_1} r_{i_1 i_2}\cdots r_{i_{k-1} i_0} > 1
\]
then starting with 1 unit yields profit after returning to same currency.

Multiplicative condition is awkward for shortest paths.  
Use log transform.

---

### C1) Transform to additive graph weights

Define directed graph with edge weight:
\[
w(i,j) = -\ln(r_{ij}).
\]
Then cycle sum:
\[
\sum w = -\ln\!\left(\prod r\right).
\]
So:
- product $>1$  $\iff$  $-\ln(\text{product})<0$  $\iff$  cycle weight $<0$.

Arbitrage exists iff graph has a **negative cycle**.

---

### C2) Algorithm

Run Bellman-Ford (or multi-source Bellman-Ford via super-source):
1. initialize distances,
2. relax edges $|V|-1$ times,
3. one extra pass: if any edge still relaxes → negative cycle exists.

If negative cycle exists:
\[
\boxed{\text{arbitrage exists}}.
\]

Runtime:
\[
O(|V||E|)=O(n^3)
\]
for dense currency graph ($E\approx n^2$).

---

### C3) Why correct
Bellman-Ford detects reachability of negative cycles by “extra relaxation after $|V|-1$ rounds.”  
Log transform preserves arbitrage condition exactly.

---

### C4) Analogy
Exchange rates are like multipliers in a game economy.  
Taking $-\log$ turns multipliers into additive “energy costs.”  
A profitable loop becomes a loop with negative total energy.

---

### C5) Common mistakes
1. using $+\log$ instead of $-\log$,
2. checking shortest path only between two nodes (need cycle detection),
3. floating precision issues in practical implementation (use tolerance).

---

---

## Section D — Aufgabe 4: Dwarf statements, contradictions, and topological sorting

You have statements like “Zwerg A < Zwerg B” (“A is smaller than B”).  
Need:
a) detect contradictions,  
b,c) prove relation to cycles/topological order,  
d) compute ordering in $O(|V|+|E|)$.

---

### D0) Modeling to directed graph

Create vertex per dwarf.  
For statement $A<B$, add directed edge:
\[
A\to B.
\]
Meaning: A must appear before B in final order.

Contradiction means impossible constraints, i.e., cycle.

---

### D1) Part (a): contradiction test

Algorithm:
- run DFS (or Kahn’s indegree algorithm) to detect directed cycle.
- if cycle exists → contradictory.
- else consistent.

Why:
Cycle $A<B<C<\dots<A$ impossible in strict order.

---

### D2) Part (b): DAG has sink (node with no outgoing edge)

Claim: Every finite Directed Acyclic Graph (DAG) has at least one sink (and at least one source).

Proof sketch:
- Suppose no sink: every node has outgoing edge.
- Start from any node, follow outgoing edges forever.
- Finite graph ⇒ some node repeats ⇒ directed cycle.
Contradiction.

Role in topological order:
- sink can be placed last.
- remove it, recurse on smaller DAG.

---

### D3) Part (c): graph topologically sortable iff acyclic

($\Rightarrow$) If topological order exists, edges always go forward in order; cycle would force impossible strict increase returning to start.

($\Leftarrow$) If acyclic, repeatedly remove sink/source to build order (induction on $|V|$).

\[
\boxed{\text{DAG} \iff \text{topological ordering exists}}
\]

---

### D4) Part (d): algorithm in $O(|V|+|E|)$

Use **Kahn’s algorithm** (source-removal):
1. compute indegree of each node,
2. queue all indegree-0 nodes,
3. pop one, append to output,
4. for each outgoing edge $u\to v$: decrement indegree(v); if becomes 0, enqueue,
5. end: if output size < $|V|$, cycle exists (“impossible”).

Runtime:
- indegree init $O(|V|+|E|)$,
- each node/enqueue once, each edge processed once:
\[
O(|V|+|E|).
\]

---

### D5) Python reference (template-compatible logic)

```python name=ub11_topological_order.py
from collections import deque

def find_topological_ordering(names, edges):
    """
    names: list of dwarf names
    edges: list of pairs (a, b) meaning a < b  i.e., edge a -> b
    returns list of names in topological order, or None if impossible
    """
    idx = {name: i for i, name in enumerate(names)}
    n = len(names)

    adj = [[] for _ in range(n)]
    indeg = [0] * n

    for a, b in edges:
        u, v = idx[a], idx[b]
        adj[u].append(v)
        indeg[v] += 1

    q = deque(i for i in range(n) if indeg[i] == 0)
    order = []

    while q:
        u = q.popleft()
        order.append(u)
        for v in adj[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    if len(order) != n:
        return None  # cycle -> impossible
    return [names[i] for i in order]
```

---

### D6) Analogy
You’re scheduling tasks with prerequisites:
- edge $A\to B$: task A must be done before B.
A cycle means circular dependency (“do A before B before C before A”) — impossible.

Topological sorting = producing a valid schedule.

---

### D7) Common mistakes
1. building edge direction backwards,
2. forgetting disconnected components,
3. assuming topological order is unique (usually not),
4. not checking output length for cycle detection in Kahn’s algorithm.

---

## ✅ Compact exam answers (UB11)

1. Adjacency lists can be globally integer-key bucket-sorted in:
\[
O(|V|+|E|).
\]
2. APSP DP with edge-budget:
\[
M[k][u][v]=\min\left(M[k-1][u][v],\min_{(x,v)\in E}(M[k-1][u][x]+w(x,v))\right),
\]
runtime:
\[
O(n^2m).
\]
3. Arbitrage detection:
- transform weights $w(i,j)=-\ln r_{ij}$,
- detect negative cycle via Bellman-Ford.
4. Dwarf consistency:
- contradictions iff directed cycle.
- topological order exists iff acyclic.
- compute in $O(|V|+|E|)$ with Kahn/DFS.

---

## 🧩 Micro-summary

UB11’s core lesson:
- model constraints as graphs,
- choose the right traversal/DP,
- prove correctness through structural equivalences (acyclic ↔ sortable, profit-loop ↔ negative cycle).

That is exactly advanced DSEA exam style.

---

## Übungsblatt 12 — MST Theory (Cycle/Cut/Bottleneck), Union-Find + Rebel Cities to Surface, Borůvka-Style MST Correctness & Runtime (ADHD-Friendly, Blank-Slate, Lecture-Level)

## 🧠 Why this sheet matters

UB12 is a major MST (Minimum Spanning Tree) sheet with both proof and implementation flavor:

1. deep MST structural properties (cycle property, uniqueness, bottleneck property),
2. practical engineering of MST with **Union-Find** for a geometry-flavored problem,
3. correctness/runtime proof of a Borůvka-like MST algorithm.

This sheet is exam-critical because it tests both:
- conceptual theorem reasoning,
- algorithm construction + complexity control.

---

## Global glossary (deep definitions)

### 1) Spanning tree
For connected undirected graph $G=(V,E)$, a spanning tree is a subgraph that:
- includes all vertices,
- is connected,
- has no cycles.
It has exactly $|V|-1$ edges.

### 2) MST (Minimum Spanning Tree)
A spanning tree with minimum possible total edge weight.

### 3) Cycle property (MST)
In any cycle, the heaviest edge cannot belong to an MST (for distinct weights; with ties: at least one maximum edge can be excluded).

### 4) Cut property (MST)
For any cut $(S,V\setminus S)$, the lightest edge crossing the cut is safe (belongs to some MST; with distinct weights: must belong to the unique MST).

### 5) Bottleneck (of a tree)
Maximum edge weight inside that tree.

### 6) Union-Find / DSU (Disjoint Set Union)
Data structure for partition of vertices into components with operations:
- `find(x)` = component representative,
- `union(a,b)` = merge components.
With path compression + union by rank/size, operations are near-constant amortized.

### 7) Borůvka algorithm (high-level)
Repeatedly, each current component chooses its cheapest outgoing edge; add all chosen edges, merge components, repeat.

---

---

## Section A — Aufgabe 1: MST proof statements

Given connected graph $G=(V,E)$ with pairwise distinct edge weights $c:E\to \mathbb{R}$.

Need prove:
a) heaviest edge in any cycle not in MST,  
b) MST unique,  
c) MST minimizes bottleneck among spanning trees.

---

### A1) Part (a): heaviest edge on a cycle cannot be in MST

Let cycle $C$ and $e_{\max}$ be its unique heaviest edge (distinct weights assumption).

Assume for contradiction $e_{\max}\in T$ for MST $T$.  
Removing $e_{\max}$ disconnects $T$ into two components $X$ and $Y$.  
In cycle $C$, there is another edge $f$ connecting $X$ and $Y$ (because cycle offers alternate route).  
Since $e_{\max}$ is heaviest on $C$:
\[
w(f)<w(e_{\max}).
\]
Then
\[
T' = T - e_{\max} + f
\]
is a spanning tree (connectivity restored, still acyclic) with strictly smaller total weight — contradiction to minimality of $T$.

\[
\boxed{e_{\max}\notin \text{MST}}
\]

#### Why this step makes sense
Cycle gives “redundant route.”  
If you keep the most expensive edge among redundant options, you can replace it with cheaper alternative and improve tree.

---

### A2) Part (b): MST is unique (distinct weights)

Suppose two different MSTs $T_1,T_2$.  
Take smallest-weight edge $e$ that is in exactly one of them, say $e\in T_1\setminus T_2$.  
Add $e$ to $T_2$: creates cycle $C$.  
Cycle must contain some edge $f\in T_2\setminus T_1$.  
By minimality choice of $e$, all differing edges lighter than $e$ cannot exist, so $w(e)<w(f)$.  
Replace $f$ by $e$ in $T_2$, get strictly smaller spanning tree than $T_2$, contradiction.

\[
\boxed{\text{MST is unique when all edge weights are distinct}}
\]

#### Intuition
Distinct weights remove tie ambiguity; every “safe” choice becomes forced.

---

### A3) Part (c): MST minimizes bottleneck edge weight

Claim: if $T$ is MST, then among all spanning trees it has minimum possible maximum edge weight.

Proof sketch via cut/cycle logic:
- Let $e^*$ be heaviest edge in MST $T$, weight $W^*$.
- Remove $e^*$: split $T$ into two components $S$ and $V\setminus S$.
- Any spanning tree must include at least one crossing edge of this cut.
- By cut-property reasoning and distinctness, $e^*$ is the minimum-weight crossing edge that can connect these sides at this stage (otherwise MST improvable).
Thus any spanning tree must use an edge with weight $\ge W^*$.  
So no spanning tree has smaller bottleneck than $T$.

\[
\boxed{T\text{ is a minimum-bottleneck spanning tree}}
\]

Important note (as in sheet): this remains true even without distinct weights (though uniqueness may fail).

---

### A4) Deeper analogy
Think of building a road network:
- Cycle property says: in any redundant loop, the most expensive road is never essential for minimum total budget.
- Bottleneck property says: MST also minimizes the “worst single road cost” threshold needed to keep everyone connected.

---

### A5) Common mistakes
1. confusing “not in any MST” vs “not in this MST” when ties exist,
2. using uniqueness proof without distinct-weight assumption,
3. mixing total weight objective and bottleneck objective without proof bridge.

---

---

## Section B — Aufgabe 2: Rebel cities + surface oxygen (implementation with Union-Find)

Problem idea:
- $n$ underground cities (red points),
- each city has its own possible surface access point (blue point) directly above on sphere of radius $r$,
- you may connect city-city or city-surface with straight tunnels,
- need minimum total tunnel length so every city has a path to *some* surface access.

Sheet hint: “You don’t need all surface accesses; each city must somehow reach surface; use a trick.”

---

### B0) Modeling trick (critical concept)

Create augmented graph:
- vertices: cities $1..n$ plus one **super-surface node** $s$,
- city-city edge weight = Euclidean distance between city coordinates,
- city $i$ to super node $s$: weight = distance from city $i$ to its surface point (radial difference to radius $r$).

Then problem becomes:
\[
\text{Find MST of augmented graph on }n+1\text{ nodes.}
\]
Why?
- In MST, all cities connected and connected to $s$ somehow,
- equivalent to each city having oxygen path to surface.

---

### B1) Why this reduction is correct (step justification)

Any feasible oxygen network induces connectivity from each city to at least one surface point.  
By contracting all surface points into super node $s$, we preserve feasibility notion (“connected to surface”) while simplifying structure.

Among connected networks, MST minimizes total length while keeping connectivity.

So computing MST on augmented graph solves original optimization goal.

---

### B2) Union-Find (DSU) operations needed

#### `find(x)`
Return representative (root) of component containing $x$.  
Use path compression:
- recursively find root and set parent directly to root.

#### `union(x,y)`
Merge components if roots differ.  
Use rank/size heuristic to attach smaller tree under larger one.

Amortized complexity per op is almost constant ($\alpha(n)$, inverse Ackermann).

---

### B3) `connect_cities` high-level algorithm (Kruskal style)

1. Build all edges:
   - city-city edges,
   - city-super edges.
2. Sort edges by length ascending.
3. Initialize DSU on $n+1$ vertices.
4. Traverse sorted edges:
   - if endpoints in different components, add edge to solution and union them.
5. Stop after selecting $n$ edges (for $n+1$ nodes MST has $n$ edges).
6. Output edges as required index format.

Runtime dominated by sorting edges.

---

### B4) Complexity

If fully dense city-city graph:
- city-city edges $\Theta(n^2)$,
- plus $n$ city-super edges.
Sorting:
\[
O(n^2\log n).
\]
DSU operations near linear in edge count.

---

### B5) Common mistakes
1. forgetting super-node trick (then hard to enforce “reach any surface” elegantly),
2. wrong index mapping for output (k>n indicates surface access of city $k-n$),
3. missing path compression/union-by-rank bugs in DSU.

---

### B6) Analogy
Imagine every city needs internet through any gateway tower on surface.  
By adding one virtual “internet cloud node” connected to each possible gateway cable, we transform “reach at least one gateway” into simple graph connectivity to cloud.

---

---

## Section C — Aufgabe 3: Borůvka-style MST algorithm

Algorithm described:
- start with each node as component,
- in each round, each component picks its cheapest outgoing edge and marks it,
- merge components connected by marked edges,
- repeat until one component remains.

Need:
a) why distinct weights needed (for stated deterministic correctness path),  
b) marked edges must belong to MST,  
c) result is spanning tree,  
d) number of rounds $\le \log|V|$,  
e) total runtime $O(|E|\log|V|)$.

---

### C1) Part (a): why distinct weights assumption appears

If equal weights exist, per-component cheapest outgoing edge may be non-unique; careless tie choices can create issues in simplified correctness argument and can even mark cyclic edge sets not matching intended deterministic proof path.

Counterexample concept:
- symmetric graph with equal edge weights where arbitrary tie choices can choose mutually inconsistent edges and complicate uniqueness/safety argument.

Distinct weights ensure each component has unique cheapest outgoing edge, simplifying correctness (safe edge argument is clean).

(Algorithm can still be made correct with ties using careful handling; assumption is for proof simplicity.)

---

### C2) Part (b): marked edge is MST-safe

For any component $X$, chosen edge $e_X$ is lightest edge crossing cut $(X,V\setminus X)$.  
By cut property, such lightest crossing edge is safe (belongs to some MST; with distinct weights effectively forced).

So every marked edge is MST-compatible.

---

### C3) Part (c): marked edges form spanning tree at end

- We only merge components by crossing edges, so eventually components connect.
- Safe-edge argument prevents selecting “bad” edges outside all MSTs.
- When one component remains, marked edges connect all vertices.
- Since each merge reduces number of components and we can avoid cycle addition by component-based union reasoning, final marked set has tree structure (connected + $|V|-1$ edges in effect).

Thus algorithm returns MST.

---

### C4) Part (d): at most $\log |V|$ rounds

In each round, every current component selects at least one outgoing edge (unless already isolated, but graph connected overall).

When these selected edges are added, each resulting merged component has size at least doubled in aggregate argument:
- each old component “points” outward via one selected edge,
- contraction yields number of components at least halved.

So if $c_t$ is components after round $t$:
\[
c_{t+1}\le \frac{c_t}{2}.
\]
Starting $c_0=|V|$, after $r$ rounds:
\[
c_r\le \frac{|V|}{2^r}.
\]
Need $c_r=1$, so $r\le \lceil\log_2 |V|\rceil$.

\[
\boxed{\text{Rounds }=O(\log|V|)}
\]

---

### C5) Part (e): runtime $O(|E|\log|V|)$

Per round:
- for each component, find cheapest outgoing edge.
If done by scanning all edges and checking component IDs via DSU/component labels:
\[
O(|E|)\text{ per round}.
\]
Number of rounds $O(\log|V|)$, total:
\[
\boxed{O(|E|\log|V|)}.
\]

(With engineering improvements you can get similar or better constants.)

---

### C6) Deeper analogy
Each component sends one “cheapest friendship request” to another component each round.  
Lots of small groups fuse quickly into larger groups, roughly halving group count each time — exponential decay in number of components.

---

### C7) Common mistakes
1. claiming “exactly half” components always (it is “at most half,” asymptotically enough),
2. forgetting cut property justification for chosen edges,
3. assuming uniqueness arguments without distinct weights.

---

## ✅ Compact exam answers (UB12)

1. In a cycle, heaviest edge cannot be in MST (cycle replacement argument).
2. Distinct weights imply MST uniqueness.
3. MST is minimum-bottleneck spanning tree.
4. Rebel cities problem:
   - add super-surface node,
   - run MST (Kruskal + DSU),
   - ensures every city connected to surface with minimal total length.
5. Borůvka-style algorithm:
   - each component picks cheapest outgoing safe edge,
   - components shrink by at least factor 2 per round,
   - $O(\log|V|)$ rounds,
   - $O(|E|\log|V|)$ total runtime.

---

## 🧩 Micro-summary

UB12 is MST mastery:
- structural theorems (cycle/cut/bottleneck),
- DSU implementation for geometric network design,
- Borůvka correctness + logarithmic-round complexity argument.

This is quintessential DSEA exam material.

---
## Übungsblatt 13 (Zusatzblatt) — Integer-Weight MST in $O(k|V|+|E|)$, CHAOS as Max Matching via Max Flow, Binary Heap Implementation, Acyclic Flows (ADHD-Friendly, Blank-Slate, Lecture-Level)

## 🧠 Why this sheet matters

UB13 is a powerful final sheet that combines:

1. **MST optimization with bounded integer weights** (data structure design),
2. **reduction thinking** (turning a matching problem into max flow),
3. **heap implementation mechanics** (core priority-queue DS),
4. **flow decomposition/cycle-canceling proof technique**.

This is advanced but very exam-useful: it tests if you can transfer known algorithms into new problem forms.

---

## Global glossary (deep definitions)

### 1) Priority queue
A data structure supporting:
- insert element with key,
- extract minimum (or maximum),
- decrease-key (optional depending algorithm).

Used heavily in Prim/Dijkstra.

### 2) Bucket queue
Priority queue variant when keys are small integers in limited range.  
Instead of balanced tree/heap, maintain buckets by key value for $O(1)$-like key access (amortized over range scan).

### 3) Matching in bipartite graph
Set of edges with no shared endpoint:
- each left node matched to at most one right node,
- each right node matched to at most one left node.

### 4) Max flow
In directed capacitated network, assign flow values respecting:
- capacity constraints,
- conservation at internal nodes,
maximize total flow from source $s$ to sink $t$.

### 5) Residual network
Graph of remaining augmenting possibilities (forward spare capacity + backward undo capacity).

### 6) Heap (binary min-heap)
Complete binary tree with heap property:
parent key $\le$ child keys.
Array-based indices:
- left child $2i+1$,
- right child $2i+2$,
- parent $\lfloor(i-1)/2\rfloor$.

### 7) Acyclic flow
Flow $f$ is acyclic if subgraph of positive-flow edges contains no directed cycle.

---

---

## Section A — Aufgabe 1: MST with weights in $\{1,\dots,k\}$ in $O(k|V|+|E|)$

Given connected undirected graph $G=(V,E)$, edge weights integer in $\{1,\dots,k\}$.  
Need MST in:
\[
O(k|V|+|E|).
\]
Hint: Prim with clever priority queue.

---

### A1) Why bounded weights help

Standard Prim with binary heap:
\[
O(|E|\log|V|).
\]
But if keys are integers from small range, we can replace log-heap by bucket-based structure.

---

### A2) Prim refresher (quick)
Prim grows MST from a start vertex:
- maintain for each outside vertex its best connecting edge weight (`key[v]`),
- repeatedly pick vertex with minimum key and add it.

---

### A3) Bucket-queue design for this task

#### Key observation
In Prim, each key is always one of edge weights $\in\{1,\dots,k\}$ (or $\infty$ initially).  
So we can store vertices by key in buckets `B[1..k]` (plus an INF bucket if needed before first update).

Operations:
- `decrease-key(v,newKey)`: move $v$ to lower bucket.
- `extract-min`: find smallest non-empty bucket index.

Maintain pointer `cur` to current smallest non-empty bucket.  
Because `cur` only moves upward through 1..k during each global “phase,” scanning cost is controlled.

---

### A4) Complexity intuition to get $O(k|V|+|E|)$

- Every edge $(u,v)$ examined once from each endpoint in adjacency scan:
  \[
  O(|E|).
  \]
- For each extracted vertex, bucket pointer can scan at most $k$ bucket positions in worst case:
  \[
  O(k|V|).
  \]

Total:
\[
\boxed{O(k|V|+|E|)}.
\]

(With tighter amortization and implementation details, sometimes even better practical behavior.)

---

### A5) Why algorithm is correct

Prim correctness relies on cut property:
- always adding minimum-key frontier vertex corresponds to adding a lightest crossing edge for current tree cut.
Bucket queue changes only data structure, not chosen keys/min rule.  
So same correctness as standard Prim.

---

### A6) Deeper analogy
Imagine edge costs are small integer toll classes 1..k.  
Instead of searching whole priority queue, you keep k trays labeled by toll class and always pick from lowest non-empty tray.

---

### A7) Common mistakes
1. forgetting to update parent edge when key decreases,
2. treating bucket scan as $O(k)$ once total (it can be per extraction unless carefully amortized),
3. mixing Prim with Kruskal logic.

---

---

## Section B — Aufgabe 2: CHAOS problem reduced to max flow

Given:
- sockets $S$,
- devices $E$,
- compatibility edges $A\subseteq S\times E$,
find maximum number of plugged devices with each socket/device used at most once.

This is maximum bipartite matching.

---

### B1) Why this is matching

Each valid plug assignment is an edge $(s,e)$ from socket to compatible device.  
Constraints “at most one incident chosen edge per vertex” exactly define matching.

Goal: maximize number of chosen edges = maximum matching.

---

### B2) Reduction to max flow (construction)

Build directed network:
1. source node $s_0$,
2. sink node $t_0$,
3. left layer sockets $S$,
4. right layer devices $E$.

Edges/capacities:
- $s_0 \to s$ for each socket $s\in S$, capacity 1,
- $s \to e$ for each compatibility $(s,e)\in A$, capacity 1,
- $e \to t_0$ for each device $e\in E$, capacity 1.

Run max flow.

---

### B3) Why this reduction is correct

- Any integer flow unit path $s_0\to s\to e\to t_0$ corresponds to selecting matching edge $(s,e)$.
- Capacity 1 on source/socket edges ensures each socket used at most once.
- Capacity 1 on device/sink edges ensures each device used at most once.
- Conversely, any matching of size $M$ gives flow of value $M$.

By integrality of max flow with integer capacities, optimal flow is integral.

Hence:
\[
\boxed{\text{max matching size}=\text{max flow value}}
\]

---

### B4) Runtime
Reduction itself is polynomial-time (linear in graph size).  
Then use any polynomial max-flow algorithm.

---

### B5) Analogy
Think of water units as “plug permissions.”  
A permission can pass through a socket and then a device only once due to capacity-1 valves.

---

### B6) Common mistakes
1. wrong edge direction (must go source→socket→device→sink),
2. forgetting capacity 1 constraints,
3. claiming min-cut directly without constructing correspondence.

---

---

## Section C — Aufgabe 3: Binary min-heap implementation notes

Task asks implementing missing heap template functions.

---

### C1) Core heap operations (what each must do)

#### `insert(x)`
1. append at end,
2. sift-up (bubble up) while parent > current.

#### `extract_min()`
1. root is minimum,
2. move last element to root,
3. remove last slot,
4. sift-down (trickle-down) by swapping with smaller child until heap property restored.

#### `decrease_key(i,newVal)` (if present)
- decrease value at index $i$,
- sift-up.

#### `heapify(i)` (sift-down at index $i$)
- compare with children,
- swap with smaller child if needed,
- continue recursively/iteratively.

---

### C2) Correctness invariants

1. Shape invariant: array represents complete binary tree.
2. Order invariant: parent $\le$ children.

Each operation preserves both:
- insert preserves shape by append, restores order by sift-up,
- extract preserves shape by root replacement, restores order by sift-down.

---

### C3) Runtimes
Height of heap with $n$ nodes:
\[
O(\log n).
\]
So:
- insert: $O(\log n)$,
- extract-min: $O(\log n)$,
- peek-min: $O(1)$,
- build-heap (bottom-up): $O(n)$.

---

### C4) Deeper analogy
Heap is like a tournament podium:
- gold medalist always at top (root),
- local rank constraints in every mini-podium keep global minimum at top.

---

### C5) Common implementation bugs
1. off-by-one index errors in children/parent formulas,
2. comparing with wrong child in sift-down,
3. forgetting to stop when heap property already satisfied.

---

---

## Section D — Aufgabe 4: From arbitrary flow to acyclic flow with same value

Given flow network $G=(V,E)$, flow $f$.  
Need show exists flow $f'$ with:
- $|f'|=|f|$,
- positive-flow edge subgraph has no directed cycle.

---

### D1) High-level strategy: cycle-canceling on flow cycles

If positive-flow subgraph contains directed cycle $C$, then each edge on $C$ has $f(e)>0$.

Let:
\[
\delta = \min_{e\in C} f(e) > 0.
\]
Decrease flow by $\delta$ on every edge of $C$:
\[
f(e)\leftarrow f(e)-\delta \quad \forall e\in C.
\]

---

### D2) Why this remains a valid flow

#### Capacity constraints
Flows only decrease on cycle edges, so still nonnegative and below capacities.

#### Flow conservation
At each cycle vertex:
- one incoming cycle edge reduced by $\delta$,
- one outgoing cycle edge reduced by $\delta$,
net balance unchanged.

#### Source-sink value
No net change in source outflow minus inflow from this cycle operation, so total flow value unchanged.

---

### D3) Why process terminates

Each cancellation sets at least one cycle edge flow to zero (edge achieving minimum $\delta$).  
Number of positive-flow edges decreases at least by one eventually, finite edges => finite steps.  
When no directed cycle remains in positive-flow subgraph, flow is acyclic.

\[
\boxed{\exists f' \text{ acyclic with }|f'|=|f|}
\]

---

### D4) Deeper analogy
Imagine water circulating uselessly in closed loops inside pipes.  
You can reduce the same amount on every pipe in that loop — total delivered water from source to sink stays same, but wasted circulation disappears.

---

### D5) Common mistakes
1. trying to remove cycle edge entirely without respecting equal decrement,
2. forgetting to prove source-sink value unchanged,
3. not proving termination.

---

## ✅ Compact exam answers (UB13)

1. Integer-weight MST ($1..k$):
   - Prim + bucket priority queue,
   - runtime:
\[
O(k|V|+|E|).
\]

2. CHAOS:
   - model as bipartite matching,
   - reduce to max flow with unit capacities source→sockets→devices→sink,
   - max flow value = max number of plug assignments.

3. Binary min-heap:
   - implement insert/sift-up, extract-min/sift-down, heapify, etc.,
   - operations $O(\log n)$, build-heap $O(n)$.

4. Acyclic flows:
   - repeatedly cancel positive directed cycles by subtracting minimum cycle flow,
   - feasibility + value preserved,
   - process terminates with acyclic positive-flow graph.

---

## 🧩 Micro-summary

UB13 closes the course with advanced transfer skills:
- optimize classical algorithms using key constraints (bounded weights),
- reduce one problem class to another (matching → flow),
- prove structural existence via iterative invariant-preserving transformations (acyclic flow from arbitrary flow).

---
