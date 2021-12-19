# EDA-Aspiring Minds outcome 2015
This repository contains the data analysis notebook of the aspiring minds employability outcomes 2015.
## Table of Content
- [About the dataset](#about-the-dataset)
- [Explorary Data Analysis](#eda)
  - [Univariate Analysis](#univariate-analysis)
  - [Bivariate Analysis](#bivariate-analysis)
  - [Analysis of Categorical features](#analysis-of-categorical-features)
- [Research Question](#research-question)

## About the dataset
- Shape of the dataset: 3998 rows and 39 columns i.e 

```df.shape``` ---> (3998,39)
- Numerical features = 27 | Categorical features = 12

## EDA
### Univariate Analysis
Histogram

![histogram]
---

Boxplots

![boxplot]

### Bivariate Analysis

Pairplot

![pairplot]

Hexplots

![Hexplots]

Scatterplot

![scatterplot]

### Analysis of Categorical features

Boxplots

![boxplotsofcat]

## Research Question

Question:
- Times of India article dated Jan 18, 2019 states that “After doing your Computer Science
Engineering if you take up jobs as a Programming Analyst, Software Engineer,
Hardware Engineer and Associate Engineer you can earn up to 2.5-3 lakhs as a fresh
graduate.” Test this claim with the data given to you.
- Is there a relationship between gender and specialisation? (i.e. Does the preference of
Specialisation depend on the Gender?)

Solution:

Plotting swarmplot and boxplot for a better visual observation

![swarmplot_question]

!{boxplot_question]

** Hypothesis Testing **:

Null Hypothesis: Ho <= Rs 300000

ALternate Hypothesis: H1 > Rs 300000

```
from scipy.stats import norm
print("Sample size = {}".format(df.Salary.count()))
print("Sample mean = {}".format(df3.Salary.mean()))
print("Population mean = 300000")
print("sample standard deviation = {}".format(df3.Salary.std()))
```
> Sample size = 3998
Sample mean = 356442.95302013424
Population mean = 300000
sample standard deviation = 152619.43526589242

```
# Calculating the z-critical value for one tail.
confidence_level = 0.95
alpha = 1 - confidence_level
z_critical = norm.ppf(1 - alpha) 
print("z-critical value = {}".format(z_critical))
```
> z-critical value = 1.6448536269514722

```
# Calculating z-score
def z_score(sample_size, sample_mean, pop_mean, pop_std):
    numerator = sample_mean - pop_mean
    denomenator = pop_std / sample_size**0.5
    return numerator / denomenator


z = z_score(df.Salary.count(), df3.Salary.mean(), 300000, df3.Salary.std())

print("z-score = {}".format(z))
```

> z-score = 16.775947991697322

```
if(np.abs(z) > z_critical):
    print("Reject Null Hypothesis")
else:
    print("Fail to reject Null Hypothesis")
```
> Reject Null Hypothesis

```
# Conclusion using p test

p_value = 2 * (1.0 - norm.cdf(np.abs(z)))

print("p_value = ", p_value)

if(p_value < alpha):
    print("Reject Null Hypothesis")
else:
    print("Fail to reject Null Hypothesis")
 
```
> p_value =  0.0
Reject Null Hypothesis

From the hypothesis test,after rejecting the NULL HYPOTHESIS $H_o$, we can conclude that after doing your Computer Science Engineering if one take up jobs as a Programming Analyst, Software Engineer,
Hardware Engineer and Associate Engineer, he or she can earn ***more than 3 lakhs*** as a fresh graduate.

Relationship between Gender and Specialization

```
# Plotting the gender and specialization for visual observation
plt.figure(figsize=(15,18))
sns.histplot(y=df.Specialization, hue=df.Gender)
plt.show()
```
![relation_gender_spec]

Chi-Square Test:

Null Hypothesis $H_o$ = the categorical variables are independent.

Alternate Hypothesis $H_1$ = the categorical variables are dependent.

```
# defining the cross table
cross_tab = pd.crosstab(df['Specialization'],df['Gender'])
cross_tab
```
```
Gender	f	m
Specialization		
aeronautical engineering	1	2
applied electronics and instrumentation	2	7
automobile/automotive engineering	0	5
biomedical engineering	2	0
biotechnology	9	6
ceramic engineering	0	1
chemical engineering	1	8
civil engineering	6	23
computer and communication engineering	0	1
computer application	59	185
computer engineering	175	425
computer networking	0	1
computer science	1	1
computer science & engineering	183	561
computer science and technology	2	4
control and instrumentation engineering	0	1
electrical and power engineering	0	2
electrical engineering	17	65
electronics	0	1
electronics & instrumentation eng	10	22
electronics & telecommunications	28	93
electronics and communication engineering	212	668
electronics and computer engineering	0	3
electronics and electrical engineering	34	162
electronics and instrumentation engineering	5	22
electronics engineering	3	16
embedded systems technology	0	1
industrial & management engineering	0	1
industrial & production engineering	2	8
industrial engineering	1	1
information & communication technology	2	0
information science	0	1
information science engineering	8	19
information technology	173	487
instrumentation and control engineering	9	11
instrumentation engineering	0	4
internal combustion engine	0	1
mechanical & production engineering	0	1
mechanical and automation	0	5
mechanical engineering	10	191
mechatronics	1	3
metallurgical engineering	0	2
other	0	13
polymer technology	0	1
power systems and automation	0	1
telecommunication engineering	1	5
```

```
from scipy.stats import chi2_contingency

stat, p, dof, expected = chi2_contingency(cross_tab)

alpha = 0.05
print("p value = {}".format(p))
if p <= alpha:
    print("{} <= {}".format(p,alpha))
    print("The categorical variables are dependent. So reject Null Hypothesis")
else:
    print("The categorical variables are independent")
```
> p value = 1.2453868176977011e-06
1.2453868176977011e-06 <= 0.05
The categorical variables are dependent. So reject Null Hypothesis

So we can conclude that there is a relattionship between Gender and Specialization. The preference of specialization depends on gender.

