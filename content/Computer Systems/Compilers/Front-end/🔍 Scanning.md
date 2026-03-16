The first phase of the compiler, reading raw characters and grouping them into tokens (e.g. `id`, `num`, `+`, `if`). Regular expressions (regex) define what the tokens look like, and finite automata (NFAs, DFAs) are the machines that are able to recognize them.
# Regular Expressions
Three fundamental operations in the order of precedence (tightest first):
- Repetition (`*`): zero or more. `a*` matches $\epsilon$, a, aa, aaa, ...
- Concatenation: `ab` matches only "`ab`"
- Alternation (`|`): choice between `a | b`, which matches "a" or "b"

Precedence matters. `a | b*` means `a | (b*)`, either a single `a`, or zero or more `b`'s. If we want zer oor more of (a or b), we'd need `(a  | b)*`.

Some shorthands:
- `a+` -> `a a*`
- `a?` -> zero or one, or `ε | a`
- `[a-z]` -> character class, or `a | b | c | ... | z`
# NFAs vs. DFAs
NFA (nondeterministic finite automaton) can have **multiple transitions** from the same state on the same input, plus $\epsilon$-transitions that move without consuming any input. A DFA (deterministic) has exactly **one transition** per state per input symbol, without $\epsilon$-transitions.

Every regex can be converted into an NFA via **Thompson's construction**, every NFA can be converted into a DFA via **subset construction**, and DFAs can be minimized to a scanner.
# Thompson's Construction
- For a single character `a`, start state -> on a -> accept state
- For a concatenation $R_1 R_2$, connect the accept state of $R_1$'s NFA to the start state of $R_2$'s NFA with an $\epsilon$-transition
- For an alternation $R_1 | R_2$, new start state with $\epsilon$-transitions to both $R_1$ and $R_2$'s start states, and both accept states get $\epsilon$-transitions to a new shared accept state.
- For a repetition $R*$, new start state with $\epsilon$-transitions to R's start. R's accept state gets an $\epsilon$-transition back to R's start (the loop). New start state also has $\epsilon$-transition directly to the new accept state (for the zero-times case).
# Subset Construction
Each DFA represents a set of NFA states. We start with the $\epsilon$-closure of the NFA's start state (all states reachable with $\epsilon$-transitions). For each input symbol, we compute where the set of NFA states can go, take the $\epsilon$-closure of that, and that becomes a new DFA state. A DFA state is accepting if **any** NFA state in its set is accepting.