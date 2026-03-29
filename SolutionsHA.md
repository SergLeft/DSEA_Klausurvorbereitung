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
   - inequality \(2^n > n\).

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
Now one university may hold up to \(c_i\) students.
This is the **hospital/residents** version of stable matching.

#### 4) Why induction?
Induction is the “domino proof”:
- prove first domino falls (base),
- prove each falling domino pushes next (step),
- then all fall.

---

## 🧠 Aufgabe 1 — University admissions with capacities

### Problem restated
- \(m\) universities with capacities \(c_1,\dots,c_m\),
- \(n\) applicants, \(n \ge \sum_i c_i\),
- strict preferences on both sides,
- we require all seats filled, and no blocking under conditions (i) and (ii).

### Key insight
This is exactly the many-to-one stable matching model (college admissions / hospital-residents).  
A stable assignment **always exists**.

### Efficient algorithm (applicant-proposing deferred acceptance, capacity version)

1. Initially all applicants free, all university slots empty.
2. While there is a free applicant with remaining universities on their list:
   - applicant proposes to best university not yet proposed to.
   - university tentatively keeps the best \(c_i\) proposers seen so far (according to its preference), rejects the rest.
3. Rejected applicants become free again and continue proposing.
4. Stop when no proposal possible.

Because \(n \ge \sum c_i\), and with complete preference lists, all university slots can be filled.

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
(plus data-structure factors for maintaining current top \(c_i\), still polynomial and efficient).

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

People: \(p_1,p_2,p_3,p_4\)

Preferences (best to worst from left):
- \(p_1: p_4 \succ p_2 \succ p_3\)
- \(p_2: p_4 \succ p_3 \succ p_1\)
- \(p_3: p_4 \succ p_1 \succ p_2\)
- \(p_4: p_3 \succ p_1 \succ p_2\)

Possible perfect pairings for 4 people:
1. \(\{(p_1,p_2),(p_3,p_4)\}\)
2. \(\{(p_1,p_3),(p_2,p_4)\}\)
3. \(\{(p_1,p_4),(p_2,p_3)\}\)

Check stability quickly:

- Pairing 1: \(p_1\) and \(p_4\) prefer each other over current partners ⇒ blocking.
- Pairing 2: \(p_3\) and \(p_4\) prefer each other over current partners ⇒ blocking.
- Pairing 3: \(p_3\) and \(p_4\) again prefer each other over current partners ⇒ blocking.

So no stable pairing exists in this instance.

\[
\boxed{\text{No stable pairing exists for the given preferences.}}
\]

---

## 🧠 Aufgabe 4 — Induction proofs

### (a) \(\sum_{k=1}^{n} k = \frac{n(n+1)}{2}\)

- Base \(n=1\): \(1 = \frac{1\cdot2}{2}\).
- Step: assume true for \(n\):
  \[
  \sum_{k=1}^{n+1}k=\left(\sum_{k=1}^{n}k\right)+(n+1)=\frac{n(n+1)}{2}+(n+1)=\frac{(n+1)(n+2)}{2}.
  \]
Done.

---

### (b) \(n^5-n\) divisible by 5 for all \(n\in\mathbb N_0\)

Use factorization:
\[
n^5-n=n(n^4-1)=n(n^2-1)(n^2+1)=n(n-1)(n+1)(n^2+1).
\]
Among three consecutive integers \(n-1,n,n+1\), one is divisible by 5? Not always.  
So better modular route:
\[
n^5 \equiv n \pmod 5
\]
(by Fermat or direct residue check \(n=0,1,2,3,4\)). Hence
\[
n^5-n \equiv 0 \pmod 5.
\]
Divisible by 5.

---

### (c) \(2^n > n\) for all \(n\in\mathbb N_0\)?

As written, this is **false for \(n=1\)** since \(2^1=1\) is not greater.

Corrected valid statement:
- \(2^n \ge n\) for all \(n\in\mathbb N_0\), and
- \(2^n > n\) for all \(n\ge 2\).

Induction for \(n\ge2\):
- Base \(n=2\): \(4>2\).
- Step: assume \(2^n>n\) (\(n\ge2\)):
  \[
  2^{n+1}=2\cdot2^n>2n\ge n+1.
  \]
Done.

---

### ⚠️ Pitfalls (UB1)
1. Forgetting capacity version is many-to-one deferred acceptance.
2. In Aufgabe 2, overlooking the “mutual first-choice lock”.
3. In Aufgabe 3, checking only one pairing.
4. In induction (c), missing the false case \(n=1\).

---

### 🧩 UB1 micro-summary
- Capacity-stable assignment: always exists, computed by deferred acceptance in polynomial time.
- Triton cannot separate Eric/Arielle.
- Given 4-person roommate instance has no stable pairing.
- Induction: (a) and (b) true; (c) needs corrected statement.

