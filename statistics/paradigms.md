**Intent: categorical formalization of statistical inference paradigms.**

A useful way to see the four paradigms is that they share the same **statistical model**, but differ in the morphism from data/model to inferential output.

Let

$$
\mathcal D
\overset{\text{data}}{\longleftarrow}
\mathcal M
$$

where a statistical model contains a parameter space \(\Theta\), sample space \(X\), and likelihood/data-generating structure.

Then the paradigms can be drawn as four different inference functors.

### 1. Frequentist

The parameter \(\theta\) is treated as fixed, while the data \(X\) is random.

$$
\begin{array}{ccc}
\Theta & \longrightarrow & \mathcal P(X)\\
& & \uparrow\\
& & x
\end{array}
$$

More concretely:

$$
\boxed{
\theta
\longmapsto
P_\theta
\longmapsto
X
\longmapsto
T(X)
\longmapsto
\text{estimator / CI / test}
}
$$

The inferential object is therefore something like

$$
I_F:X\to\mathcal I_F
$$

where \(\mathcal I_F\) contains estimators, confidence procedures, test statistics, \(p\)-values, etc.

The defining conceptual direction is:

$$
\text{parameter}
\rightarrow
\text{sampling distribution}
\rightarrow
\text{observed data}
\rightarrow
\text{procedure}.
$$

---

### 2. Bayesian

Bayesian inference introduces a prior:

$$
\Theta
\xrightarrow{\pi}
\mathcal P(\Theta)
$$

and the likelihood combines with the prior:

$$
\boxed{
\pi(\theta)
\quad+\quad
p(x\mid\theta)
\quad\longrightarrow\quad
p(\theta\mid x)
}
$$

Categorically, think of:

$$
\begin{array}{ccc}
\Theta & \xrightarrow{\text{prior}} & \mathcal P(\Theta)\\
\downarrow && \downarrow\\
X & \xrightarrow{\text{data}} & \mathcal P(X)
\end{array}
$$

with Bayes' rule providing the transformation

$$
(\pi,p)
\longmapsto
\pi(\theta\mid x).
$$

So the inferential output is itself a probability measure:

$$
I_B:
X\to\mathcal P(\Theta).
$$

This is a major categorical distinction from the frequentist picture:

$$
I_F(x)\in\text{procedures}
$$

versus

$$
I_B(x)\in\mathcal P(\Theta).
$$

---

### 3. Likelihoodist

Likelihoodism focuses on the map

$$
\theta\mapsto L(\theta;x)
$$

where

$$
L(\theta;x)=p(x\mid\theta).
$$

So its central diagram is:

$$
\boxed{
X
\xrightarrow{\;\;x\;\;}
\operatorname{Likelihood}(\Theta,\mathbb R_{\ge0})
}
$$

or explicitly:

$$
x
\longmapsto
\left[
\theta\mapsto p(x\mid\theta)
\right].
$$

The inferential object is therefore a function

$$
I_L(x):\Theta\to\mathbb R_{\ge0}.
$$

Notice how nicely this connects to your previous question about monoids and functions:

> A likelihood function is literally an element of a function space.

For fixed \(x\),

$$
L_x\in\mathbb R_{\ge0}^{\Theta}.
$$

---

### 4. Akaike / information-criterion paradigm

Here the central object is not primarily a posterior, test, or likelihood function. Instead, models are mapped to a **criterion** measuring predictive/information loss.

For candidate models

$$
\mathcal M=\{M_1,\ldots,M_n\},
$$

AIC assigns

$$
\operatorname{AIC}(M)
=
2k-2\log L(\hat\theta_M;x).
$$

So:

$$
\boxed{
M
\longrightarrow
\hat\theta_M
\longrightarrow
L(\hat\theta_M;x)
\longrightarrow
\operatorname{AIC}(M)
}
$$

and model comparison becomes

$$
\mathcal M
\xrightarrow{\operatorname{AIC}}
\mathbb R.
$$

You then select models according to their criterion values.

---

## Putting the four together

The common categorical skeleton is:

$$
\boxed{
\text{Model}
\longrightarrow
\text{Data}
\longrightarrow
\text{Inferential object}
}
$$

but the four paradigms choose different codomains:

$$
\begin{array}{c|c}
\text{Paradigm} & \text{Inferential object}\\
\hline
\text{Frequentist} & \text{sampling procedures}\\
\text{Bayesian} & \mathcal P(\Theta)\\
\text{Likelihoodist} & \mathbb R_{\ge0}^{\Theta}\\
\text{Akaikean} & \mathbb R^{\mathcal M}
\end{array}
$$

The really interesting abstraction is therefore:

$$
\boxed{
\text{data}
\xrightarrow{\;\;I\;\;}
\text{space of inferential objects}
}
$$

with

$$
I\in
\left\{
I_F,\,
I_B,\,
I_L,\,
I_A
\right\}.
$$

And this gives you a very clean connection to **category theory**: rather than thinking of these as four unrelated philosophies, you can study them as four different ways of mapping the same underlying statistical-model category into different target categories/spaces of objects.
