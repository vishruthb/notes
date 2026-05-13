the first phase of the compiler, reading raw characters and grouping them into tokens (e.g. `id`, `num`, `+`, `if`). regular expressions (regex) define what the tokens look like, and finite automata (nfas, dfas) are the machines that are able to recognize them.
# regular expressions
three fundamental operations in the order of precedence (tightest first):
- repetition (`*`): zero or more. `a*` matches $\epsilon$, a, aa, aaa, ...
- concatenation: `ab` matches only "`ab`"
- alternation (`|`): choice between `a | b`, which matches "a" or "b"

precedence matters. `a | b*` means `a | (b*)`, either a single `a`, or zero or more `b`'s. if we want zer oor more of (a or b), we'd need `(a  | b)*`.

some shorthands:
- `a+` -> `a a*`
- `a?` -> zero or one, or `ε | a`
- `[a-z]` -> character class, or `a | b | c | ... | z`
# nfas vs. dfas
nfa (nondeterministic finite automaton) can have **multiple transitions** from the same state on the same input, plus $\epsilon$-transitions that move without consuming any input. a dfa (deterministic) has exactly **one transition** per state per input symbol, without $\epsilon$-transitions.

every regex can be converted into an nfa via **thompson's construction**, every nfa can be converted into a dfa via **subset construction**, and dfas can be minimized to a scanner.
# thompson's construction
- for a single character `a`, start state -> on a -> accept state
- for a concatenation $R_1 R_2$, connect the accept state of $R_1$'s nfa to the start state of $R_2$'s nfa with an $\epsilon$-transition
- for an alternation $R_1 | R_2$, new start state with $\epsilon$-transitions to both $R_1$ and $R_2$'s start states, and both accept states get $\epsilon$-transitions to a new shared accept state.
- for a repetition $R*$, new start state with $\epsilon$-transitions to r's start. r's accept state gets an $\epsilon$-transition back to r's start (the loop). new start state also has $\epsilon$-transition directly to the new accept state (for the zero-times case).
# subset construction
each dfa represents a set of nfa states. we start with the $\epsilon$-closure of the nfa's start state (all states reachable with $\epsilon$-transitions). for each input symbol, we compute where the set of nfa states can go, take the $\epsilon$-closure of that, and that becomes a new dfa state. a dfa state is accepting if **any** nfa state in its set is accepting.