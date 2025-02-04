# Correctness Problem
```python
procedure GraphSearch (𝐺: graph, 𝑠: vertex) 
	Initialize X = empty, F = {𝑠}, U = V – F. 
	While F is not empty: 
		Pick 𝑤 in F. 
		For each outgoing neighbor 𝑦 of 𝑤 (for every (𝑤, 𝑦) ∈ 𝐸): 
			If 𝑦 is not in X or F, then move 𝑦 from U to F. Move 𝑤 from F to X.
	Return X
```
# Proof
To prove correctness, we must show that at the end of the algorithm, we either have A or B -
* A: If v $\in$ X then there is a path from s to v.
* B: If v $\notin$ X then there is not a path from s to v.
## Correctness (A)
* Loop Invariant: After t iterations of the while loop, every element of X or F is reachable from s in G.
* Base Case: Before going through the loop, X is empty and F is {s}.
* You pick a vertex 𝑣 in F. (Which vertex depends on the data structure. For the sake of this proof, we can pick any of the vertices in F next.)
* We move all neighbors of 𝑣 into F if they are in U.
	* If there is a path from s to v and an edge (v, u) then there is a path from s to u.
* We move v from F to X.
	* By the inductive hypothesis, we know there is a path from s to v.
## Correctness (B)
* Suppose by contradiction that there is a vertex v reachable from s that is not in X. Then there is a path from s to v. Let z be the last vertex in the path that is in X and w be the next vertex after z in the path.
* Then z must have been in F at some point. And when z was picked from F, w must have been moved from U to F. And down the line, w must have been moved from F to X.
* This contradicts our assumption that v is not in C.
