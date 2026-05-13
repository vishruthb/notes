a parser's job is to take a stream of tokens from the scanner and determine if they form a valid program according to the grammar's rules. an ll(1) parser does this top-down, left-to-right, using **1 token of lookahead**. in order to build an ll(1) parser, we need a parsing table that tells us which production rule to use given a nonterminal $A$ and the next token $x$.

this process can be formalized into 3 steps.
#### preface: grammar notation
a cfg has terminals which include actual tokens like $x$, $y$, $+$, nonterminals which are abstract categories like $S$, $\text{Expr}$, $\text{List}$, and production rules that can look like $A \rightarrow B C d$.

the symbol $\epsilon$ means empty. in practice, a production rule like $B \rightarrow \epsilon$ means that $B$ can produce nothing.
### 1. compute first sets
$\text{FIRST}(X)$ is just telling us what terminals can x start with.
- if $X$ is a terminal, first(x) = {x}
- if $X \rightarrow \epsilon$ is a production, we add $\epsilon$ to first(x)
- if $X \rightarrow Y_{1} Y_{2} Y_{3} ...$, we add $\text{FIRST}(Y_{1})$ minus $\epsilon$. if $\epsilon \in\text{FIRST}(Y_1)$, we also $\text{FIRST}(Y_{2})$ minus $\epsilon$, and so on. if all of them can be $\epsilon$, then we add $\epsilon$.

**example:**
```
1: S → A z
2: A → B D
3: B → x
4: B → ε
5: D → y
6: D → ε
```

we want to find first(s). we start with s -> a z, so we look at first(a), which is a -> b d. we then look at first(b), which gives b -> x, which gives {x}. we also have b -> e, which gives {e} as well. following the same process for d, we have {y, e}. since both b and d can be e, then a can as well. that means that first(a) = {x, y, e}. however, s has z as well in s -> a z, so we conclude that first(s) = {x, y, z}.
### 2. compute follow sets
follow(x) sets tell us what terminals can appear immediately after x in any derivation.
- add eof or $ to follow(start symbol)
- if there's a production a -> a b b, add first(b) minus e to follow(b)
- if there's a production a -> a b or a -> a b b where e $\in$ first(b), we add follow(a) to follow(b)

from the above example, we can start with follow(s) = {eof}, as its the start symbol. we then go to s -> a z. follow(a) gets first(z) = {z}, so follow(a) = {z}. next is a -> b d, follow(b) gets first(d), giving us follow(b) = {y}. since d can be e, follow(b) also gets follow(a) = {z}, so follow(b) = {y, z}. lastly, in a -> b d, follow(d) gets follow(a) = {z}, so follow(d) = {z}.

### 3. compute first+ sets per production rule
this is what actually goes in the table.
- if e $\notin$ first(rhs), first+(a -> a) = first(a)
- if e $\in$ first(rhs), first+(a -> a) = first(a) $\cup$ follow(a)

if the right hand side can vanish entirely, then we'd choose this rule when we see anything that could follow a. following the same example:
- rule 1: s -> a z. first(az) = {x, y, z}. no e. first+ = {x, y , z}.
- rule 2: a -> b d. first(bd) = {x, y, e}. since e is present, first+ = {x, y} $\cup$ follow(a) = {x, y, z}
- rule 3: b -> x, so first+ = {x}
- rule 4: b -> e, since e is present, first+ = follow(b) = {y, z}.
- rule 5: d -> y, so first+ = {y}
- rule 6: d -> e, since e is present, first+ = follow(d) = {z}.

now, all we need to do is just fill in the ll(1) table. for each rule a -> a, we put that rule number in the table[a, t] for every terminal t in first+(a -> a).

|     | x   | y   | z   | eof |
| --- | --- | --- | --- | --- |
| s   | 1   | 1   | 1   | err |
| a   | 2   | 2   | 2   | err |
| b   | 3   | 4   | 4   | err |
| d   | err | 5   | 6   | err |

> [!important]
> a grammar is ll(1) iff no cell in this table has more than one rule. if two rules for the same nonterminal have overlapping first+ sets, we have a conflict and the grammar is ll(1).

the skeleton parser uses this table with a stack, pushing eof then the start symbol. it loops if the top-of-stack is a terminal, matching it with current input and pop. if it is a nonterminal, we look up `TABLE[TOS, curr_word]`, pop tos, and push the rhs in reverse order. it's done when both stack + input are eof.