# 10/19/23
## Inverse-Propensity Weighting
#### Previously
Identification: Control for a confounder, causal effect is identified
#### Intuition
- in an experiment, we randomize treatment independently of covariates
    - want treatment group to look exactly like control group
    - distribution of covariates among the treated units is similar to the distribution of covariates in the control units
        - then we say the covariates are balanced
- in observational study, treatment is assigned in a way that is dependent on covariates
### Imbalance
- covariate imbalance is a problem wheneber covaraiates are also associated with the outcomes
    - on a DAG, a confounder
### correcting imbalance
- how do we correct imbalance in the covariate distribution
    - ex. not many people in 10 year old category but a lot in 24 year old category
        - so maybe we're seeing outcome influenced by an imbalanced covariate distribution
- re weight the treatment units!
### Weighting
- upweight or downweight certain underrepresented units
- in larger datasets with many units, we use propensity scores
- the most popular weighting method is propensity weighting
    - the probability of receiving the treatment is known as the propensity score
    - $e_i=Pr(D_i=1)$
    - $e_i=e(x_i)=Pr(D_i=1|X_i=x_i)$
    - use this score to balance groups
### horowitz thompson estimator
- popular weighted estimator with propensity scores
- inverse probability weighted estimator
- our weights are 1/propensity scores -> do a weighted average
### in real life
- how do we compute propensity scores
- in an observational study we dont know the true value of $e(X_i)$ so we estimate it from the data
- when X is continuous or discrete with many values, we can't stratify
    - use logistic regression to estimate propensity score
    - we do this because our treatment is binary (0,1)

### estimator good?
If our estimator for the propensity score is consistent, then the IPW(Inverse probability weighting) estimator is consistent for the ATE
- logisitc regression is consistent if its assumptions are true

### bootstrap
- to estimate the standard error of our treatment effect estimator with propensity scores

# 11/2/23
## Matchingun
### challenges with inverse probability weighting
- model misspecification: could have not included all the confounders in the model
- extreme weights
### matching
- for each unit i, we find unit j with opposite treatment and most similar covariate values and use their outcome as the missing one for i
- can also help induce balance in the data
- find treatment and control units with the same value as the confounder
- throw away other data
- repeating units that need to be upweighted
- discard units that do not have good matches
#### issue with matching
- we throw away so much data that we increase our standard error by a lot
### intuition of matching
- for each treated unit find the closest control unit: $Y_i(0)$
- imputing the missing potential outcome
- averaging the treatment effects for each treated observation yields an estimator for the ATT. this is called a matching estimator
### why matching
- matching is nonparametric: no need to specify a model like we did with logistic regression
    - only need much milder assumptions
    - matching addresses the curse of dimensionality problem
        - curse: as confounders grow, its really hard to find values that match
- matching is interpretable: estimates can be explained intuitively in terms of who is matched to whom
- matching allows to estimate many different causal quantities
    - ATE, ATT, CATE, etc

### How do we define closeness?
- k nearest neighbor matching -> fix a value of K and choose the K closest units to i
- exact matching
    - only keep unit i on exactly matched covariates

