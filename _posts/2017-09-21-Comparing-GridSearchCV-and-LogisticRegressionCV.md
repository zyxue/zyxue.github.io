---
layout: post
title: Comparing GridSearchCV and LogisticRegressionCV
author: Zhuyi Xue
tags: python, machine-learning, scikit-learn, grid-search
---


**TL;NR**: GridSearchCV for logisitc regression and
LogisticRegressionCV are effectively the same with very close
performance both in terms of model and running time, at least with the
following parameter settings.

See more discussion on https://github.com/scikit-learn/scikit-learn/issues/6619

Below is a short summary. The data used is RNA-Seq expression data
from [The Cancer Genome Atlas (TCGA)](https://cancergenome.nih.gov/).

```
_clf = LogisticRegression()
gs = GridSearchCV(
    _clf,
    param_grid={'C': Cs, 'penalty': ['l1'],
                'tol': [1e-10], 'solver': ['liblinear']},
    cv=skf,
    scoring='neg_log_loss',
    n_jobs=4,
    verbose=1,
    refit=True)
%time gs.fit(Xs, ys)

lrcv = LogisticRegressionCV(
    Cs=Cs, penalty='l1', tol=1e-10, scoring='neg_log_loss', cv=skf,
    solver='liblinear', n_jobs=4, verbose=0, refit=True,
    max_iter=100,
)
%time lrcv.fit(Xs, ys)
```
The results:

```
gs.best_score_
# -0.047306741321593591

lrcv.scores_[1].mean(axis=0).max()
# -0.047355806767691064
```

Well, the difference is rather small, but consistently captured. I
wonder if there is other reason beyond randomness. Since the solver is
`liblinear`, there is no warm-starting involved here.
