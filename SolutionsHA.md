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
