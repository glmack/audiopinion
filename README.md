## Audiopinion
Audiopinion conducts NLP classification and associated hypothesis testing using Pitchfork music reviews

**Presentation**: [link](https://docs.google.com/presentation/d/1SeczrIDfq1cEGCOKwxKE1LMpYVLcog686coTyoyuS88/edit#slide=id.g59f5cd01eb_0_16)

Functions used in the jupyter notebook can be found in the `functions.py` file.

### **Data understanding**

The data source was Pitchfork music reviews. A PostgreSQL database was created to store the data, via a migration from a SQLite database using a `pgloader` migration tool.

Jupyter notebook for hypothesis tests: [link](https://github.com/glmack/music_reviews/blob/master/tests_1_to_3.ipynb)

Jupyter notebook for classification algorithms testing: [link](https://github.com/glmack/music_reviews/blob/master/binomial_pitchfork_content_sandbox-Copy1.ipynb)

### **Data preparation**
We first apply standard NLP text pre-processing (e.g. tokenization) to the review text/. We represent the vocabulary, which we limited to the top 1000 salient words, through tf-idf (term frequency inverse document frequency) using scikit-learn.

### **Modeling**

**Hypothesis testing**
We explored the relationships between music genres, authors and review scores.

We apply statistical tests (t-texts and z-tests) to answer the following questions:

- Is there a statistical difference between different music genres?
- If there a statistical difference in the ratings of different genres by the same reviewer?
- Is there a statistical difference between the scores produced by one author and the average scores given by the total of authors?

To answer these questions we used Student t-test as well as a z-test to produce p-values necessary to reject or accept null hypothesis. Chosen level of confidence was 95%. The reason to choose this level was to leave a reasonable margin of error considering:

- Subjective nature of reviewing music albums (thus, we cannot make a confidence interval too high)

Findings

Question 1: Is there a statistical difference between 'metal' and 'jazz' music genres?

To compare 'metal' and 'jazz' scores we looked at the scores of metal and jazz genres, taking a random sample of the size 50 from each group.

**Null hypothesis**: there is no statistical difference between reviews scores for 'metal' and 'jazz' genres, that is, the difference in means is equal to 0.

**Alternative Hypothesis**: there is difference between reviews scores for 'metal' and 'jazz' genres: the difference in means is not equal to 0.

Confidence level: 95%

We applied a two-sample t-test. The resulting p-value was smaller that our significance level. So we rejected the null hypothesis in favor of the alternative hypothesis.

### Question 2: If the same person does the review, does he/she score genres differently, in particular, 'metal' and 'jazz' genres?

To answer this questions we performed a 2-tailed paired t-test to see if multigenre reviewrs produced different review scores for 'metal' and 'jazz' genres in particular, since these genres are very different. We took random samples of the size 50 to do the test

**Null hypothesis**: the mean difference between paired observations is equal to zero

**Alternative hypothesis**: the mean difference between paired observations is not equal to zero

Confidence level: 95%

The resulting p-value of p-value is:  0.953 was higher than our significance level aplha=0.05, so we could not reject the null hypothesis.

### Question 3: Looking at the scores produced by one author, are they different from the average scores given by the total of authors? 

To answer that question we looked at reviews made by each particular author, and selected those that did more than 50 reviews. Fewer amount of reviews per author would have less normally distribuited data.

To perform the statistical test, we created a custom-built function that looped through the scores of each author, created a 50 data points random sample and applied the z-test on each sample. The function returned a list of authors, where p-value was lower than alpha level, implying that those are the authors that give higher or lower average review score comparing to the average score of the dataset.

In this series of experiments 36 out of 85 (around 40%) authors have highter or lower average review score than the average of all dataset.

### Question 4: Do the naive bayes classifier and the support vector machine agree in their discrete predictions of music genres, as either rock or non-rock

This workflow uses a McNemar statistic to compare two classification algorithms, a naive bayes classifier and a support vector machine, on a binomial document text classification task using a common corpus of 20,000 music reviews. The objectives of this workflow, which are part of a larger project, are to a) explore and test hypotheses on text classification algorithms on common domains, in terms of relative performance advantages in different scenarios, and b) explore test statistics for comparison of text classification algorithms in varied contexts. The question that we sought to answer is whether the naive bayes classifier and the support vector machine agreed in their discrete predictions of music genres, as either rock or non-rock.

##### Model comparison
The models used for comparison were a naive bayes classifier and a support vector machine, using scikit-learn implementation.

### Evaluation
The null hypothesis for this test, following Dror et al, is that the marginal probability for each outcome (label ‘rock or label ‘not_rock”) is the same for both algorithms. When applying both algorithms on the same music reviews, we expect them to be correct/incorrect on the same proportion of items. 
Test Statistic: This test used statsmodel’s implementation of the McNemar’s statistic, a non-parametric statistic for use in comparing two classifiers on one common domain. This test is appropriate for binary classification tasks on a common domain. The McNemar test has been used in such NLP works and, following Japkowicz, can be thought of as the “non-parametric counterpart of the t-test”. 
The finding: was that the two models did not “agree” in their predictions. The marginal probability of each outcome was not the same for both algorithms.	