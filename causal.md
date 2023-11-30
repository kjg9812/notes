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

# 11/7/23
## Regression
### Linear Regression
- model where we take some input and it comes with some output
- any method of estimating conditional expectation function (CEF) of an outcome Y given X: $E[Y|X]$ 
- assumes that the CEF is linear in the parameters (coefficients) 
### Ordinary Least Squares (OLS) estimator
- how do we come up with good estimate of the coefficients $\beta$
- minimize the squared prediction erros or the sum of squared residuals
### Regression with a binary treatment
- suppose we have a randomized experiment with a binary treatment
- if we have ignorability, then we can write
$$E[Y_i|D_i] = \beta_0 + D_i\tau$$
- how would we get the treatment group mean? adding the control group mean +- the treatment effect -> add the $\beta_0$ and the $\tau$ (what is the treatment group average)
- $D_i$ is the treatment assignment -> intuitively if $D_i$ is 0 then the control estimator is $\beta_0$
### Regression Adjustment
- suppose we have confounders in observational study
- in regression we just add the confounders to the equation
- subset the data on the treatment group to fit the model, then fill in potential outcome for every unit
- then do the same for the control group

# 11/9/23
#### Review of Regression
- fit a model among the control observations, impute Yi(0)
- do the same for treatment and Yi(1)
- bootstrap to get standard errors
- why ATT? sometimes too many control, we get a better view only looking at the treatment
    - the actual causal effect is sometimes the ATT
### assumption
- need regression with constant treatment effects
- under conditional ignorability we need to include the covariates in the model
- what assumptions are we making to interpret $\beta$ as an estimate of the treatment effect?
    - constant treatment effect: treatment effect is constant across units
$\beta_i$ means we're not under the assumtpion
- we can deal with this by adding interaction effects to our regression

#### interaction terms
- what is the $\beta$ here
- $\beta$ is the CATE with covariate = 0 in the example on the slide
##### so how do we get the ATE?
- take a weighted average among the CATES
- de mean the covarriates
- subtract the covariate average from that covariate

# 11/14/23
## Regression and Grouping
#### studying/grades example
- confounder could be different majors
- group data by majors and look within that group; then you see a positive slope
- run a regression within groups, and average those numbers
- we can use fixed effects to adjust for unobservable confounders which are constant within group
#### grouped observations
- - units grouped into G groups: $S_1,...,S_G$
- we have an unobserved confounder U
    - can't condition on U explicitly
    - can exploit that U is constant within groups
- basketball roids example
#### difference between grouped and stratified
- differences are conceptual
- methods for stratification will work on goruped data
- in stratified data:
    - we observe the covariates that we stratify on, whereas in grouped data we do not
    - we create strata so that observed confounders are constant within each strata
    - we know the value of the observed confounder that is associated with a stratum

# 11/16/23
## Difference in Differences
#### ex. does anxiety cause students to perform worse in class?
- cross sectional design: between unit design
    - unit of analysis is each individual person
    - one measure, no time component to this

### time series data
- measure the outcome twice
- panel data
    - panel: tracking a unit over time
- intuitively, two groups in this data
    - the unit itself
    - the time of observation

# 11/21/23
### DID
- assumption: parallel trends assumption
    - what was special about NJ did not change in the period before and after the policy was implemented

# 11/28/23
## Instrumental Variables
### Experimental Motivation
- clinical trial: effect of drug A on heart health
- non-compliance: some people are assigned the treatment but do not take it
- Z: control or treatment
    - being assigned the treatment group
- D: did not take the drug, or took the drug
    - whether or not they comply
    - extra layer of complication -> people may not comply to the rules
    - call this one sided non compliance
- u: unmeasured confounders between compliance rate and outcome
    - potential confounders?
        - culture
        - political leanings
        - superstition
        - religion
        - geography
#### what if you condition on D?
- you will not get a causal effect
    - conditioning on a mediator (post treatment variable)
### Observational Motivation
- causal effect of protest activity on policy change?
- D is protest
- Y is policy
- Z - instrument
    - rain -> less likely to go protest
    - and probably doesn't affect policy change

### notation
- Z is the "instrument"
- Z denotes whether a unit is assigned to treatment. D is whether a unit complies with the treatment
    - we assume Z is randomly assigned

### What do we want to estimate?
- ideally we want the ATE
- the ATE is the effect of being assigned to **and** complyin- Z is the "instrument" with the treatment

### intent to treat effect
- one approach because we can't estimate alpha
- change the quanity of interest - not the effect of the treatment but the effect of assignment to treatment
- our difference in means between group assigned to treatment and the group not assigned to treatment identifies the **intent to treat** effect
- even though D isnt randomized, Z still is

### is the ITT enough?
- the effect of being assigned to treatment
    - this is not the treatment itself
- when could these two be different?
    - whent he process of administering the treatment affects compliance
    - example: there is a psychological effect of being told to take a drug, versus just being offered it as an option
- affects generalizability of TE estimates: we don't know if compliance will be different in other populations

### instrumental variables framework
- dag with Z and U
    - this setting implies:
        - randomization of instrument
        - exclusion restriction
        - first stage relationship
        - monotonicity- Z is the "instrument"
    - also basically to get the effect
        - you can find the effect of Z on D
            - which gives you alpha (DY)
        - and you can find the effect of D on Y
            - which gives you a combination of lambda and alpha (the arrows between ZD and DY)
        - thus to get the effect you can take the combination effect and divide by that alpha to isolate the effect

# 11/30/23
#### 4 major assumptions to make instrumental variables work
1. randomization of instrument
    - z is independent of both sets of potential outcomes
    - no confounder that affects Z that also affects D and Y
2. exclusion restriction
    - Z should not affect Y directly
    - Z only affects Y by way of its effect on D
3. first stage relationship
    - instrument should really be predicting treatment -> high correlation
    - "magnitude" matters for estimator performance - a weak first stage could mean biased instrumental variables estimates
    - IV estimators are consistent under these assumptions but not **unbiased**
    - also called the relevance assumption
4. monotonicity - Z is the "instrument"
    - Zs effect on D only goes in one direction at the individual level

### what do the IV assumptions get us?
- not an ATE, but an Local Average Treatment Effect (LATE)
    - an ATE of a very specific sub population of compliers

#### bad instrument
the more you use a specific instrument like rain, the less likely the exclusion restriction assumption holds