Datenstrukturen und effiziente Algorithmen – Wintersemester 2025
Präsenzblatt 13
Aufgabe 1 Heap-Laufzeiten
Wir betrachten zwei Arten, einen binären Heap aufzubauen. Zunächst betrachten wir die
rekursive Variante mittels heapify aus der Vorlesung. Dabei rufen wir rekursiv heapify
auf das linke und rechte Kind auf (falls existent) und anschließend trickle-down auf den
Knoten selbst.
a) Begründen Sie, dass dieser Algorithmus in O (n) läuft.
Wir können die Heapeigenschaften initial auch auf andere Weise herstellen: Dazu fügen
wir zunächst alle Elemente in beliebiger Reihenfolge in das zugrundeliegende Array ein
und rufen für absteigendes i = n − 1, . . . , 0 die Funktion trickle-down für Element i auf.
b) Die Elemente seien wie folgt eingefügt worden. Führen Sie den restlichen Algorithmus aus:
					        4
				/		              \
			1			              	5
	 /	    \			        /	     \
	5      		2	      	6	       	7
  /	 \	  /	   \		  |
	3   4  6 	    8		  3



Wo könnte man i beginnen lassen?

c) Zeigen Sie, dass der Algorithmus in O(n) einen Heap aufbaut. Verwenden Sie die
Schleifeninvariante “Zu Beginn der Schleifeniteration für i sind alle Elemente an
den Positionen n − 1, . . . , i + 1 Wurzeln von Heaps”.
Tipp: Man verwende für die Laufzeit Summe^∞_k=0 von kx^k = x/((1−x)^2) .

Aufgabe 2 Residualnetzwerk

Stellen Sie das Residualnetzwerk für folgendes Flussnetzwerk auf:

		      	0	      ->	 1
		12/15 /   \ 7/9	  	    \ 5/11
 (up arrow)  (down arrow)   (down arrow)
		   5/15	    5/13	      13/20
	  s   ->   2   ->    3    	->	 t		
			                 ^           ^
		   \ 1/15     / 1/7	         / 0/9
		 (down arrow)	      0/5
		          4      	   ->  	5
Gibt es einen augmentierenden Pfad?


Präsenzblatt 12
Aufgabe 1
Berechnen Sie einen MST im folgenden Graphen einmal mit dem Algorithmus von Prim
und einmal mit dem Algorithmus von Kruskal.

		        3
	    A	    -	    B	
	  /    \		  |     \
  / 8   	\ 10	|2      \7
	  0         1		    11
 C  -      D  - E     -  	F
	    |	       /        /
	    |4      /	    9 /
	    |     / 6	    /
	    |   /   5    /
	    G       -   H


Aufgabe 2 ZHKs bestimmen
Sei G = (V, E) ein (ungerichteter) Graph, der in der Form von Adjazenzlisten repräsentiert sei. Wir haben also für jeden Knoten v eine Liste A[v], die die Nachbarn von v enthält.
Wir betrachten jetzt einen Subgraphen H = (V, E′) mit E′ ⊆ E von G. Diesen repräsentieren wir, in dem wir zusätzlich zu jeder Kante noch speichern, ob sie in H enthalten ist (Das können wir entweder in A selbst tun, oder in getrennten Listen S[v], die für jede Kante diesen Wahrheitswert verwalten.).

Zeigen Sie, dass Sie die Zusammenhangskomponenten in H in Zeit O (|V | + |E|) finden können.


Präsenzblatt 11
Aufgabe 1 Tabellen reduzieren
In der Vorlesung wurde der Floyd-Warshall-Algorithmus zunächst mit k verschiedenen Tabellen eingeführt. Später wurde die Behauptung getroffen, dass bereits eine Tabelle ausreicht. Begründen Sie, dass dies der Fall ist, also, dass wir also tatsächlich im k-ten Schritt
d_ij = min{d_ij , d_ik + d_kj } anstelle von d^(k)_ij = min{d^(k−1)_ij , d^(k−1)_ik + d^(k−1)_kj } schreiben können.

Aufgabe 2 Dijkstra
Betrachten Sie den folgenden gerichteten Graphen:
       C
       ^
       |
A ->   B
^    ^ ^
|   /  |
| /    |
s ->    D

