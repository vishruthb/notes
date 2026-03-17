A parser's job is to take a stream of tokens from the scanner and determine if they form a valid program according to the grammar's rules. An LL(1) parser does this top-down, left-to-right, using **1 token of lookahead**. In order to build an LL(1) parser, we need a parsing table that tells us which production rule to use given a nonterminal $A$ and the next token $x$. 

This process can be formalized into 3 steps.
#### Preface: Grammar Notation
A CFG has terminals which include actual tokens like $x$, $y$, $+$, nonterminals which are abstract categories like $S$, $\text{Expr}$, $\text{List}$, and production rules that can look like $A \rightarrow B C d$. 

The symbol $\epsilon$ means empty. In practice, a production rule like $B \rightarrow \epsilon$ means that $B$ can produce nothing.
### 1. Compute FIRST Sets
$\text{FIRST}(X)$ is just telling us what terminals can X start with.
- If $X$ is a terminal, FIRST(X) = {X}
- If $X \rightarrow \epsilon$ is a production, we add $\epsilon$ to FIRST(X)
- If $X \rightarrow Y_{1} Y_{2} Y_{3} ...$, we add $\text{FIRST}(Y_{1})$ minus $\epsilon$. If $\epsilon \in\text{FIRST}(Y_1)$, we also $\text{FIRST}(Y_{2})$ minus $\epsilon$, and so on. If ALL of them can be $\epsilon$, then we add $\epsilon$.

**Example:**
```
1: S → A z
2: A → B D
3: B → x
4: B → ε
5: D → y
6: D → ε
```

We want to find FIRST(S). We start with S -> A z, so we look at FIRST(A), which is A -> B D. We then look at FIRST(B), which gives B -> x, which gives {x}. We also have B -> e, which gives {e} as well. Following the same process for D, we have {y, e}. Since both B and D can be e, then A can as well. That means that FIRST(A) = {x, y, e}. However, S has z as well in S -> A z, so we conclude that FIRST(S) = {x, y, z}.
### 2. Compute FOLLOW sets
FOLLOW(X) sets tell us what terminals can appear immediately after X in any derivation.
- Add eof or $ to FOLLOW(start symbol)
- If there's a production A -> a B b, add FIRST(b) minus e to FOLLOW(B)
- If there's a production A -> a B or A -> a B b where e $\in$ FIRST(b), we add FOLLOW(A) to FOLLOW(B)

From the above example, we can start with FOLLOW(S) = {eof}, as its the start symbol. We then go to S -> A z. FOLLOW(A) gets FIRST(Z) = {z}, so FOLLOW(A) = {z}. Next is A -> B D, FOLLOW(B) gets FIRST(D), giving us FOLLOW(B) = {y}. Since D can be e, FOLLOW(B) also gets FOLLOW(A) = {z}, so FOLLOW(B) = {y, z}. Lastly, in A -> B D, FOLLOW(D) gets FOLLOW(A) = {z}, so FOLLOW(D) = {z}.

### 3. Compute FIRST+ sets per production rule
This is what actually goes in the table.
- If e $\notin$ FIRST(rhs), FIRST+(A -> a) = FIRST(a)
- If e $\in$ FIRST(rhs), FIRST+(A -> a) = FIRST(a) $\cup$ FOLLOW(A)

If the right hand side can vanish entirely, then we'd choose this rule when we see anything that could follow A. Following the same example:
- Rule 1: S -> A z. FIRST(Az) = {x, y, z}. No e. FIRST+ = {x, y , z}.
- Rule 2: A -> B D. FIRST(BD) = {x, y, e}. Since e is present, FIRST+ = {x, y} $\cup$ FOLLOW(A) = {x, y, z}
- Rule 3: B -> x, so FIRST+ = {x}
- Rule 4: B -> e, since e is present, FIRST+ = FOLLOW(B) = {y, z}.
- Rule 5: D -> y, so FIRST+ = {y}
- Rule 6: D -> e, since e is present, FIRST+ = FOLLOW(D) = {z}.

Now, all we need to do is just fill in the LL(1) table. For each rule A -> a, we put that rule number in the TABLE[A, t] for every terminal t in FIRST+(A -> a).

|     | x   | y   | z   | EOF |
| --- | --- | --- | --- | --- |
| S   | 1   | 1   | 1   | ERR |
| A   | 2   | 2   | 2   | ERR |
| B   | 3   | 4   | 4   | ERR |
| D   | ERR | 5   | 6   | ERR |

> [!Important]
> A grammar is LL(1) iff no cell in this table has more than one rule. If two rules for the same nonterminal have overlapping FIRST+ sets, we have a conflict and the grammar is LL(1).

The skeleton parser uses this table with a stack, pushing EOF then the start symbol. It loops IF the top-of-stack is a terminal, matching it with current input and pop. IF it is a nonterminal, we look up `TABLE[TOS, curr_word]`, pop TOS, and push the RHS in reverse order. It's done when both stack + input are EOF.