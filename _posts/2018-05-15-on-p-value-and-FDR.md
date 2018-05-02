---
layout: post
title: On the p-value
author: Zhuyi Xue
tags: statistics, p-value
---

<style>
table.confusion-table {
max-width: 100%;
text-align: center;
margin-bottom: 15px;
}

td.true-positive, td.true-negative, span.green {
background-color: #99CC99;
}

td.false-positive, td.false-negative, span.redish {
background-color: #FFCC00;
}

span.true-positive, span.true-negative, span.false-positive, span.false-negative {
text-shadow: -2px 0 white, 0 2px white, 2px 0 white, 0 -2px white;
font-weight: bold;
}

span.true-positive {
color: blue;
}

span.true-negative {
color: #6599FF;
}

span.false-positive {
color: red;
}
span.false-negative {
color: magenta;
}

span.marginal {
font-weight: bold;
}

.nowrap {
white-space: nowrap;
}
</style>

**TL;DR:** 

**Defintion**: P-value is the probability of observing the **current** or **more
extreme data** conditioned on the **null hypothesis being true**. (key words
highlighted). Note it's a conditional probability, and it's NOT only about
observing the current data, but also more extreme ones.

P-value could be written as 

$$\mathrm{Pr}(\left| X \right| \ge x|H_0)$$

where the absolute value ($|X|$) takes both one-sided and two-sided tests into
consideration, and $H_0$ means true null hypothesis.

**Interpretation**: P-value is equal to 1 - specificity, and it does not tell
you anything about sensitivity or false discovery rate (FDR). With a p-value of
0.05, a sensitvity of 0.8, and a prior probability of 0.01 for false null
hypothesis, FDR could be up to 0.86. In other words, 0.05 is too generous a
cutoff in general.

# Calculation of p-value