Geben Sie für ihn Kantengewichte an, sodass
1. nur eines der Kantengewichte negativ ist, der Dijkstra-Algorithmus mit Startknoten s aber nicht für jeden Knoten die korrekte Kürzeste-Wege-Distanz liefert,
2. alle Kantengewichte negativ sind, der Dijkstra-Algorithmus mit Startknoten s aber trotzdem korrekte Kürzeste-Wege-Distanzen für alle Knoten liefert,
3. es verschiedene Kürzeste-Wege-Bäume mit Startknoten s gibt.
Begründen Sie Ihre Antworten


Präsenzblatt 10
Aufgabe 1 Minimales Entrobier
Bestimmen Sie von Hand einen optimalen präfixfreien Code für das Wort ESGIBTFREIBIER mithilfe der Huffman-Kodierung. Wie viele Bits werden für dieses Wort benötigt?

Aufgabe 2 Richtige Induktion
Zeigen Sie die folgende Aussage:
Sei G = (V, E) ein endlicher, ungerichteter Graph ohne Schleifen1. Ist deg(v) ≤ 2 ∀v ∈ V , so gilt für jede Zusammenhangskomponente Z genau eine der drei folgenden Aussagen:
• Z ist ein isolierter Knoten
• Z ist ein einfacher Pfad (von mindestens zwei Knoten)
• Z ist ein einfacher Zyklus (von mindestens drei Knoten)
Tipp: Induktion

Präsenzblatt 9
Aufgabe 1 Matrix-Kettenmultiplikation
Berechnen Sie mit einem Polynomialzeitalgorithmus aus der Vorlesung die optimale
Klammerung des Produktes A1 ·A2 ·A3 ·A4 von Matrizen mit den folgenden Dimensionen:

A1 	A2 	A3 	A4
35 × 15	 15 × 5	 5 × 10 	10 × 20

Aufgabe 2 Tatsächlich Tragfähige Teiltortentürme
Wir erinnern uns an die Präsenzaufgabe vom letzten Übungsblatt: Dort wollte Professor Hirnriss seine Tochter mit einer mehrstufigen Geburtstagstorte überraschen. Beim Umsetzen in die Praxis fällt allerdings nach mehrfachen Tortenrutschen auf, dass für eine stabile Torte die einzelnen Schichten in absteigender Größe aufeinander gestapelt werden müssen.
Wir erinnern uns dennoch zunächst an den Lösungsansatz für dieses einfachere Problem. Die Idee war, dass wir die Größe der letzten Springform frei wählen konnten, und deshalb

T (n) = Summe_v_i∈V von T (n − v_i)  mit T (0) = 1 und T (v) = 0 für v < 0.

gilt. Modifizieren Sie nun den Ansatz, sodass Sie für gegebenes n ∈ N und eine Menge
von (paarweise verschiedenen) Springformen V alle Darstellungen einer n-stückigen Torte berechnen, wobei nun die aufeinandergestapelten Tortenscheiben absteigend nach ihrer Größe sortiert sein müssen.

Tipp: Versuchen Sie, die Sortiertheit der Darstellungen schlau in ihrem Programm zu
kodieren, ohne die Darstellungen dabei explizit zu speichern. Dafür müssen Sie Ihre
Tabelle vergrößern!

Präsenzblatt 8
Aufgabe 1 145
a) Prüfen Sie für jeden der folgenden Binärbäume:
• Hat der Baum die Suchbaumeigenschaft?
• Erfüllt der Baum die AVL-Höhenbedingung?
		    4				        	4
	   /	    \				   /	    \
	  1		      5			  2		      6
 /	  \	    /	   \	    	    /	    \	
0	     2	 3      7  		     1	      3
			   /   \              /
			  6     8            0



b) Fügen Sie die folgenden Elemente in einen (zunächst leeren) AVL-Baum ein: 1, 0, 5, 4, 3, 2, 6.

Zeichnen Sie den Baum nach jeder Einfüge-Operation und markieren Sie, welche
Rotationen Sie vorgenommen haben.

Aufgabe 2 Theoretische Teiltortentürme
Die Tochter von Professor Hirnriss hat bald Geburtstag und als liebevoller Papa möchte er ihr natürlich diesen besonderen Tag mit einer prachtvollen Torte versüßen. Der gute Akademiker hat verschiedene Springformen zur Verfügung, mit denen er runde Kuchen bestehend aus 1, 2 oder 5 Tortenstücken backen kann.

