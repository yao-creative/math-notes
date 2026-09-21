Framing this as a **graded epistemic ladder**: each tier presupposes mastery of the tier below it, moving from *syntactic recognition* → *operational computation* → *structural/equivalence reasoning* → *typing-theoretic synthesis*. I'll label the cognitive demand of each question so you can locate exactly which competency it's probing.

---

**Tier 1 — Syntactic recall (can you parse the grammar itself?)**

1. *(recognition)* In $\Pi x^{t_1}.t_2$, which subterm is in binder position, which is the type annotation, and which is the scope?
2. *(recognition)* Is $t\,t$ (application) a binding construct? Justify by checking whether it appears in the definition of $\mathrm{FV}$ with a set-difference term.

**Tier 2 — Operational computation (can you execute the definitions by hand?)**

3. *(computation)* Compute $\mathrm{FV}(\lambda x^{*}.\lambda y^{x}. x\,y)$ step by step, showing each recursive call.
4. *(computation)* Compute the capture-avoiding substitution $(\lambda y^{t}.x\,y)[x := y]$. What goes wrong if you substitute naively without renaming, and what does the corrected result look like?

**Tier 3 — Structural/equivalence reasoning (do you understand *why* the machinery is needed?)**

5. *(structural)* Prove informally that $\sim_\alpha$ as I defined it (smallest equivalence relation closed under consistent renaming) is reflexive, symmetric, and transitive — which of these three is actually doing the interesting work?
6. *(representation-theoretic)* Translate $\lambda x^{t}.\lambda y^{t}. x$ into de Bruijn form. Then explain why de Bruijn indices make $\alpha$-equivalence coincide with syntactic ($=$) equality on the quotient set $\mathrm{Term}/{\sim_\alpha}$.
7. *(counterexample construction)* Construct a term $u$ and a substitution $t'[x:=u]$ where naive (non-capture-avoiding) substitution changes the term's *meaning*, not just its shape — i.e. produces a term whose free-variable set differs from what capture-avoidance would give.

**Tier 4 — Typing-theoretic synthesis (can you connect binding to the PTS machinery itself?)**

8. *(judgment-level)* Write the formation rule for $\Pi x^{t_1}.t_2$ as a typing judgment: what must $\Gamma \vdash t_1 : ?$ be, and under what extended context must $t_2$ be typed?
9. *(dependency-tracing)* In a *dependent* product $\Pi x^{t_1}.t_2$, $x$ can occur free in $t_2$. Give a concrete example where this dependency is essential — i.e. where replacing $t_2$'s occurrences of $x$ with a fixed term would produce a strictly less expressive type. (This is really asking: can you distinguish $\Pi$ from ordinary $\to$?)
10. *(meta-level)* Why does $\beta$-reduction, $(\lambda x^t.t')\,u \rightsquigarrow t'[x:=u]$, *require* capture-avoiding substitution to be well-defined as an operation on $\mathrm{Term}/{\sim_\alpha}$ rather than on raw syntax trees?

---

Want me to hold off and let you answer these one at a time (Socratic mode), or would you rather I post model answers now so you can self-grade?