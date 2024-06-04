# Compilers Assignments
## Assignment 1
Implementare tre passi LLVM (dentro lo stesso passo LocalOpts già scritto durante il LAB 2) che realizzano le seguenti ottimizzazioni locali:
 
 1. Algebraic Identity
    - $x + 0 = 0 + x \Rightarrow x$
    - $x \times 1 = 1 \times x \Rightarrow x$

 2. Strength Reduction (più avanzato)
    - $15 \times x = x \times 15 \Rightarrow (x << 4) – x$
    - $y = x / 8 ⇒ y = x >> 3$

 3. Multi-Instruction Optimization	
    - $a = b + 1, c = a − 1 ⇒ a = b + 1, c = b$
    
    
## Assignment 2
Consideriamo i seguenti problemi di Dataflow Analysis:
 - Very Busy Expressions 
 - Dominator Analysis
 - Constant Propagation

Per ognuno di essi…
 1. Derivare una formalizzazione riempiendo lo specchietto coi parametri adeguati
<p align="center">
  <img src="./assets/second_assignment/formalization.png" width="400"/>
</p>
 2. Per il CFG di esempio fornito, popolare una tabella con le iterazioni dell’algoritmo iterativo di soluzione del problema
<p align="center">
  <img src="./assets/second_assignment/iteration_table.png" width="400"/>
</p>


## Assignment 3
Implementare un passo di Loop-Invariant Code Motion (LICM) che sposti al di fuori del loop le istruzioni non dipendenti dal loop stesso.

L'algoritmo si suddivide nei seguenti passaggi:
   1. Dato l'insieme di nodi di un loop, calcolare le reaching definitions.
   2. Determinare le istruzioni loop-invariant, definite tali se entrambi gli operandi lo sono a loro volta.
      Un operando è loop-invariant se:
         - è una costante
         - è un argomento di funzione
         - la sua reaching definition non è contenuta nel loop ed è a sua volta loop-invariant
   3. Calcolare i dominatori (dominance tree)
   4. Trovare le uscite del loop, ovvero i successori al di fuori del loop
   5. Determinare la presenza di istruzioni candidate per la Code Motion:
      1. istruzioni loop invariant
      2. l’istruzione (la definizione) domina tutti gli usi (sottointeso nel SSA)
      3. il basic block contenente l'istruzione domina tutte le uscite del loop OPPURE la variabile definita dall'istruzione è dead all'uscita del loop, ovvero non ha usi
   6. Spostare le istruzioni trovate nel blocco preheader.

## Assignment 4
Implementare un passo di Loop Fusion che sia in grado di fondere due loop se verificate le seguenti condizioni:
1. $L_j$ e $L_k$ devono essere adiacenti
   - Non ci devono essere statements da eseguire tra la fine di $L_j$ e l'inizio di $L_k$
2. $L_j$ e $L_k$ devono iterare lo stesso numero di volte
3. $L_j$ e $L_k$ devono essere control flow equivalenti
   - Se $L_j$ esegue, anche $L_k$ deve eseguire, o viceversa
4. Non ci devono essere dipendenze a distanza negativa
   - una distanza negativa accade tra $L_j$ e $L_k$ ($L_j$ prima di $L_k$), quando ad un’iterazione m $L_k$ usa un valore che è calcolato da $L_j$ ad una iterazione futura $m+n$ (con $n>0$)