Um nun den n-ten Geburtstag angmessen zu feiern (und dabei die Kuchen-Katastrophen der letzten n − 1 Jahre möglichst nicht zu wiederholen), soll die Torte aus insgesamt n ∈ N Tortenstücken bestehen und durch Stapeln der runden Kuchen hergestellt werden.

a) Überlegen Sie sich alle Möglichkeiten, eine Torte mit n = 5 Stücken darzustellen.
b) Entwickeln Sie einen Algorithmus, mit dem Sie für einen gegebenen Zielgröße n
und eine Menge V von verfügbaren Springformen die Anzahl der Darstellungsmög-
lichkeiten berechnen können.

Präsenzblatt 7
Aufgabe 2 Tanzparty reloaded
Erinnern Sie sich an das zweite Präsenzblatt – dort wollte Professor Hirnriss Tanzpartner so einander zuordnen, dass deren kombinierte Größe einem vorgegebenen Wert entspricht. Wir hatten dafür sogar einen Linearzeitalgorithmus kennengelernt (welcher allerdings voraussetzte, dass die Liste der Größen sortiert war). Wenden Sie nun neugewonnenes Wissen aus der Vorlesung an, um einen Algorithmus für dieses Problem zu entwerfen, der ohne diese Voraussetzung auskommt und im Worst Case erwartete lineare Zeit benötigt. 
Zur Erinnerung: Gegeben ist eine (jetzt unsortierte) Liste L von paarweise verschiedenen Größen l_i ∈ R sowie eine vorgegebene Zielgröße h ∈ R.

Gesucht sind möglichst viele Paare (i, j) mit l_i + l_j = h.

Tipp: Sie können davon ausgehen, dass wir eine Hashfunktion aus einer c-universellen Klasse von Hashfunktionen in O (1) wählen und auswerten können.

Aufgabe 3 2-universelle Hashfunktionen
Wir betrachten die folgende Familie von 2-universellen Hashfunktionen:
Sei die Tabellengröße m ∈ N und sei p eine Primzahl mit m/2 ≤ p ≤ m. Wir betrachten
als Universum U natürliche Zahlen zur Basis p mit r Stellen, also 
x = Summe ^(r−1) _i=0 von  x_i · p^i, x_i ∈ {0, 1, . . . , p − 1}.
Für a = (a_0, a_1, a_(r−1)) ∈ {0, 1, . . . , p − 1}^r sei
h_a : U → {0, 1, . . . , m − 1}, h_a(x) = Summe ^(r−1) _i=0 von a_i * x_i mod p,
die Familie von Hashfunktionen ist:

H = {h_a | a ∈ {0, 1, . . . , p − 1}^r}.

Wir wählen unsere Hashfunktion für p = 11 durch a = (6, 1, 7, 4). Seien x = (1, 3, 3, 7), y = (4, 7, 1, 1) aus U .
1. Berechnen Sie h_a (x) und h_a (y). Gibt es eine Kollision?
2. Sei nun a(c) = (6, 1, c, 4), wobei c ∈ {0, 1, . . . , p − 1} variabel ist.
Bestimmen Sie, für welche c die Funktionswerte h_a(c) (x) und h_a(c) (y) kollidieren

Präsenzblatt 6
Aufgabe 1 Eine untere Schranke
In der Vorlesung haben wir bereits bewiesen, dass vergleichsbasiertes Sortieren mindestens Ω(n log n) Zeit benötigt. Gemeinhin ist es sehr schwer, untere Schranken zu beweisen; bei den meisten Problemen ist tatsächlich nicht bekannt, ob die derzeit besten Algorithmen wirklich asymptotisch optimal sind.
Dennoch wollen wir uns in dieser Aufgabe an einer weiteren unteren Schranke erfreuen: Zeigen Sie, dass das vergleichsbasierte Finden eines Elements in einer sortierten Liste mindestens Ω(log n) Zeit braucht.