---

## ✅ UB1 compact exam answers

1. Stable assignment with capacities exists; applicant-proposing deferred acceptance computes one in \(O(nm)\).
2. No, Triton cannot prevent Arielle/Eric pairing.
3. Given \(n=2\) instance: no stable pairing.
4. (a) and (b) proven; (c) as stated false at \(n=1\), true for \(n\ge2\).

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
4. Show that combining two stable matchings via applicant-wise better assignment \(X\vee Y\) remains stable.

---

### 📚 Zero-to-functional foundations

#### 1) What are Landau symbols?
- \(O(g)\): asymptotic upper bound.
- \(\Omega(g)\): asymptotic lower bound.
- \(\Theta(g)\): both bounds.

#### 2) What is asymptotic proof style?
You compare growth for large \(n\), ignoring constants and low-order terms.

#### 3) Selection Sort core
Repeatedly pick minimum of unsorted suffix and swap to front.

#### 4) Why “\(X\vee Y\)” in stable matching?
You compare two stable outcomes applicant-by-applicant and keep preferred assignment per applicant.

---

## 🧠 Aufgabe 1 — Landau symbols

### (a) \(100n^2 \in O(n^3)\)
\[
\frac{100n^2}{n^3}=\frac{100}{n}\to 0
\]
So yes.

### (b) \(n^2 \in \Omega\!\left(\frac{2n^2+4n+2}{n+1}\right)\)

Simplify RHS:
\[
\frac{2n^2+4n+2}{n+1}=2n+2.
\]
Need \(n^2 \in \Omega(n)\), true.

### (c) \(\log_b n \in \Theta(\ln n)\), fixed \(b>1\)
\[
\log_b n = \frac{\ln n}{\ln b},
\]
constant factor only ⇒ same \(\Theta\)-class.

### (d) \(g\in O(f)\iff f\in\Omega(g)\)
This is exactly the definition duality.

### (e) \(\sum_{i=1}^n \frac{1}{i}\in O(\log n)\)
Harmonic sum \(H_n\le 1+\ln n\), so yes.

### (f) If \(\limsup_{k\to\infty}\frac{g(k)}{f(k)}<\infty\), then \(g=O(f)\) (with \(f(n)\neq0\))
Bounded limsup means ratio eventually bounded by some constant \(C\), which is definition of \(O\).

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

### (a) Example on \([5,3,4,2,1]\)
- Pass 1: min=1, swap with first → \([1,3,4,2,5]\)
- Pass 2: min in suffix \([3,4,2,5]\) is 2 → \([1,2,4,3,5]\)
- Pass 3: min in \([4,3,5]\) is 3 → \([1,2,3,4,5]\)
- Pass 4: min in \([4,5]\) is 4 (no change)

### (b) Correctness invariant
After pass \(i\), first \(i\) elements are the \(i\) smallest elements in sorted order.
Initialization true for \(i=0\).  
Maintenance by selecting minimum of remainder and placing at position \(i+1\).  
Termination gives full sorted array.

### (c) Number of comparisons
\[
(n-1)+(n-2)+\cdots+1=\frac{n(n-1)}{2}=\Theta(n^2).
\]

### (d) Min+max modification
Still \(\Theta(n^2)\) comparisons asymptotically. Constant factor may improve, class does not.

---

## 🧠 Aufgabe 4 — Combine stable matchings: \(X\vee Y\)

Definition:
\[
X\vee Y:=\{(p,\max_p(X(p),Y(p)))\mid p\in P\},
\]
i.e., each applicant gets the preferred of their two assignments.

### Claim
\(X\vee Y\) is stable.

### Proof idea (standard lattice argument sketch)
Assume \(X\vee Y\) unstable with blocking pair \((p,s)\).  
Then \(p\) prefers \(s\) over \((X\vee Y)(p)\), hence over both \(X(p)\) and \(Y(p)\) or at least over the better one.  
Using stability of \(X\) and \(Y\), this forces incompatible preference relations at \(s\) with the partners assigned in \(X\) and \(Y\).  
One derives a contradiction to at least one stability condition in \(X\) or \(Y\).  
Therefore no blocking pair exists in \(X\vee Y\).

So:
\[
\boxed{X\vee Y\text{ is stable.}}
\]

---

### ⚠️ Pitfalls (UB2)
1. Mixing up \(O\) and \(\Omega\) directions.
2. Forgetting harmonic sum growth is logarithmic.
3. Claiming modified Selection Sort becomes \(O(n\log n)\) (false).
4. For Aufgabe 4, proving informally without contradiction structure.

---

