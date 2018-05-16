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

table.confusion-table td:hover {
cursor: pointer;
}

td.true-positive, td.true-negative, td.true-positive-rate, td.true-negative-rate, td.positive-predictive-rate, td.negative-predictive-rate {
background-color: #C0C0C0;
}

td.column-margin, td.condition-positive, td.condition-negative, th.row-margin, td.predicted-positive, td.predicted-negative {
background-color: #F5F5F5;
}

td.true-positive, td.true-negative, td.false-positive, td.false-negative {
font-weight: bold;
}

span.true-positive, span.true-negative, span.false-positive, span.false-negative {
text-shadow: -2px 0 white, 0 2px white, 2px 0 white, 0 -2px white;
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
extreme data** conditioned on a **true null hypothesis**. (key words are
highlighted). Note that it is a conditional probability, and it is NOT only
about observing the current data, but also more extreme ones.

P-value could be written as

$$\mathrm{Pr}(X \le x|H_0)$$

This is the formula for one-tailed test, but is generalizable to the other-tail
or two-tail tests. Here, we will focus on one-tail test. $H_0$ means that null
hypothesis is being true. In this post, I will use hypothesis always to mean
null hypothesis unless noted otherwise.

**Interpretation**: P-value is equal to 1 - specificity, and it provides little
information about sensitivity or false discovery rate (FDR). With a p-value of
0.05, a sensitvity of 0.8, and a prior probability of 0.99 for a null hypothesis
being true, FDR could be as high as 0.86. In other words, 0.05 is too generous a
cutoff for p-value in general.

# Calculation of p-value

I have heard of null-hypothesis testing for a long time, but not until recently,
I set out to implement several common hypothesis testing methods (t-test,
Mann–Whitney U test, Wilcoxon signed-rank test, Fisher's exact test) myself,
trying to really internalize my understanding.

The calculation of a p-value follow a consistent pattern.

* First, the data under consideration is summarized with a test statistic. For
example, the test statisic is called $Z$ in z-test, $T$ in t-test, $U$ in
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
statistic. It should have been proved when the testing methods were initially
developed. For example, $Z$ follows a normal distribution, $T$ follows a
t-distribution. Both $U$ (Mann–Whitney U test) and $W$ (Wilcoxon signed-rank
test) follow a normal distribution when the sample size is large, so they
essentially become z-test once the statistic is obtained. For Fisher's exact
test, since the probability distribution is discrete, so all probabilities
corresponding to the current or more extreme tuplets are calculated and then
summed up.

# Distribution of p-values under null hypothesis

When the null hypothesis is true, p-value follows a uniform distribution. A
brief proof using the
[universality of uniform distribution](https://www.quora.com/What-is-Universality-of-the-Uniform)
is provided below.

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

Therefore, P follows a uniform distribution.

# Frequentist interpretation of p-value

We will interpret the p-value from two perspectives, *frequentist* and
*Bayesian*. Let's start with the frequentist view.

What does a p-value exactly mean? The definition in the **TL;DR** section may
sound still too abstract, I find it very useful to interpret p-value in the
context of a confusion table;thus, it is the frequentist view.

<table class="confusion-table">
  <tr>
    <th></th>
    <th><span class="nowrap">Condition positive</span> <br> (<span class="marginal">CP</span>, False null hypotheses)</th>
    <th><span class="nowrap">Condition negative</span> <br> (<span class="marginal">CN</span>, True  null hypotheses)</th>
    <th class="row-margin">Row margin</th>
    <th colspan="2">Row metrics</th>
  </tr>
  <tr>
    <td><b>Predicted positive (<span class="marginal">PP</span>, rejected hypotheses)</b></td>
    <td class="true-positive">True positive (<span class="true-positive">TP</span>)</td>
    <td class="false-positive">False positive (<span class="false-positive">FP</span>) <br> Type I error</td>
    <td class="predicted-positive"><span class="marginal">PP</span> = <span class="true-positive">TP</span> + <span class="false-positive">FP</span></td>
    <td class="positive-predictive-rate"><span class="nowrap">PPV = <span class="true-positive">TP</span> / <span class="marginal">PP</span></span> <br> Positive predictive rate, <i>Precision</i> </td>
    <td class="false-discovery-rate"><span class="nowrap">FDR = <span class="false-positive">FP</span> / <span class="marginal">PP</span></span> <br> <i>False discovery rate</i> </td>
  </tr>
  <tr>
    <td><b>Predicted negative (<span class="marginal">PN</span>, accepted hypotheses)</b></td>
    <td class="false-negative">False negative (<span class="false-negative">FN</span>) <br> Type II error</td>
    <td class="true-negative">True negative (<span class="true-negative">TN</span>)</td>
    <td class="predicted-negative"><span class="nowrap"><span class="marginal">PN</span> = <span class="false-negative">FN</span> + <span class="true-negative">TN</span></span></td>
    <td class="false-omission-rate"><span class="nowrap">FOR = <span class="false-negative">FN</span> / <span class="marginal">PN</span></span><br> False omission rate</td>
    <td class="negative-predictive-rate"><span class="nowrap">NPV = <span class="true-negative">TN</span> / <span class="marginal">PN</span></span><br> Negative predictive rate</td>
  </tr>
  <tr>
    <td class="column-margin"><strong>Column margin</strong></td>
    <td class="condition-positive"><span class="marginal">CP</span> = <span class="true-positive">TP</span> + <span class="false-negative">FN</span></td>
    <td class="condition-negative"><span class="marginal">CN</span> = <span class="false-positive">FP</span> + <span class="true-negative">TN</span></td>
    <td colspan="3" rowspan="3"></td>
    <!-- <td></td> -->
    <!-- <td></td> -->
  </tr>
  <tr>
    <td rowspan="2">Column metrics</td>
    <td class="true-positive-rate"><span class="nowrap">TPR = <span class="true-positive">TP</span> / <span class="marginal">CP</span></span> <br> True positive rate, <i>Sensitivity</i>, <i>Recall</i>, <i>Power</i></td>
    <td class="false-positive-rate"><span class="nowrap">FPR = <span class="false-positive">FP</span> / <span class="marginal">CN</span></span> <br> False positive rate, <i>p-value</i></td>
    <!-- <td></td> -->
    <!-- <td></td> -->
    <!-- <td></td> -->
  </tr>
  <tr>
    <td class="false-negative-rate"><span class="nowrap">FNR = <span class="false-negative">FN</span> / <span class="marginal">CP</span></span> <br> False negative rate</td>
    <td class="true-negative-rate"><span class="nowrap">TNR = <span class="true-negative">TN</span> / <span class="marginal">CN</span></span> <br> True negative rate, <i>Specificity</i></td>
    <!-- <td></td> -->
    <!-- <td></td> -->
    <!-- <td></td> -->
  </tr>
</table>

The table is adapted from <a
href="https://en.wikipedia.org/wiki/Sensitivity_and_specificity">wiki</a> with a
more consistent naming and styling convention applied. For example,
* All eight metrics are named with three-letter acronyms (TPR, FPR, FNR, TNR, PPV, FDR,
FOR, NPV). Their full names, as well as alternative names, are listed below
their respective equations. Relatively more well-known metrics are <i>italicized</i>.
* All input variables to the equations are named with two letters, and they are
  colored to be more trackable.
  * Marginal sums (<span class="marginal">CP</span>, <span
    class="marginal">CN</span>, <span class="marginal">PP</span>, <span
    class="marginal">PN</span>) are black, while
  * the other four key variables (<span class="true-positive">TP</span>, <span
class="true-negative">TN</span>, <span class="false-positive">FP</span>, <span
class="false-negative">FN</span>) are colored differently.
  * As for background color, those with grey background colors are the variables
    and metrics that the higher, the better. You could see them distributed
    diagonally.
  * Row/Column margins are higlighted in light grey.
  * Hovering a cell of interest will related cells, and you shall see the
    pattern of which metrics are related to which variables. <span
    style="background-color: yellow">Yellow highlight</span> means variables
    needed to calculate the metric under cursor. <span style="background-color:
    orange">Orange highlight</span> means metrics that use the variable under
    cursor.

* Aside of <span class="marginal">CP</span>, <span class="marginal">CN</span>,
  <span class="marginal">PP</span>, <span class="marginal">PN</span>, I also add
  their respective description in the context of null hypothesis testing.

**Insight**: If you try to match the definition of p-value that is "the
probability of observing the **current** or **more extreme data** conditioned on
that **the null hypothesis being true**", you would focus on the **CN** column
since all hypotheses are true in this column so that it meets the condition. If
p-value is less than a cutoff (e.g. significance level, α=0.05), then hypothesis
is rejected, which results in a false positive. Therefore a p-value is
effectively the FPR (at least in a single-hypothesis testing case), which is
complement to specificity as FPR = 1 - specificity. Therefore, keeping the
p-value low is equivalent to keeping the specificity high.

While this post is mostly about single-hypothesis testing (SHT), here I put a
few notes about p-values in the context of multiple-hypothesis testing (MHT).

* While such interpretation (i.e. 1 - specificity) applies to the raw p-values
in MHT, the adjusted p-values would have a different interpretation depending on
what kind of type I error rate is being controlled, e.g. per-comparison error
rate (PCER), family-wise error rate (FWER), or false discovery rate (FDR).
* Because adjusted p-values may have a different interpretation (not related to
specificity anymore) in the MHT case, the corresponding significance level (α)
will also need interpreted differently.
* You may have noticed that FDR is also defined in the above table. However, in
reality, usually none of <span class="true-positive">TP</span>, <span
class="true-negative">TN</span>, <span class="false-positive">FP</span>, <span
class="false-negative">FN</span> is known, so they have to be treated as random
variables. As a result, FDR could be defined as an expectation instead, i.e.
E(<span class="false-positive">FP</span>/PP) as in the famous <a
href="https://www.jstor.org/stable/pdf/2346101.pdf?refreqid=excelsior%3Afeea25ba73d9297f8b5983e66f141208"
target="_black">Benjamini-Hochberg correction method</a>.

Now, we have estabilished the relationship between p-value and specificity, does
p-value tell us more, for example, sensitivity or false discovery rate (FDR)?
Since precision and FDR sum to one, knowing one will get us the other, so we'll
focus only on FDR.

First, since sensitivity and specificity depend on completely different
variables (<span class="true-positive">TP</span> and <span
class="false-positive">FP</span> vs <span class="true-negative">TN</span> and
<span class="false-negative">FN</span>), and these are usually unobserved
variables, even if with margins fixed, sensitivity is intractable given
specificity. Then, what about FDR? Although FDR and specificity share <span
class="false-positive">FP</span>, it is also intractable without further
information. As noted in <a
href="http://rsos.royalsocietypublishing.org/content/1/3/140216"
target="_blank">An investigation of the false discovery rate and the
misinterpretation of p-values (2014)</a> by David Colquhoun, knowing p-value
gets you "nothing whatsoever about the FDR". This is a bit disappointing, but it
is also crucial to keep in mind so as not to over-interpret a p-value.

It turns out there is a simple equation that connects sensitivity, specificity
and FDR. Just one more bit of critical information is needed, the prior
probability of a hypothesis being false ($\mathrm{Pr}(H_0^C)$ or $r$). In the
context of diagnostic method development, the prior probability could be the
probability of NOT having a disease in the population. Then, we have

$$\mathsf{FDR} = \frac{N (1 - r) (1 - \mathsf{specificity})}{N(1 - r)(1 - \mathsf{specificity}) + Nr\mathsf{Sensitivity}}$$

$N$ is the population size, which would be cancelled anyway, so we do not need
to know its exact value. This relationship is easily verifiable according to the
confusion table. Let's take a concrete example first and then draw a 3D surface
to visualize the relationship. Suppose $r=0.01$ (in other words, the disease is
rare), specificity is 0.95 (corresponding to the common cutoff of 0.05 for
p-value), and sensitivity (power) is 0.80. This is a reasonable setup, not
extreme at all, but if you replace the variables with these numbers in the
equation, you get a FDR of 0.86, which effectively renders the test method under
consideration useless. Remarkably, even when the sensitivity is 1, the FDR is
still as high as 0.83. It turns out that increasing sensitivity has a limited
effect on the FDR when the $r$ is low and specificity is not high enough (<
0.999), which is seen from the following plots for $r=0.01$ and different
specificities.

<a href="/assets/FDR_sensitivity.png" target="_blank" ><img src="/assets/FDR_sensitivity.png" alt="FDR vs sensitivity" /></a>

The surface of FDR ~ sensitivity & specificity is shown below for different $r$ values.

<a href="/assets/FDR_sensitivity_specificity.png" target="_blank"><img src="/assets/FDR_sensitivity_specificity.png" alt="FDR vs sensitivity and specificity"/></a>

Note when $r = 0.01$ or low in general, FDR is mostly near 1 until specificity
becomes extremely high, which explains <a
href="http://rsos.royalsocietypublishing.org/content/1/3/140216"
target="_blank">Colquhoun's advice</a> that "if you wish to keep your false
discovery rate below 5%, you need to ... insist on p≤0.001" (i.e. specificity >
0.999). The surface changes as $r$ increases.

The notebook for generating this figure is available at <a
href="https://github.com/zyxue/sutton-barto-rl-exercises/blob/master/stats/relationship-between-FDR-sensitivity-and-specificity.ipynb"
target="_blank">here</a>. In it, I also plotted FDR ~ rate & sensitivity for
different specificities.

<a href="/assets/FDR_rate_sensitivity.png" target="_blank" ><img src="/assets/FDR_rate_sensitivity.png" alt="FDR vs rate and sensitivity" /></a>


Note when the specificity is very high (0.9999 or 0.9999, in other words, p <
0.0001 or p < 0.001), then the surface is mostly around 0 unless when the rate
is extremely low (see the sharp color transitions around the edges, beyond which
the FDR shoots up like a rocket).

# Bayesian interpretation of p-value

As p-value is a conditional probability, we could write it in terms of Bayes
rule, and there comes to be some more insight.

$$
\mathrm{Pr}(X \le x | H_0) = \frac{\mathrm{Pr}(H_0 | X \le x)\mathrm{Pr}(X \le x)}{\mathrm{Pr(H_0)}}
$$

The left-hand side is the p-value. However, what we usually care the most lies
on at the right-hand side that is the probability of null hypothesis being true
or false given the test statistic, $\mathrm{Pr}(H_0 | X \le x)$. Unfortunately,
without knowing $\mathrm{Pr}(X \le x)$ and $\mathrm{Pr(H_0)}$, we cannot
calculate its exact value. Therefore, an important **insight** from this
observation is that **rejecting a hypothesis** is very different from saying
**the probability of the hypothesis being true is low or high**, or
equivalently, **the probability of the alternative hypothesis being true is high
or low**. Actually, they don't have much to do with each other. Quoting <a
href="http://rsos.royalsocietypublishing.org/content/1/3/140216"
target="_blank">Colquhoun</a> again, rejecting a hypothesis (e.g. p-value <
0.05) simply means it's worth another look. But you might get a sense already
that a cutoff of 0.05 is likely to be really generous, if not too generous.

# Summary

If you've read the long version, I hope you now have a better sense of the
limited information conveyed by a p-value. In summary, a p-value is complement
to specificity, keeping the p-value low is equivalent to keeping the specificity
high, but it provides little information to the sensitivity or FDR of the test.
**Importantly**, if the prior probability of the hypothesis being false is low
which could be common (0.01), then even a very sensitive (e.g. 0.8) and specific
(e.g. 0.95) test could still have an unacceptably high FDR (e.g. 0.86). I
thought this is the main message conveyed in the seemingly absurd paper <a
href="https://doi.org/10.1371/journal.pmed.0020124" target="_blank">Ioannidis
JPA (2005) Why Most Published Research Findings Are False. PLoS Med 2(8):
e124.</a>. But if you understand how such a high FDR is calculated and the
surface plots shown in this post, you may not feel it so absurd anymore.

To me, the widespread of a generally too generous cutoff of 0.05 appears to be
an unfortunate accident due to miunderstanding of a p-value. Such generosity
explains why <a
href="https://www.nature.com/news/psychology-journal-bans-p-values-1.17001"
target="_blank">some journal went as far as to ban p-values</a>, but in my
opinion, it is not that p-value is bad, but more of that a p-value of 0.05 is
often too generous a cutoff. Perhaps More importantly, if you insist on having
p<0.05, then be conservative with the conclusion, because *extraordinary
conclusions require extraordinary evidence* and the evidence of a p<0.05 is
usually not so extraordinary.

This post reflects my current understanding of p-value and null hypothesis
testing, if you see any mistake in the post, please let me know by commenting
below.


<script>
function higlight(srcSelector, tgtSelector, color) {
    // higlight src itself as well
    tgtSelector = srcSelector + ', ' + tgtSelector;

   $(srcSelector)
       .mouseenter(function () {
           $(tgtSelector).each(function() {
               let elem = $(this);
               elem.attr('originalBackgroundColor', $(elem).css('background-color'));
               elem.css('background-color', color);
           })
               })
       .mouseleave(function() {
           $(tgtSelector).each(function() {
               let elem = $(this);
               elem.css('background-color', elem.attr('originalBackgroundColor'));
           })
       });
};

higlight('td.true-positive-rate', 'td.true-positive, td.condition-positive', 'yellow');
higlight('td.false-negative-rate', 'td.false-negative, td.condition-positive', 'yellow');
higlight('td.condition-positive', 'td.true-positive, td.false-negative', 'yellow');

higlight('td.false-positive-rate', 'td.false-positive, td.condition-negative', 'yellow');
higlight('td.true-negative-rate', 'td.true-negative, td.condition-negative', 'yellow');
higlight('td.condition-negative', 'td.false-positive, td.true-negative', 'yellow');

higlight('td.positive-predictive-rate', 'td.true-positive, td.predicted-positive', 'yellow');
higlight('td.false-discovery-rate', 'td.false-positive, td.predicted-positive', 'yellow');
higlight('td.predicted-positive', 'td.true-positive, td.false-positive', 'yellow');

higlight('td.false-omission-rate', 'td.false-negative, td.predicted-negative', 'yellow');
higlight('td.negative-predictive-rate', 'td.true-negative, td.predicted-negative', 'yellow');
higlight('td.predicted-negative', 'td.false-negative, td.true-negative', 'yellow');



higlight('td.true-positive', 'td.condition-positive, td.true-positive-rate, td.predicted-positive, td.positive-predictive-rate', 'orange');
higlight('td.false-negative', 'td.condition-positive, td.false-negative-rate, td.predicted-negative, td.false-omission-rate', 'orange');
higlight('td.false-positive', 'td.condition-negative, td.false-positive-rate, td.predicted-positive, td.false-discovery-rate', 'orange');
higlight('td.true-negative', 'td.condition-negative, td.true-negative-rate, td.predicted-negative, td.negative-predictive-rate', 'orange');


</script>