Aufgabe 2 Randomized Partition
Es sei ein Array von n ≥ 3 paarweise verschiedenen Elementen gegeben. Wir modifizieren Randomized-Partition aus der Vorlesung:
Statt ein Element gleichverteilt aus dem Array zu ziehen, ziehen wir zufällig drei Elemente (ohne Zurücklegen) und wählen den Median der drei als Pivot.

a) Sei p_i = P (“Element mit Rang i ist das gewählte Pivot”). Geben Sie p_i als Funk-
tion von n und i an für 1 ≤ i ≤ n.
b) Wie haben sich unsere Chancen verbessert, den Median des Arrays als Pivot auszu-
wählen? Geben Sie das Verhältnis der Wahrscheinlichkeiten der 3-Median-Variante
und der Vorlesungsvariante von Randomized-Partition im Grenzwert n → ∞
an. (Sie müssen den Grenzwert nicht rigoros nachweisen.)

Präsenzblatt 5
Aufgabe 1 Polynommultiplikation
Betrachten Sie:
A(x) = 1 + 2x, B(x) = 3 − x.

Wir wollen das Produktpolynom C(x) = A(x)B(x) in Koeffizientenform berechnen.
Hierbei müssen wir nicht die n-ten, sondern die 2n-ten Einheitswurzeln verwenden, da das
Produktpolynom C(x) vom Grad 2n − 2 ist. Wir können dazu die Koeffizientenvektoren zu a′ = (1, 2, 0, 0) und b′ = (3, −1, 0, 0) erweitern, um konsistent in der Notation zu bleiben. Berechnen Sie zunächst die Vektoren 
DF T (a′) := 	[ A(ω0,4) ]
		          [ A(ω1,4) ]
		          [ A(ω2,4) ]
		          [ A(ω3,4) ],
DF T (b′) :=	[ B(ω0,4) ]
		          [ B(ω1,4) ]
		          [ B(ω2,4) ]
		          [ B(ω3,4) ],

bilden Sie das komponentenweise Produkt und bestimmen Sie daraus die Koeffizienten
von C.

Aufgabe 2
Alice würfelt mit einem roten und einem grünen Würfel gleichzeitig (beide sind fair) und
betrachtet die Augensumme S als Zufallsvariable.
1. Geben Sie die Ergebnismenge (Menge der Elementarereignisse) an.
2. Sind die Ereignisse „S ≥ 6“ und „S ≤ 3“ disjunkt? Sind sie unabhängig?
3. Alice macht n Doppelwürfe. Berechnen Sie den Erwartungswert der Anzahl X der
Doppelwürfe, in denen die Augensumme mindestens sechs ist.

Präsenzblatt 4
Aufgabe 1 Karatsuba
a) Geben Sie die Rekursionsgleichung für binäre Suche an. Was ist die Laufzeit? Geben
Sie a, b, α aus dem Mastertheorem an.
b) Berechnen Sie mit dem Karatsuba-Algorithmus das Produkt a · b für a = 4711, b =
1337, wobei Sie für n = 2 den Rekursionsabbruch nutzen können. Taschenrechner
sind ausnahmsweise erlaubt.

Aufgabe 2 Master-Theorem
Geben Sie möglichst enge Schranken in Θ-Notation für die folgenden Rekursionsgleichun-
gen an. Wenn Sie das Mastertheorem benutzen, so geben Sie die Beziehungen von a, b
und α an.
Alle n sind Zweierpotenzen und es gilt T (1) = 1 und c > 0. Für n ≥ 2:
a) T (n) = 4T (n/2) + cn^2,
b) T (n) = 3T (n/2) + cn,
c) T (n) = 2T (n/2) + c log_2 (n).

Präsenzblatt 3
Aufgabe 1 Falsche Induktion
Betrachten Sie den folgenden (fehlerhaften) Beweis dafür, dass Merge-Sort in O (n) Zeit
läuft: 
Beweis. Es ist zunächst T (n) ≤ 2T (⌈ n/2 ⌉) + cn die Rekursionsgleichung, welche die Laufzeit von Merge-Sort beschreibt. Wir zeigen nun mit vollständiger Induktion, dass die Laufzeit für alle n ∈ N linear ist.