### 🧩 UB2 micro-summary
- Landau statements all hold.
- Naive Fibonacci recursion is exponential.
- Selection Sort: correct via invariant, \(\Theta(n^2)\) comparisons.
- \(X\vee Y\) preserves stability.

---

## ✅ UB2 compact exam answers

1. (a)–(f): all true with standard asymptotic arguments.
2. Naive Fibonacci recursion has exponential runtime \(\Theta(\varphi^n)\).
3. Selection Sort comparisons: \(\Theta(n^2)\); min+max variant remains \(\Theta(n^2)\).
4. \(X\vee Y\) is stable.

---
## 🔁 Final Retrofitted Replacement — Übungsblatt 3 (Operational, Blank-Slate, ADHD-Oriented)

## 🧩 Topic 1: Inversionen via MergeSort-Logik — Warum O(n log n) möglich ist

---

### 🧠 What this sheet asks (plain language)
You must:
1. count inversions in one concrete array and its halves,
2. explain what Merge’s cross-comparisons count,
3. conclude an \(O(n\log n)\)-algorithm.

So this is a “counting + divide-and-conquer correctness” task.

---

### 📚 Zero-to-functional foundations

#### 1) What is an inversion?
Given array \(a_1,\dots,a_n\), an inversion is a pair \((i,j)\) with
\[
i<j \quad\text{and}\quad a_i>a_j.
\]
Interpretation: a pair is “out of sorted order.”

- sorted ascending array \(\Rightarrow 0\) inversions.
- sorted descending array \(\Rightarrow \frac{n(n-1)}2\) inversions (maximum).

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

## 🧠 Aufgabe 1a — Count \(I\), \(I_1\), \(I_2\)

Given:
\[
a=(1,8,5,3,4,2,7,6).
\]
Halves:
- left: \((1,8,5,3)\),
- right: \((4,2,7,6)\).

### Left-half inversions \(I_1\)
Pairs:
- \(8>5\), \(8>3\), \(5>3\) \(\Rightarrow 3\).

So:
\[
I_1=3.
\]

### Right-half inversions \(I_2\)
Pairs:
- \(4>2\), \(7>6\) \(\Rightarrow 2\).

So:
\[
I_2=2.
\]

### Total \(I\) in full array
Additional crossing inversions (left index in first half, right index in second half):
- from \(8\): \(8>4,2,7,6\) \(\Rightarrow 4\),
- from \(5\): \(5>4,2\) \(\Rightarrow 2\),
- from \(3\): \(3>2\) \(\Rightarrow 1\),
- from \(1\): none.

Crossing total \(=7\).

Hence:
\[
I=I_1+I_2+7=3+2+7=12.
\]

\[
\boxed{I=12,\ I_1=3,\ I_2=2}
\]

---

## 🧠 Aufgabe 1b — Merge step and \(I-(I_1+I_2)\)

Sorted halves given:
- \(L=(1,3,5,8)\),
- \(R=(2,4,6,7)\).

During merge:
- compare 1 vs 2 \(\to\) take 1 (no new crossing inversion),
- compare 3 vs 2 \(\to\) take 2; remaining left \((3,5,8)\) all \(>2\): +3,
- compare 3 vs 4 \(\to\) take 3,
- compare 5 vs 4 \(\to\) take 4; remaining left \((5,8)\): +2,
- compare 5 vs 6 \(\to\) take 5,
- compare 8 vs 6 \(\to\) take 6; remaining left \((8)\): +1,
- compare 8 vs 7 \(\to\) take 7; remaining left \((8)\): +1,
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

## 🧠 Aufgabe 1c — Show \(O(n\log n)\)

### Algorithm idea
Function `count_inversions(A)`:
1. if length \(\le 1\): return (A, 0),
2. split into \(L,R\),
3. recursively get \((L_{sorted}, I_L)\), \((R_{sorted}, I_R)\),
4. merge while counting crossing inversions \(I_C\),
5. return \((A_{sorted}, I_L+I_R+I_C)\).

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
- \(I=12\), \(I_1=3\), \(I_2=2\),
- merge-cross part is \(7=I-(I_1+I_2)\),
- total inversion count via merge-based divide-and-conquer in \(O(n\log n)\).

---

## 🧩 Topic 2: Zweitkleinstes Element mit weniger Vergleichen

---

### 🧠 What this asks
Find smallest and second smallest with fewer than naive \(2n-3\) comparisons.

---

### 📚 Foundation
Naive:
- min in \(n-1\),
- second min among rest in \(n-2\),
\[
2n-3.
\]

Better idea: **tournament tree**.
- each comparison is a match,
- smallest element is tournament winner,
- second smallest must be among elements that directly lost to winner.

Winner has \(\log_2 n\) wins (because \(n\) is power of 2), so only \(\log_2 n\) candidates for second smallest.

---