I have heard of null-hypothesis testing for a long time, but not until recently,
I set out to implements several common testing methods (z-test, t-test,
Mann–Whitney U test, Wilcoxon signed-rank test, Fisher's exact test) myself,
trying to really internalize my understanding. In this post, hypothesis alwaysm
eans null hypothesis unless noted otherwise.

The calculation of p-values follow a consistent pattern. 

* First, the data under consideration is summarized a test statistic. For
example, the statisic is called $Z$ in z-test, $T$ in t-test, $U$ in
Mann-Whiteney U test, $W$ in Wilcoxon signed-rank test. In Fisher's exact test,
with the margins fixed, any of the four cell values could be considered a test
statistic
([ref](https://stats.stackexchange.com/questions/56773/what-is-the-test-statistic-in-fishers-exact-test)).
In a way, a test statstic could be considered as a single-value representation
of the data under consideration.
* Then, by locating the observed value of the test statistic in its probability
distribution, p-value is obtained by integrating over the area that corresponds
to the current statistic value and more extreme ones.

This process would sound familiar if you have done t-test at least once. When I
learned t-test for the first time, I was taught to look up a critical value in
the t-table instead of computing the p-value directly, which could be difficult
without a computer. The critical value is the value of a test static that
corresponds to a desired upper bound of p-value (e.g. 0.05, aka significance
level, $\alpha$). So even without knowning the exact p-value, by comparing my
calculated t value to the critical value, I could know my p-value is above or
below $\alpha$.

You may wonder how do we know about the probability distribution of the test
statistic. It should've been proved when those testing methods were initially
developed. For example, $Z$ follows a normal distribution, $T$ follows a
t-distribution. Both $U$ (Mann–Whitney U test) and $W$ (Wilcoxon signed-rank
test) follow a normal distribution when the sample size is large, so they
essentially become a z-test when the statistic is obtained. For Fisher's exact
test, since the probability distribution is discrete, so all possible values are
calculated and then summed accordingly.

# Distribution of p-values under null hypothesis

Under the null hypothesis is true, p-value follows a uniform distribution. A
brief proof using the
[universality of uniform distribution](https://www.quora.com/What-is-Universality-of-the-Uniform)
for one-sided test is provided below, which could be generalized to two-sided
test.

Let $P = F(X)$, the cumulative distribution function of $X$. Then, $X =
F^{-1}(P)$.

$$
\begin{align*}
P &= \mathrm{Pr}(X \le x | H_0) \\
  &= \mathrm{Pr}(F^{-1}(P) \le x | H_0) \\
  &= \mathrm{Pr}(P \le F(x)  | H_0) \\
  &= \mathrm{Pr}(P \le p | H_0) 
\end{align*}
$$

Hence, P follows a uniform distribution.

# Frequentist interpretation of p-value

So now we know how p-values are calculated, what does a p-value exactly mean? If
the definition in the **TL;DR** section sounds still too abstract, I find it
useful to interpret p-value in the context of a confusion table (thus
frequentist view).

<table class="confusion-table">
  <tr>
    <th></th>
    <th>Condition positive (<span class="marginal">CP</span>) <br> (False null hypotheses)</th>
    <th>Condition negative (<span class="marginal">CN</span>) <br> (True  null hypotheses)</th>
    <th>Row margin</th>
    <th colspan="2">Row metrics</th>
  </tr>
  <tr>
    <td><b>Predicted positive (<span class="marginal">PP</span>) <br> (rejected hypotheses)</b></td>
    <td class="true-positive">True positive (<span class="true-positive">TP</span>)</td>
    <td class="false-positive">False positive (<span class="false-positive">FP</span>) <br> Type I error</td>
    <td class="nowrap"><span class="marginal">PP</span> = <span class="true-positive">TP</span> + <span class="false-positive">FP</span></td>
    <td><span class="nowrap">PPV = <span class="true-positive">TP</span> / <span class="marginal">PP</span></span> <br> Positive predictive rate, <i>Precision</i> </td>
    <td><span class="nowrap">FDR = <span class="false-positive">FP</span> / <span class="marginal">PP</span></span> <br> <i>False discovery rate</i> </td>
  </tr>
  <tr>
    <td><b>Predicted negative (<span class="marginal">PN</span>) <br> (accepted hypotheses)</b></td>
    <td class="false-negative">False negative (<span class="false-negative">FN</span>) <br> Type II error</td>
    <td class="true-negative">True negative (<span class="true-negative">TN</span>)</td>
    <td><span class="nowrap"><span class="marginal">PN</span> = <span class="false-negative">FN</span> + <span class="true-negative">TN</span></span></td>
    <td><span class="nowrap">FOR = <span class="false-negative">FN</span> / <span class="marginal">PN</span></span><br> False omission rate</td>
    <td><span class="nowrap">NPV = <span class="true-negative">TN</span> / <span class="marginal">PN</span></span><br> Negative predictive rate</td>
  </tr>
  <tr>
    <td><strong>Column margin</strong></td>
    <td><span class="marginal">CP</span> = <span class="true-positive">TP</span> + <span class="false-negative">FN</span></td>
    <td><span class="marginal">CN</span> = <span class="false-positive">FP</span> + <span class="true-negative">TN</span></td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td rowspan="2">Column metrics</td>
    <td><span class="nowrap">TPR = <span class="true-positive">TP</span> / <span class="marginal">CP</span></span> <br> True positive rate, <i>Sensitivity</i>, <i>Recall</i>, <i>Power</i></td>
    <td><span class="nowrap">FPR = <span class="false-positive">FP</span> / <span class="marginal">CN</span></span> <br> False positive rate, p-value</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td><span class="nowrap">FNR = <span class="false-negative">FN</span> / <span class="marginal">CP</span></span> <br> False negative rate</td>
    <td><span class="nowrap">TNR = <span class="true-negative">TN</span> / <span class="marginal">CN</span></span> <br> True negative rate, Specificity</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>

The table is adapted from <a
href="https://en.wikipedia.org/wiki/Sensitivity_and_specificity">wiki</a>, with
a more consistent naming and styling convention applied. For example,

* All eight metrics are named with three letters (TPR, FPR, FNR, TNR, PPV, FDR,
FOR, NPV). Their full names, as well as alternative names, are listed below
their respective equations. Relatively more well-known metrics are <i>italicized</i>.
* All eight variables are named with two letters, and they are colored to be more trackable.
  * Marginal sum (CP, CN, PP, PN) are black, while
  * the four key variables (<span class="true-positive">TP</span>, <span
class="true-negative">TN</span>, <span class="false-positive">FP</span>, <span
class="false-negative">FN</span>) are colored differently.
  * background color: <span class="green">green</span> means good, while <span class="redish">redish</span> means bad.
* Aside of CP, CN, PP, PN, I also add their respective description in the
  context of null hypothesis testing.

**Insight**: If you try to match the definition of p-value that is "the
probability of observing the **current** or **more extreme data** conditioned on
that **the null hypothesis being true**", you would focus on the **CN** column
since all hypotheses are true in this column so that it meets the condition. If
p-value is less than a cutoff (e.g. significance level, α=0.05), then hypothesis
is rejected, which results in a false positive. Therefore a p-value is
effectively the FPR (at least in a single-hypothesis testing case), which is
complement to specificity as FPR = 1 - specificity. Therefore, keeping p-value
low is equivalent to keeping specificity high.

While this post is mostly about single-hypothesis testing (SHT), here is a few notes
about p-values in the context of multiple-hypothesis testing (MHT).

* While such interpretation (1 - specificity) applies to the raw p-values in
MHT, the adjusted p-values would have a different interpretation depending on
what kind of type I error rate is being controlled (e.g. per-comparison error
rate (PCER), family-wise error rate (FWER), and false discovery rate (FDR)).
* Because adjusted p-values may have a different interpretation in the MHT case,
the corresponding significance level (α) will also needs interpreted
differently.
* You may have noticed that FDR is also defined in the above table. However, in
reality, usually none of <span class="true-positive">TP</span>, <span
class="true-negative">TN</span>, <span class="false-positive">FP</span>, <span
class="false-negative">FN</span> is known, so they have to be treated as random
variables. As a result, FDR could be defined as an expectation instead, i.e.
E(<span class="false-positive">FP</span>/PP) as in the famous <a
href="https://www.jstor.org/stable/pdf/2346101.pdf?refreqid=excelsior%3Afeea25ba73d9297f8b5983e66f141208"
target="_black">Benjamini-Hochberg correction method</a>.
 
Now, we have estabilished the relationship between p-value and specificity, does
p-value tell us more than that, for example, sensitivity or false discovery rate
(FDR)? Since precision and FDR sum to one, knowing one will get us to the other,
so we'll focus only on FDR.

First, since sensitivity and specificity depend on completely different
variables (<span class="true-positive">TP</span> and <span
class="false-positive">FP</span> vs <span class="true-negative">TN</span> and
<span class="false-negative">FN</span>), and these are unknown variables. So
sensitivity is intractable given specificity). Then, what about FDR? Although
FDR and specificity share <span class="false-positive">FP</span>, it is also
intractable without further information. As noted in <a
href="http://rsos.royalsocietypublishing.org/content/1/3/140216"
target="_blank">An investigation of the false discovery rate and the
misinterpretation of p-values (2014)</a> by David Colquhoun, knowing p-value
gets you "nothing whatsoever about the FDR". This is a bit disappointing, but
crucial to keep in mind so as not to over interprete a p-value.

It turns out there is a simple equation that connects sensitivity, specificity
and FDR. Just one bit more but crucial information is necessary, a prior
probability of a hypothesis being true ($\mathrm{Pr}(H_0)$). Then, we have

$$\mathsf{FDR} = \frac{N\mathrm{Pr}(H_0)(1 - \mathsf{specificity})}{N\mathrm{Pr}(H_0)(1 - \mathsf{specificity}) + N(1 - \mathrm{Pr}(H_0))\mathsf{Sensitivity}}$$

$N$ is the population size, which would be cancelled. This relationship is
easily verifiable according to the confusion table. Let's take a concrete
example first and then draw a 3D surface to illustrate the relationship. Suppose
$\mathrm{Pr}(H_0)=0.99$, specificity is 0.95 (corresponding to the common cutoff
0.05 for p-value), and sensitivity (power) is 0.80. This is a reasonable setup,
not extreme at all, but if you replace the variables with these numbers in the
equation, you get a FDR of 0.86, which effectively renders the test method under
consideration is useless.Remarkably, even when the sensitivity is 0.99, the FDR
is still as low as 0.83.

If you draw the surface of FDR ~ sensitivity & specificity as shown in the first
subplot below

<img src="/assets/FDR_sensitivity_specificity.png" />

The notebook for generating this figure is available at <a
href="https://github.com/zyxue/sutton-barto-rl-exercises/blob/master/stats/relationship-between-FDR-sensitivity-and-specificity.ipynb"
target="_blank">here</a>. So when $r$ (1 - $\mathrm{Pr}(H_0)$) is low, FDR is
mostly near 1 untill specificity becomes extremely high, which explains <a
href="http://rsos.royalsocietypublishing.org/content/1/3/140216"
target="_blank">Colquhoun's advice</a> that "if you wish to keep your false
discovery rate below 5%, you need to ... insist on p≤0.001" (equivalently,
specificity > 0.999). The surface changes as $r$ increases.

# Bayesian interpretation of p-value

As p-value is a conditional probability, we could write it in terms of Bayes
rule, and there comes some new insight.

$$
\mathrm{Pr}(\left|X\right| \ge x | H_0) = \frac{\mathrm{Pr}(H_0 | \left| X \right| \ge x)\mathrm{Pr}(\left| X \right| \ge x)}{\mathrm{Pr(H_0)}}
$$

The left-hand side is what p-value represents. However, what we usually care the
most lies on at the right-hand side that is the probability of null hypothesis
being false given the test statistic, $\mathrm{Pr}(H_0 | \left| X \right| \ge
x)$. Unfortunately, without knowing $\mathrm{Pr}(\left| X \right| \ge x)$ and
$\mathrm{Pr(H_0)}$, we cannot deduce it. Therefore, an important **insight** is
that **rejecting a hypothesis** is very different from saying **the probability
of hypothesis being false is high**, or equivalently, **the probability of the
alternative hypothesis being true**. Actually, they don't have much to
do with each other. Quoting <a
href="http://rsos.royalsocietypublishing.org/content/1/3/140216"
target="_blank">Colquhoun</a> again, rejecting a hypothesis (p-value < a cutoff)
simply means it's worth another look to see if the hypothesis could be false.

# Summary

If you've read the long version, I hope now you have a better sense of the
limited information conveyed by p-value. In summary, p-value is complement to
specificity, keeping p-value low is equivalent to keeping specificity high, but
it provides little clue to the sensitivity or specificity of the test.
**Importantly**, if the prior probability of the hypothesis being false is low
(which is common, e.g. 0.01), then even a very sensitive (e.g. 0.8) and specific
(e.g. 0.95) test could still have an unacceptably high FDR (e.g. 0.86). This is
the main message conveyed in the seemingly absurd paper <a
href="https://doi.org/10.1371/journal.pmed.0020124" target="_blank">Ioannidis
JPA (2005) Why Most Published Research Findings Are False. PLoS Med 2(8):
e124.</a>. But if you understand how such a high FDR is reached and the surface
plot in this post (esp. the first subplot), you may not feel it so absurd
anymore. To me, the widespread of a generally too generous cutoff of 0.05 seems
an unfortunate accident. Such generosity explains why <a
href="https://www.nature.com/news/psychology-journal-bans-p-values-1.17001"
target="_blank">some journal went as far as to ban p-values</a>, but in my
opinion, it is not that p-value is bad, but more of that a p-value of 0.05 is
often too lenient a cutoff. More importantly, if you insist on having p<0.05,
then be conservative with the conclusion, because *extraordinary conclusions
require extraordinary evidence* and the evidence of p<0.05 is usually not so
extraordinary.

This post reflects my current understanding of p-value and null hypothesis
testing, if you see any mistake in the post, please let me know by commenting
below.