IA: Offensichtlich brauchen wir für Arrays der Größe n = 1 nur konstante Laufzeit.
Damit gilt die Aussage hier also.
IV: Es gelte T (k) ∈ O (k) für alle k < n für ein beliebiges, festes n ∈ N.
IS: Wir rechnen nun:
T (n) ≤ 2T (⌈ n/2 ⌉) (gilt nach IV in O(n)) + cn (auch in O(n)) ∈ O (n) .
Das stimmt nicht, denn Merge-Sort braucht Ω(n log n) Zeit. Wo liegt also der Fehler?

Aufgabe 2 Stabil Sortieren
Zeigen Sie: Jeder vergleichsbasierte Sortieralgorithmus kann stabil gemacht werden.


Präsenzblatt 2
Aufgabe 1 Landau-Symbole
Ordnen Sie die folgenden Funktionen bezüglich ihres asymptotischen Wachstums (d.h.
Ordnen Sie die Funktionen bezüglich ⪯ mit f ⪯ g ⇔ f ∈ O (g)).
f1 : n  → πn
f2 : n  → 10 ^(1/n)
f3 : n  → 2^n
f4 : n  → n ln(n)
f5 : n  → log(n log n)
f6 : n  → sqrt(n^3)

Aufgabe 2 Partnersuche
Professor Hirnriss veranstaltet eine Tanzparty und möchte für den ersten Tanz geeignete
Gäste einander zuordnen. Anstatt jedoch dafür zu sorgen, dass gleichgroße Personen
miteinander tanzen, ist es das Ziel des Professors, dass die kombinierte Größe jedes
Tanzpaars gleich ist.
a) Entwerfen Sie einen Algorithmus, der für eine vorgegebene kombinierte Größe h und
eine sortierte Liste L der Körpergrößen der n Gäste herausfindet, wie viele Paare
mit Gesamtgröße h möglich sind. Sie dürfen davon ausgehen, dass alle Personen
unterschiedlich groß sind.
Tipp: Die Liste ist bereits sortiert.
b) Begründen Sie nun die Korrektheit und analysieren Sie die Laufzeit.

räsenzblatt 1
Aufgabe 1 Endlosschleife im naiven Algorithmus
Wir betrachten eine alternative Interpretation des Stable-Matching-Problem aus der Vorlesung. Dabei geht es um Bewerbende und freie Stellen. Konkret gibt es drei Personen
p1, p2, p3 und drei ausgeschrieben Stellen a1, a2, a3 mit den folgenden Präferenzen:
Person / Stelle	 Präferenzen
p1 		a3 < a1 < a2
p2 		a3 < a2 < a1
p3		a3 < a2 < a1
a1 		p2 < p3 < p1
a2 		p2 < p1 < p3
a3 		p2 < p1 < p3

Weiterhin wollen wir den naiven Algorithmus aus der Vorlesung erneut betrachten (Zur Erinnerung: Starte mit einer beliebigen Paarung Z. Solange es ein instabiles Paar {pi, aj } gibt, entferne die Paare mit p_i und a_j . Füge das Paar {p_i, a_j } und das Paar der ursprünglichen Partner von p_i und a_j aus Z hinzu.).

a) Zeigen Sie, dass der Algorithmus für die obige Eingabe nicht terminieren muss.
Starten Sie dabei mit der Zuordnung {pi, ai}, 1 ≤ i ≤ 3.
b) Finden Sie eine stabile Paarung Z^∗ für die obige Instanz. Kann der Algorithmus
Z^∗ finden, wenn er mit der Zuordnung aus a) beginnt?

Aufgabe 2 Logarithmen
Beweisen Sie die folgende Identitäten:
a) Seien a, b, x > 0. Dann gilt:
log_a(x) = log_b (x) / log_b (a)

b) Seien a, x > 0 und y ∈ R beliebig. Dann gilt:
log_a (xy) = y log_a (x).

Vereinfachen Sie nun die folgenden Ausdrücke.
c) 4^log_2(n)
d) (ln(n^ln(n))) / (ln(n))^2
e) Summe^n_k=1 von ln(k) 

Aufgabe 3 Nadel im Heuhaufen
Wie viele mögliche (perfekte) Paarungen gibt es beim Problem der stabilen Paarungen
mit jeweils n Bewerbenden und freien Stellen?
(Achtung: Es ist nicht nach der Anzahl der stabilen Paarungen gefragt!)