### 🌉 Analogy: Knockout bracket
Gold medalist (smallest) beats \(\log_2 n\) players.
Silver (second smallest) must be one of those defeated directly by champion, not arbitrary participant.

---

### Efficient comparison count
- tournament to find min: \(n-1\),
- min over loser-list of size \(\log_2 n\): \(\log_2 n-1\).

Total:
\[
n+\log_2 n-2.
\]
This is \(\le \frac32 n-2\) for \(n\ge2\), so required bound holds.

\[
\boxed{n+\log_2 n-2\ \text{comparisons}}
\]

---

## 🧩 Topic 3: “Squaring vs Multiplication” equivalence statement

---

### Statement
“Exactly then there is an \(O(f)\)-algorithm for squaring \(n\)-digit numbers iff one can multiply two \(n\)-digit numbers in \(O(f)\).”

### Direction 1 (\(\Rightarrow\) trivial part)
If multiplication is \(O(f)\), then squaring is special case:
\[
x^2=x\cdot x
\]
also \(O(f)\).

### Direction 2 (nontrivial)
If squaring is \(O(f)\), can we multiply in \(O(f)\)? Yes via identity:
\[
xy=\frac{(x+y)^2-x^2-y^2}{2}.
\]
So three squarings + linear-time additions/subtractions/division by 2.

Total:
\[
O(f(n))+O(n).
\]
For standard multiplication contexts \(f(n)\in\Omega(n)\), this is \(O(f(n))\).

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
You must compare algorithms for \((n\times n)\)-matrix multiplication:
1. naive formula,
2. straightforward block recursion,
3. Strassen recursion,
4. asymptotic memory usage.

Assume \(n=2^k\), arithmetic op cost constant.

---

### 📚 Foundations

#### Matrix multiplication definition
For \(C=AB\):
\[
c_{ij}=\sum_{k=1}^n a_{ik}b_{kj}.
\]

Naive cost:
- \(n^2\) entries,
- each entry does \(n\) multiplications (+ additions),
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
- \(a=8,b=2\Rightarrow n^{\log_2 8}=n^3\),
- combine term \(n^2\) smaller,

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
- 7 recursive mults of size \(n/2\),
- plus \(\Theta(n^2)\) for additions/subtractions.

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

Need to store matrices/submatrices and temporary \(M_1,\dots,M_7\)-style buffers.

At recursion level \(i\), total active matrix-area is geometric and dominated by top level \(n^2\).  
Recursion stack contributes only \(O(\log n)\) frames.

Hence asymptotic auxiliary memory:
\[
\boxed{\Theta(n^2)}.
\]

(Implementation constants differ by in-place strategy, but order remains \(n^2\).)

---

### ⚠️ Pitfalls
1. Confusing time with memory.
2. Forgetting addition cost \(\Theta(n^2)\) per level.
3. Claiming Strassen is \(O(n^2)\) (false).

---

## 🧱 Topic 2: Zweidrittelsort — Correctness + Runtime

---

### 🧠 What this asks
For algorithm that recursively sorts:
1. first \(2/3\),
2. last \(2/3\),
3. first \(2/3\) again,

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

(Using simplification \(l\ge3\) divisible by 3.)

Let segment length \(l\), \(k=l/3\):
- call 1 sorts \(A[i..j-k]\),
- call 2 sorts \(A[i+k..j]\),
- call 3 sorts \(A[i..j-k]\) again.

Induction on \(l\):
- base \(l\le2\): direct swap correct.
- step: recursive calls sort overlapping large segments.
Because overlap size \(l/3\) is nonempty, extremes move toward correct sides:
  - smallest element gets forced into left segment and remains,
  - largest gets forced into right segment and remains,
  - recursive sorting + overlap yields full sorted order.

Thus algorithm sorts correctly.

\[
\boxed{\text{ZweidrittelSort ist korrekt}}
\]

---

## 🧠 Aufgabe 2b — Runtime recurrence

Each level does 3 recursive calls on size about \(2n/3\), plus constant overhead:
\[
T(n)=3T(2n/3)+\Theta(1).
\]
(Or \(\Theta(n^0)\) combine term.)

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
Without it, wildly oscillating \(f(n)\) can break naive “\(T(n)=\Theta(f(n))\)” conclusion.

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
Take \(n=3+12k\Rightarrow \sin(\pi n/2)=-1\), so \(f(n)=n\).  
But for \(n/3\)-related values sine may be \(+1\), making \(f(n/3)\) comparatively too large; no uniform contraction factor \(c<1\).

### (c) Why induction breaks
Trying \(T(n)\le C f(n)\):
\[
T(n)\le C f(n/3)+f(n).
\]
Need \(C f(n/3)\le (C-1)f(n)\), equivalent to regularity-type bound. If absent, step fails.

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
