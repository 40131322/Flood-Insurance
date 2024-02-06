
Flood Insurance Policy

Table of Contents
Introduction	4
I.MODEL SELECTION	5
1.1 Summary of the Data Set	5
1.2 Log transformation	5
1.3 Graphs	8
1.4 Assumptions Identification and Validation	9
1.5 Choice of Distributions to Model Data	10
II. ESTIMATION	11
2.1 Method of Moments	11
2.2 Maximum Likelihood Estimation (MLE)	13
2.3 Asymptotic Covariance Matrix of MLE estimators	16
III. GOODNESS-OF-FIT TESTS	18
3.1.1 Probability Density and Cumulative Distribution Functions	18
3.1.2 Mean Residual Life (MRL) and Limited Expected Value (LEV)	20
3.1.3 P-P and Q-Q Plots	21
3.1.3.1 Gamma	22
3.1.3.2 Weibull	23
3.1.3.3 Normal	23
3.1.3.4 Logistic	24
3.2 Summary Statistics Comparisons	24
3.3 Formal Goodness-of-Fit Tests	25
3.3.1 Anderson-Darling Test	25
3.3.2 Kolmogorov-Smirnov Test	26
IV. CONCLUSIONS	27
4.1 Final Model Selection	27
4.2 Possible Uses and Limitations of the Fitted Model	27
V. Bibliography	28
VI. Appendix	29
6.1 R code	29

 

Introduction
Flooding is a significant natural disaster that can result in severe financial damages for individuals, businesses and communities affected by it. Flood insurance is a crucial tool for mitigating the risks of floods, and an accurate estimation of insurance claims is essential for insurers to effectively manage that risk. Actuarial and statistical data modeling is a powerful tool that can be used by actuaries to analyze and predict the distribution of flood insurance claims, aiding insurers to make optimal decisions.
In this paper, I propose a comprehensive approach for fitting flood insurance claims to a statistical distribution. I begin by providing an overview of the flood insurance claim data and then review existing statistical methods for fitting data to distributions and discuss their goodness of fit.

 
I.MODEL SELECTION
1.1 Summary of the Data Set
The data set consists of claim amounts resulting from flooding for a group of individuals under the flood insurance policy, with 2009 claims recorded in the database.  For each of these claims, the data provides the aggregate claim amount and number of claims. In this study, we aim to analyze individual claims only as IID data, as such we removed all aggregate claim data with more than one claim. This trimming results in a dataset of 1950 individual claims.
1.2 Log transformation
The following table shows the statistics that were found from a brief analysis of the dataset. The statistics were calculated on both the original data and the log-transformed data.
Sample Statistic of the Claim Amount	Value of the original data	Value of Log-transformed data
Mean	13028.57	7.61
Variance	3769002756.57	3.02
Standard deviation	61392.20	1.74
Coefficient of Variation	4.71	0.23
Skewness	10.60	0.32
Kurtosis	138.38	3.32
Minimum	25.00	3.22
Maximum	1093267.00	13.90
Median	2132.50	7.67

Percentile	Value of the original data	Value of Log-transformed data
5%	110.00	4.70
10%	184.90	5.22
25%	645.00	6.47
50%	2132.50	7.67
75%	5684.25	8.65
90%	15552.30	9.65
95%	42752.55	10.66

We see the original data being extreme values in many key statistics which are brought to a normal with the log transformation.
To get a further understanding of the data, two scatter plots were generated.
  
In the scatter plot of the original data, most of the claims are less than 1000$ with only a few outliers above this amount. It is also highly skewed (>3) and has a very high kurtosis (>100). By using the log-transformed data the skewness and kurtosis are greatly reduced. The skewness is slightly greater than zero and the kurtosis is a little bit above 3 meaning the data is slightly skewed to the right and has a peak that is a taller than a normal distribution.
    
The MRL of the base data seems to follow no clear direction and have high variance and no clear indication of the distribution of the claim amounts. However, if we follow the Logged Empirical MRL , we see a clear downward slope, indicating the log transformed data may follow Gamma, Weibull, Logistic or Normal distribution.
As for the percentiles, the growth is much more constant in the log-transformed data, whereas the original data increases much more steeply at the larger percentiles compared to the smaller ones. From this analysis of the dataset, it was concluded that the log-transformed data would be used to complete the rest of the project.
From this analysis of the dataset, it was concluded that the log-transformed data would be used to complete the rest of the project.
1.3 Graphs
The histogram of the data could lead to the conclusion that the data follows a log-normal distribution, a log-gamma distribution, a log-Weibull distribution, or a log-logistic distribution.
 

1.4 Assumptions Identification and Validation
The data is assumed to be independent and identically distributed. To verify the validity of this assumption, the time series autocorrelation function (ACF) learned in class STAT460, showing the relationship between data observations as a function of the time lag between them, as well as the Ljung-Box test were used.
The following ACF plot was obtained at the 95% confidence level:
 
Clearly, all lags except for lag 0 and 3 other are within the 95% confidence bounds, represented by the dotted blue lines. This makes sense as the ACF at lag 0, the correlation of a times series with itself, equals to 1 by default. This result supports our initial assumption. To further validate our assumption, the Ljung-Box test was taken at a p-value of 0.05.
Ljung-Box Test
H0: The data Xi’s are independent and identically distributed.
Ha: The data Xi’s are not independent and identically distributed.
Test Statistic: Q = n(n+2) Σpk2 / (n-k), where n=sample size and pk=sample ACF at lag k
Lag	Test Statistic Q	P-value
5	4.7809	0.4432
10	5.3388	0.8674
15	14.576	0.4824
The p-values of 0.4432, 0.8674, and 0.4824, at lags 5, 10, and 15, respectively, are much larger than 0.05. Therefore, there is insufficient evidence to reject the null hypothesis of the test and thus conclude that the sample data are independent and identically distributed.
1.5 Choice of Distributions to Model Data
The data is almost perfectly symmetrical as it has a skewness very close to 0. Its kurtosis of 3.95 indicates a higher peak (relative to the normal distribution) around the mean value. The empirical mean residual lifetime is a decreasing function for the most part, suggesting a light-tailed distribution.
These characteristics lead to the Weibull and Gamma distributions being selected to model the data. Both distributions are light-tailed and can be made symmetrical depending on the parameters used.
They are also commonly used to model claim costs. 
II. ESTIMATION
There are many different techniques to estimate the parameters of a model. In this study, the method of moments and maximum likelihood estimation are considered. 
2.1 Method of Moments
The method of moments is a method of estimation of population, in our case, the flood insurance claims. We start by expressing the populations moments as functions of the parameters of interest, and find as many moments as parameters of interest, and solve the equations to find the parameters.
For Normal distributions:
 

So
E(X)=μ
E(X^2 )-μ^2=σ^2
μ=7.608308
σ^2=3.020338
For Gamma distributions:
 

α= 19.175353
β=2.520318
For Weibull Distributions:
E(X)=αΓ(1+1/β)=1/n ∑_(i=1)^n▒x_i 
E(X^2 )=α^2 Γ(1+2/β)=1/n ∑_(i=1)^n▒〖x_i〗^2 
α=4.28466
β= -2.16883


For Logistic distributions:
E(X)=α=1/n ∑_(i=1)^n▒x_i 
E(X^2 )=(ß^2 π^2)/3+α^2=1/n ∑_(i=1)^n▒〖x_i〗^2 
α=7.608308
β= 0.9579154
 
2.2 Maximum Likelihood Estimation (MLE)
Maximum likelihood estimation (MLE) is a widely used statistical method for estimating unknown parameters in a probability distribution model. The MLE of the Normal, Gamma, Weibull and Logistic distributions will be shown below.
By selecting the parameter values that maximize the probability of obtaining the observed data
Function	Estimated Parameter 1	Estimated Parameter 2
Normal	Mean	Standard deviation
Gamma	Share	Rate
Weibull	Share	Scale
Logistic	Location	Scale

 

Function	Normal	Gamma	Weibull	Logistic
Likelihood	∏_(i=1)^n▒〖f(〗 x_i; μ,σ^2)=(1/√(2πσ^2 ))^n e^((-1/(2σ^2 ) ∑_(i=1)^n▒〖(x_i-μ)〗^2 ) )	∏_(i=1)^n▒〖f(〗 x_i; α,ß)=ß^(α+n) ∏_(i=1)^n▒〖x_i〗^((α-1) ) 
e^(-ß∑_i^n▒x_i )/〖(Γ(α))〗^n	∏_(i=1)^n▒〖f(〗 x_i; α,ß)=ß^(-(α+n)) ∏_(i=1)^n▒x_i 
e^(〖-(∑_i^n▒x_i/ß)〗^α )	∏_(i=1)^n▒〖f(〗 x_i; μ,s)=
1/4s ∏_(i=1)^n▒〖〖sech〗^2 ((x_i-µ)/2s)〗
Log Likelihood	-n/2  ln⁡(2π)*2/n  ln⁡( σ^2 )
-1/(2σ^2 ) ∑_(i=1)^n▒〖(x_i-μ)〗^2   	(α-1) ∑_(i=1)^n▒ln⁡(x_i ) 
+nα ln⁡(ß)
-ß∑_(i=1)^n▒x_i 
-nln(Γ(α))	n(ln⁡(α)-α ln⁡(ß) )+∑_(i=1)^n▒ln⁡(x_i ) -〖(∑_i^n▒x_i/ß)〗^α
	-ln4s+
2∑_(i=1)^n▒〖ln⁡(sech⁡((x_i-µ)/2s))〗

Parameter 1	δl/(δµ)=1/σ^2  ∑_(i=1)^n▒〖(x_i-μ)〗  	δl/δα=αn/ß^2	δl/δα=n(ln⁡(1/α)-ln⁡(ß) )
-(∑_i^n▒x_i/ß)^α ln⁡(∑_i^n▒x_i/ß)	δl/(δµ)=
2∑_(i=1)^n▒〖(tanh⁡((x_i-µ)/2s) 〗(-1/2s)  


Parameter 2	δl/(δσ^2 )=-n/σ^2 
+〖(1/σ^2 )〗^2 ∑_(i=1)^n▒〖(x_i-μ)〗^2   	δl/δß=∑_(i=1)^n▒x_i +αn/ß	δl/δß=-αn/ß
+α(∑_i^n▒x_i )^α/ß^(α+1)	δl/δs=-1/4s
+2∑_(i=1)^n▒tanh⁡((x_i-µ)/2s) *((x_i-µ)/s^2 )


 
Function	Estimated Parameter 1	Estimated Parameter 2
Normal	7.608308	1.737466
Gamma	18.760511	2.465733
Weibull	4.625225	8.297061
Logistic	7.5923243	0.9797538


 
2.3 Asymptotic Covariance Matrix of MLE estimators
While the MLE is an excellent method of measure to fit parameters, there’s is a level of uncertainty or variability associated with the estimated parameters values obtained through it. As such, the asymptotic variance will be used to construct confidence intervals on the parameters.
Function	Parameter 1	Parameter 2	Variance Parameter 1	Variance 
Parameter 2
Normal	7.608308	1.737466	0.0015481	0.000774046
Gamma	18.760511	2.465733	0.35468	0.00629307
Weibull	4.625225	8.297061	0.0058281	0.00184797
Logistic	7.5923243	0.9797538	0.0014911	0.000341744

2.3.1 Covariance Matrix
Normal	mean	sd
mean	0.001548	0
sd	0	0.000774046

Gamma	Shape	rate
Shape	0.35468	0.04661632
Rate	0.046616	0.00629307

Weibull	shape	scale
shape	0.005828	0.001073632
scale	0.001074	0.00184797


Logistic	location	scale
location	0.001149	-1.06196E-05
scale	-1.06E-05	0.000341744
2.3.2 Confidence Bounds
Here, it is important to consider that the Asymptotic Covariance Matrix all include covariance of each parameter is close to 0. As such, we will not consider their relationship to each other and their confidence array.
	Parameter 1	Parameter 1
Function	Lower bound	Upper Bound	Lower bound	Upper Bound
Normal	7.531	7.685	1.683	1.792
Gamma	17.593	19.928	2.310	2.621
Weibull	4.476	4.775	8.213	8.381
Logistic	7.517	7.668	0.944	1.016

 

III. GOODNESS-OF-FIT TESTS
3.1.1 Probability Density and Cumulative Distribution Functions
 
The cumulative distribution function (CDF) graph shows that the fitted Gamma, Weibull, Normal and Logistic distribution all follow the empirical distribution. Compared to the empirical data, the Gamma, Weibull, and normal distribution assign to little weight to the earlier claims and too little towards the middle to end values of the logged claims. However, we see the Weibull distribution fits better at the beginning of the distribution while the Logistic distribution fits better overall and follows the empirical more closely. The Normal and Gamma Fitted distribution fails to properly close the gap between itself and the empirical distribution at the beginning and the end points, where we see severe underweighting in the smaller claims and heavier distributions in the heavier claims.
 
When looking at the probability density function (PDF), contrary to the CDF, we see that while the Logistic Fitted distribution follows the small and large values of the empirical PDF, it fails to demonstrate the large increase in data in the middle, which the Gamma, Weibull and Normal distributions do better. The normal distribution seems to reflect the PDF the most accurately, with its density lines following the most accurately the histogram distribution, with the Gamma fitted distribution failing to match its mode with the empirical distribution and the Weibull distribution spreading the weights of the distribution too much to the extremities, generate tails too heavy for the empirical distribution.

3.1.2 Mean Residual Life (MRL) and Limited Expected Value (LEV)
   
The Mean Residual life (MRL) is the expected paument of a claim with a deductible d. MRL functions fo a distributions are observed in comparison to the MRL of an exponential distribution, where the MRL function follows a constant. By observing the MRL, we can see that if the MRL function is decreasing, the distribution will be lighter-tailed than the exponential distribution and if the MRL is increasing, the distribution will be heavier-tailed than the exponential distribution. In our case, we see that the MRL is, overall, a decreasing function aside from when we reach large deductible values, indicating to us that we are working with distributions with lighter tails than exponential, such as the Gamma, Weibull, Normal and Logistic distribution that are shown in the graph above. The fluctuation at larger deductibles starting at 8 are most likely due to a lack of data at larger values.
 
The Limited Expected Value (LEV) is the expected payment for a claim with a policy limit u. When analysing the discrete data, it is often important to consider the LEV function rather than the CDF as it is a confitnous function. When a fitted distribution LEV follows closely the LEV of the empirical data, we can deduct that the fitted distribution is a good match for the data. The LEV graph above shows that all fitted distributions follows the Empirical LEV closely, with a the Logistic distribution tracing it the best and the Weibull distribution failing to trace the Empircal data at the values between 6 and 10. However, we can see all data converges towards the Empirical LEV starting from 8, which is close to the mean of the Empirical log-transformed data of 7.61

3.1.3 P-P and Q-Q Plots
The P-P plot, the probability plot, is created by ordering the observations in increasing orders. If the fitted distribution model follows the P-P plot of the empirical data well, returned a near perfect 45-degree line, or in other words, follows a x=y function, the fitted distribution is a good fit for the dataset. A strong deviation from the main diagonal unit indicator shows a poor fit for the specific dataset.

The Q-Q plot, or the Quantile plot, is a plot of the quantiles where two distributions against each other. This plot is used to compare the pattern of points in the plot. Similar to the P-P plot, if the fitted distribution deviates too much from the empirical scatter plot, then the fitted model is not appropriate for the specific data.
3.1.3.1 Gamma
   
The Gamma Distribution sees the P-P Plot of the Gamma distribution to stray often from the center line, although often converging back to the center, indicating a poor fit. However, the Q-Q plot follows the indicator line the best out of all the distributions, staying close to the line even at the highest and lowest percentiles.
3.1.3.2 Weibull
   
The Weibull distribution sees deviation from the indicator line of the P-P plot at the lowest Empirical cdf (0,0.05) and significant deviation at (0.6,0.9), signifying that this fitted distribution may not be the best fit for the claims. Similarly, the Q-Q plot indicates poor fit for the Weibull model and the data, as the Q-Q plot strongly deviates at lowest and highest percentiles.
3.1.3.3 Normal
   
The Normal distribution contends the strongest fit in terms of following the indicator line in the P-P plot with the Logistic distributiuon, only straying slightly from the centerline. However, the Q-Q plot  indicates that, just like the Weibull distribution, it fails to follow the empirical data at lower and higher percentile of Data
3.1.3.4 Logistic
   
The Logistic fitted distribution has a P-P plot following the centerline very closely, with moderate deviation between (0,0.2).  The Q-Q plot also follows the indicator line well, but fails to stay close at lower percentiles, indicate that it may not be the best fit for this dataset either.
3.2 Summary Statistics Comparisons
Summary Statistic	Empirical (log-transformed)	Gamma	Weibull	Normal	Logistic
Mean	7.608308	7.608493	7.583756	7.608308	7.592324
Variance	3.020338	3.085693	3.478074	3.018789	18.4443
Standard Deviation	1.737912	1.756614	1.864959	1.737466	4.294683
Coefficient of Variation	0.228423	0.230875	0.245915	0.228364	1.767843
Skewness	0.320216	0.461751	-0.19862	0	0
Kurtosis	3.218876	3.319821	2.825417	3	1.2
Median	7.66505	5	7.664959	7.608308	7.592324

Percentile	Empirical (log-transformed)	Gamma	Weibull	Normal	Logistic
5%	4.70	4.97	4.37	4.75	4.71
10%	5.22	5.46	5.10	5.38	5.44
25%	6.47	6.36	6.34	6.44	6.52
50%	7.61	7.61	7.58	7.61	7.59
75%	8.65	8.71	8.90	8.78	8.87
90%	9.65	9.93	9.94	9.83	9.75
95%	10.66	10.71	10.52	10.47	10.48
Overall, the summary statistics are not very different from the empirical to the fitted distributions. 
3.3 Formal Goodness-of-Fit Tests
Goodness-of-Fit tests are widely used in statistical modeling analysis to assess adequacy of a fitted distribution for a given dataset. Two of the commonly used tests are the Anderson-Darling test and the Kolmogorov-Smirnov test, which are utilized to evaluate the validity of certain distributions by comparing observed data with the expected values of the data given certain distributions.
3.3.1 Anderson-Darling Test
The Anderson-Darling test was performed at a level of significance of 5% with hypotheses:
H0: The observed data set comes from the fitted distribution.
Ha: The observed data set does not come from the fitted distribution
The Anderson-darling test uses the following test statistic to test the previous hypotheses.
Test Statistic:
 
The following test statistics were produced for the Gamma, Weibull, Normal and Logistic Distributions with the use of R.
	Gamma	Weibull	Normal	Logistic
Test Statistic A2	11.877	13.086	6.97	5.8826
P-value	3.409e-07	3.077e-07	0.0003409	0.001096
Conclusion	Reject H0	Reject H0	Reject H0	Reject H0
Since the P-values for all four distributions are smaller than 0.05 and the test statistic A2 is larger than 2.492, the null hypotheses must be rejected at a level of significance of 5%. Therefore, the observed data does not come from the Gamma, Weibull, Normal or Logistic Distributions. The P-value is however the largest for the Logistic distribution which indicates that the Logistic distribution is the best fit from the three selected distributions.
3.3.2 Kolmogorov-Smirnov Test
The Kolmogorov-Smirnov test was performed at a level of significance of 5% with hypotheses:
H0: The observed data set comes from the fitted distribution.
Ha: The observed data set does not come from the fitted distribution
Critical value at 5%: 1.36/√n=1.36/√1950=0.030797935
The Kolmogorov-Smirnov test statistic is:
 
Where t is the left truncating point, while u is the right censoring point.
	Gamma	Weibull	Normal	Logistic
Test Statistic D	0.056782	0.056782	0.036035	0.04354
P-value	2.244e-06	0.01264	9.397e-10	0.001231
Conclusion	Reject H0	Reject H0	Reject H0	Reject H0
We can see that all distributions have a test statistic D larger than 0.030797935 and smaller P-value than 5%, the null hypothesis must be rejected for all of them. However, the Normal Distribution sees the closest value to reach close to the critical value of Test Statistic D and the Weibull and Logistic distribution has significantly larger P-Values than other distributions, both these statistics may be considered for the best fit for the flood insurance data.
IV. CONCLUSIONS
4.1 Final Model Selection
In conclusion, with the support of the goodness-of-fit tests, the Gamma, Weibull, Normal and Logistic fitted distributions are not fit for the data. However, the logistic distribution seems to fit best to the log transformed distribution overall, given the CDF, the PDF, the LEV, the MRL and the AD and KS tests.
Test	Gamma	Weibull	Normal	Logistic
CDF				x
PDF	x	x	x	
MRL				x
LEV	x	x	x	x
P-P plot			x	x
Q-Q plot	x			
AD test				
KS test				
Total Score	3	2	3	4
 By scoring every distribution by how well they passed each Goodness-of-fit test, we can see that the Logistic distribution fits best on the log transformed dataset.
 4.2 Possible Uses and Limitations of the Fitted Model
Data modeling is used often in the insurance market. The fitted models can be used to generate statistically sound prediction and models to reduce risk and calculate valuation reserves, the amount of money an insurance company requires to be able to pay out its future claims. The fitted models can also be used to price policies and generate more profitable policy limits and deductibles using the LEV and MRL graphs of the fitted models. However, there are some visible limitations to using fitted models.
Data availability and quality can pose a significant limitation to the accuracy of data. Data that are sparce, incomplete or containing errors may impact the accuracy and reliability of the models they produce using them.
The dynamic nature of the insurance business may cause the data to struggle to catch up to the evolving nature of risk, such as the change in risk and claim behavior due to Covid recently.
Data privacy legislation and regulatory constraints put on insurance data may limit access and use of data for the purposes of modeling, which leads back to the data availability.
Insurance policies and regulations can change over time, like claim data on certain drugs or certain procedures. The change in policy limits, terms and regulatory requirements may change up the nature of the claim behavior enough to negate the accuracy of the data and therefore reduce the validity of the data models in the insurance context.
V. Bibliography
Groparu-Cojocaru, I. (2001). Lecture 8: Asymptotic Variance of MLE. Montreal: Concordia University.
Stuart A. Klugman, H. H. (1949). Loss Models From Data to Decisions. New Jersey: John Wiley & Sons.

 
VI. Appendix
6.1 R code
install.packages("goftest")
install.packages("dplyr")   
install.packages("readxl")
install.packages("moments")
install.packages("ggplot2")
install.packages("fitdistrplus")
install.packages("actuar")
install.packages("EnvStats")
install.packages("ParetoPosStable")
install.packages("pracma")
install.packages("e1071")
install.packages("stats4")
install.packages("ADGofTest")
install.packages("FAdist")
install.packages("fBasics")
install.packages("matlib")
install.packages("openxlsx")

library(goftest)
library(ggplot2)
library(dplyr)               
library(moments)
library(readxl)
library(actuar)
library(ADGofTest)
library(fitdistrplus)
library(EnvStats)
library(ParetoPosStable)
library(pracma)
library(e1071)
library(stats4)
library(FAdist)
library(fBasics)
library(matlib)
library(openxlsx)

#Base
FloodInsurance = read_xlsx("/Users/cheng/Documents/floodinsurance.xlsx",sheet = )
Base.FloodInsurance<- FloodInsurance %>% filter(N!=2) 
Base.FloodInsurance<- Base.FloodInsurance$`Claim X`
sorted.Base.FloodInsurance<-sort(Base.FloodInsurance)


#Log Transformed
log.FloodInsurance<-log(Base.FloodInsurance)
sorted.log.FloodInsurance<-sort(log(Base.FloodInsurance))

#Parameters
mean(Base.FloodInsurance)
range(Base.FloodInsurance)
median(Base.FloodInsurance)
var(Base.FloodInsurance)
sd(Base.FloodInsurance)
mean(log.FloodInsurance)
range(log.FloodInsurance)
median(log.FloodInsurance)
var(log.FloodInsurance)
sd(log.FloodInsurance)

mean(log.FloodInsurance)
var(log.FloodInsurance)

# scatter plots
plot(Base.FloodInsurance,main="Scatter Plot of Original Claim Amounts")
plot(log.FloodInsurance,main="Scatter Plot of Log-Transformed Claim Amounts")

#Quantiles
quantile(Base.FloodInsurance, c(0.05, 0.1, 0.25, 0.5, 0.75, 0.9,0.95))
quantile(log.FloodInsurance, c(0.05, 0.1, 0.25, 0.5, 0.75, 0.9,0.95))

#Boxplot
B<-boxplot(Base.FloodInsurance,xlab="Claim Amount",ylab="Frequency")
BL<-boxplot(log.FloodInsurance,xlab="Log-Transformed Claim Amount",ylab="Frequency")

#Coefficient of variation, skewness, kurtosis
cv<-function(x){sd(x)/mean(x)}
skew<-function(x){mean((x -mean(x))^3)/sd(x)^3}
kurt<-function(x){mean((x-mean(x))^4)/sd(x)^4}
cv(Base.FloodInsurance)
cv(log.FloodInsurance)
skew(Base.FloodInsurance)
skew(log.FloodInsurance)
kurt(Base.FloodInsurance)
kurt(log.FloodInsurance)

#Histograms
hist (Base.FloodInsurance,
      main="Histogram for Amount", xlab="Amount",las=1)
hist (log.FloodInsurance, prob=T, breaks=seq(from=2, to =15, by=0.5),xlim=c(2,9), main="Histogram for Logged
Amount", xlab="Amount",las=1)


#Proceed with log transformed Data
#Empirical CDF function
cdf<- function(x){
  a<-x
  for(i in 1:length(x) ) { a[i]<-sum(x<=x[i])/length(x)
  }
  return(a)
}
amount.fn<-cdf(sorted.log.FloodInsurance)
plot(sorted.log.FloodInsurance,amount.fn,type="s",xlab="Claims",ylab="F_n",main="Empirical vs. Fitted
Normal CDF")

#Empirical MRL function:
emrl<- function(x){ a<-x
x.Fn<-cdf(x)
for(i in 1:length(x)) {(if(x[i]<max(x))(a[i]<-sum(x[x-x[i]>0]-x[i])/(length(x))/(1-x.Fn[i])) else
  a[i]<-0) }
return(a)}

amount.mrl<-emrl(sorted.log.FloodInsurance)
plot(sorted.log.FloodInsurance,amount.mrl,type="l",xlab="Claims",ylab="Mean Excess",main="Logged Empirical
MRL")
amount.base.mrl<-emrl(sorted.Base.FloodInsurance)
plot(sorted.Base.FloodInsurance,amount.base.mrl,type="l",xlab="Claims",ylab="Mean Excess",main="Base Empirical
MRL")

#Empirical LEV
amount.lev<-mean(sorted.log.FloodInsurance)-amount.mrl*(1-amount.fn)
plot(sorted.log.FloodInsurance, amount.lev, type="l",xlab="Claims",ylab="Limited Expected Value",
     main="Empirical LEV",las=1)

#Empirical MRL and Empirical LEV:
par(mfrow=c(1,2))
plot(sorted.log.FloodInsurance,amount.mrl,type="l",xlab="Claims",ylab="Mean Excess",main="Empirical
MRL")
plot(sorted.log.FloodInsurance, amount.lev, type="l",xlab="Claims",ylab="Limited Expected
Value",main="Empirical LEV",las=1)

#test for independence
par(mfrow=c(1,1))
iid_test <- acf(x= log.FloodInsurance, type = "correlation")
iid_test
#seems to be a WN

#Ljung-Box test
test1 = Box.test(log.FloodInsurance,lag=5,type= "Ljung-Box")
test2 = Box.test(log.FloodInsurance,lag=10,type= "Ljung-Box")
test3 = Box.test(log.FloodInsurance,lag=15,type= "Ljung-Box")
test1
test2
test3

#Normal Q-Q plot
par(mfrow=c(1,1))
qqnorm(log.FloodInsurance)
qqline(log.FloodInsurance)

#P-P Plot
y<-pnorm(sorted.log.FloodInsurance,mean(log.FloodInsurance),var(log.FloodInsurance))
plot(amount.fn,y,type="p", main="P-P plot (normal)",
     xlab="F_n (x)",ylab="F(x)")
abline(a=0,b=1,col="red")

#Fitting distributions: "mle"=maximum likelihood function "mme"=moment matching
#"qme"=quantile matching "mge"=maximizing goodness-of -fit

gamma_fit_mme <- fitdist(log.FloodInsurance, "gamma",method = c("mme"))
normal_fit_mme <- fitdist(log.FloodInsurance, "norm",method = c("mme"))
lognormal_fit_mme <- fitdist(log.FloodInsurance,"lnorm",method = c("mme"))
logistic_fit_mme <- fitdist(log.FloodInsurance,"logis",method = c("mme"))

gamma_fit <- fitdist(log.FloodInsurance, "gamma")
weibull_fit <- fitdist(log.FloodInsurance, "weibull")
normal_fit <- fitdist(log.FloodInsurance, "norm")
lognormal_fit <- fitdist(log.FloodInsurance,"lnorm")
logistic_fit <- fitdist(log.FloodInsurance,"logis")

#Fitted parameters of gamma & cov matrix

gamma_fit$estimate
gamma_cov<-gamma_fit$vcov
summary(gamma_fit)
plot(gamma_fit)

#Fitted parameters of Weibull & cov matrix

weibull_fit$estimate
weibull_cov<-weibull_fit$vcov
summary(weibull_fit)
plot(weibull_fit)

#Fitted parameters of Normal & cov matrix

normal_fit$estimate
normal_cov<-normal_fit$vcov
summary(normal_fit)
plot(normal_fit)

#Fitted parameters of lognormal & cov matrix

lognormal_fit$estimate
lognormal_cov<-lognormal_fit$vcov
summary(lognormal_fit)
plot(lognormal_fit)

#Fitted parameters of Normal & cov matrix

logistic_fit$estimate
logistic_cov<-logistic_fit$vcov
summary(logistic_fit)
plot(logistic_fit)

###GAMMA

# cdf of fitted gamma dist
gamma_fit_cdf <- function(x) pgamma(x, shape = gamma_fit$estimate["shape"], rate =
                                      gamma_fit$estimate["rate"])
# Survival function of fitted gamma dist
gamma_fit_sf <- function(x) 1 - gamma_fit_cdf(x)
# Density function of fitted gamma dist
gamma_fit_dens <- function(x) dgamma(x, shape = gamma_fit$estimate["shape"], rate =
                                       gamma_fit$estimate["rate"])
# Lev of fitted gamma dist
lev_gamma <- function(t) integrate(f = gamma_fit_sf, lower = 0, upper = t)$value
# mrl of fitted gamma dist
mrl_gamma <- function(t) (lev_gamma(Inf) - lev_gamma(t))/gamma_fit_sf(t)

###WEIBULL
weibull_fit
# cdf of fitted weibull dist
weibull_fit_cdf <- function(x) pweibull(x, shape = weibull_fit$estimate["shape"], scale =
                                          weibull_fit$estimate["scale"])
# Survival function of fitted weibull dist
weibull_fit_sf <- function(x) 1 - weibull_fit_cdf(x)
# Density function of fitted weibull dist
weibull_fit_dens <- function(x) dweibull(x, shape = weibull_fit$estimate["shape"], scale =
                                           weibull_fit$estimate["scale"])
# Lev of fitted weibull dist
lev_weibull <- function(t) integrate(f = weibull_fit_sf, lower = 0, upper = t)$value
# mrl of fitted weibull dist
mrl_weibull <- function(t) (lev_weibull(Inf) - lev_weibull(t))/weibull_fit_sf(t)

###NORMAL

# cdf of fitted normal dist
normal_fit_cdf <- function(x) pnorm(x, mean = normal_fit$estimate["mean"], sd =
                                      normal_fit$estimate["sd"])
# Survival function of fitted normal dist
normal_fit_sf <- function(x) 1 - normal_fit_cdf(x)
# Density function of fitted normal dist
normal_fit_dens <- function(x) dnorm(x, mean = normal_fit$estimate["mean"], sd =
                                       normal_fit$estimate["sd"])
# Lev of fitted normal dist
lev_normal <- function(t) integrate(f = normal_fit_sf, lower = 0, upper = t)$value
# mrl of fitted normal dist
mrl_normal <- function(t) (lev_normal(Inf) - lev_normal(t))/normal_fit_sf(t)

###LOGNORMAl

(lognormal_fit)
# cdf of fitted lognormal dist
lognormal_fit_cdf <- function(x) plnorm(x, meanlog = lognormal_fit$estimate["meanlog"], sdlog =
                                      lognormal_fit$estimate["sdlog"])
# Survival function of fitted lognormal dist
lognormal_fit_sf <- function(x) 1 - lognormal_fit_cdf(x)
# Density function of fitted lognormal dist
lognormal_fit_dens <- function(x) dlnorm(x, meanlog = lognormal_fit$estimate["meanlog"], sdlog =
                                           lognormal_fit$estimate["sdlog"])
# Lev of fitted lognormal dist
lev_lognormal <- function(t) integrate(f = lognormal_fit_sf, lower = 0, upper = t)$value
# mrl of fitted lognormal dist
mrl_lognormal <- function(t) (lev_lognormal(Inf) - lev_lognormal(t))/lognormal_fit_sf(t)

###LOGISTIC

# cdf of fitted logistic dist
logistic_fit_cdf <- function(x) plogis(x, location = logistic_fit$estimate["location"], scale =
                                          logistic_fit$estimate["scale"])
# Survival function of fitted logistic dist
logistic_fit_sf <- function(x) 1 - logistic_fit_cdf(x)
# Density function of fitted logistic dist
logistic_fit_dens <- function(x) dlogis(x, location = logistic_fit$estimate["location"], scale =
                                          logistic_fit$estimate["scale"])
# Lev of fitted logistic dist
lev_logistic <- function(t) integrate(f = logistic_fit_sf, lower = 0, upper = t)$value
# mrl of fitted logistic dist
mrl_logistic <- function(t) (lev_logistic(Inf) - lev_logistic(t))/logistic_fit_sf(t)



# Empirical cdf vs fitted cdf's
l<-length(sorted.log.FloodInsurance)
ecdfPlot(sorted.log.FloodInsurance, ecdf.lwd = 1, main = "Cumulative Distribution Function", xlab =
           "log(claims)")
fitted_gamma <- sapply(X = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), gamma_fit_cdf)
lines(x = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), y = fitted_gamma, col = "blue")
fitted_weibull <- sapply(X = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), weibull_fit_cdf)
lines(x = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), y = fitted_weibull, col = "cadetblue")
fitted_normal <- sapply(X = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), normal_fit_cdf)
lines(x = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), y = fitted_normal, col = "deepskyblue")

fitted_logistic <- sapply(X = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), logistic_fit_cdf)
lines(x = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), y = fitted_logistic, col = "lightblue")

legend("bottomright", c("Empirical","Gamma fit", "Weibull fit","Normal fit","Logistic fit"), fill=c("black","blue","cadetblue",
                                                                                     "deepskyblue","lightblue"))
# Graph of empirical vs fitted lev's
plot(elev(sorted.log.FloodInsurance), type="l", xlab="u", ylab="LEV(u)", main="LEV")
fitted_gamma <- sapply(X = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), lev_gamma)
lines(x = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), y = fitted_gamma, col = "blue")
fitted_weibull <- sapply(X = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), lev_weibull)
lines(x = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), y = fitted_weibull, col = "cadetblue")
fitted_normal <- sapply(X = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), lev_normal)
lines(x = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), y = fitted_normal, col = "deepskyblue")

fitted_logistic <- sapply(X = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), lev_logistic)
lines(x = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), y = fitted_logistic, col = "lightblue")


legend("bottomright", c("Empirical","Gamma fit", "Weibull fit","Normal fit","Logistic fit"), fill=c("black","blue","cadetblue",
                                                                                                    "deepskyblue","lightblue"))

# Graph of empirical vs fitted mrl's
plot(x=sorted.log.FloodInsurance,y=emrl(sorted.log.FloodInsurance), type="l", xlab= "d", ylab="e(d)", main ="MRL")
fitted_gamma <- sapply(X = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), mrl_gamma)
lines(x = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), y = fitted_gamma, col = "blue")
fitted_weibull <- sapply(X = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), mrl_weibull)
lines(x = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), y = fitted_weibull, col = "cadetblue")
fitted_normal <- sapply(X = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), mrl_normal)
lines(x = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), y = fitted_normal, col = "deepskyblue")

fitted_logistic <- sapply(X = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), mrl_logistic)
lines(x = seq(min(log.FloodInsurance),max(log.FloodInsurance),length=l), y = fitted_logistic, col = "lightblue")


legend("bottomright", c("Empirical","Gamma fit", "Weibull fit","Normal fit","Logistic fit"), fill=c("black","blue","cadetblue",
                                                                                     "deepskyblue","lightblue"))
# Gamma model
gamma_fit_parameters <- gamma_fit$estimate
cdf_gamma <- sort(pgamma(log.FloodInsurance,shape = gamma_fit_parameters[1],rate =
                           gamma_fit_parameters[2]))
n <- length(log.FloodInsurance)
#PP plot for gamma
p <- (1 : n) / n - 0.5 / n #empirical CDF
plot(x=p,y=cdf_gamma, main="Gamma P-P Plot", xlab ="Empirical CDF", ylab = "Gamma CDF")
lines(x=p,y=p,col="blue")
legend("bottomright", c("(Fn, F_Fitted)","X = Y"), cex=0.9, fill=c("black","blue"))

#QQ plot for gamma
gamma_quantile_fitted <- vector()
for (i in 1:n){
  gamma_quantile_fitted[i] = qgamma(i/(n+1),shape = gamma_fit_parameters[1],rate =
                                      gamma_fit_parameters[2])
}
plot(x=sorted.log.FloodInsurance,y=gamma_quantile_fitted, main="Gamma Q-Q Plot", xlab="Empirical Percentile",
     ylab="Gamma
Percentile" )
lines(x=sorted.log.FloodInsurance,y=sorted.log.FloodInsurance,col = "blue")
legend("bottomright", c("(Empirical Percentile,Gamma Percentile)","X = Y"), cex=0.9,
       fill=c("black","blue"))

# Weibull model
WB_fit <- fitdist(log.FloodInsurance,"weibull")
WB_fit_parameters <- WB_fit$estimate
cdf_WB <- sort(pweibull(log.FloodInsurance,shape = WB_fit_parameters[1],scale = WB_fit_parameters[2]))
n <- length(log.FloodInsurance)
#PP plot for Weibull
p <- (1 : n) / n - 0.5 / n #empirical CDF
plot(x=p,y=cdf_WB, main="Weibull P-P Plot",xlab ="Empirical CDF", ylab = "Weibull CDF")
lines(x=p,y=p,col="cadetblue")
legend("bottomright", c("(Fn, F_Fitted)","X = Y"), cex=0.9, fill=c("black","cadetblue"))
#QQ plot for Weibull
WB_quantile_fitted <- vector()
for (i in 1:n){
  WB_quantile_fitted[i] = qweibull(i/(n+1),shape = WB_fit_parameters[1],scale =
                                     WB_fit_parameters[2])
}
plot(x=sorted.log.FloodInsurance,y=WB_quantile_fitted,main="Weibull Q-Q Plot",xlab ="Empirical Percentile", ylab
     = "Weibull
Percentile")
lines(x=sorted.log.FloodInsurance,y=sorted.log.FloodInsurance,col = "cadetblue")
legend("bottomright", c("(Empirical Percentile,Weibull Percentile)","X = Y"), cex=0.9,
       fill=c("black","cadetblue"))

#Normal model
normal_fit_parameters <- normal_fit$estimate
cdf_normal <- sort(pnorm(log.FloodInsurance,mean = normal_fit_parameters[1],sd =
                           normal_fit_parameters[2]))
n <- length(log.FloodInsurance)
#PP plot for normal
p <- (1 : n) / n - 0.5 / n #empirical CDF
plot(x=p,y=cdf_normal, main="Normal P-P Plot", xlab ="Normal Empirical CDF", ylab = "Normal CDF")
lines(x=p,y=p,col="deepskyblue")
legend("bottomright", c("(Fn, F_Fitted)","X = Y"), cex=0.9, fill=c("black","deepskyblue"))
#QQ plot for normal
normal_quantile_fitted <- vector()
for (i in 1:n){
  normal_quantile_fitted[i] = qnorm(i/(n+1),mean = normal_fit_parameters[1],sd =
                                      normal_fit_parameters[2])
}
plot(x=sorted.log.FloodInsurance,y=normal_quantile_fitted, main="Normal Q-Q Plot", xlab="Empirical Percentile",
     ylab="Normal
Percentile" )
lines(x=sorted.log.FloodInsurance,y=sorted.log.FloodInsurance,col = "deepskyblue")
legend("bottomright", c("(Empirical Percentile,Normal Percentile)","X = Y"), cex=0.9,
       fill=c("black","deepskyblue"))

#Lognormal model
lognormal_fit_parameters <- lognormal_fit$estimate
cdf_lognormal <- sort(plnorm(log.FloodInsurance,meanlog = lognormal_fit_parameters[1],sdlog =
                              lognormal_fit_parameters[2]))
n <- length(log.FloodInsurance)
#PP plot for lognormal
p <- (1 : n) / n - 0.5 / n #empirical CDF
plot(x=p,y=cdf_lognormal, main="Lognormal P-P Plot", xlab ="Empirical CDF", ylab = "lognormal CDF")
lines(x=p,y=p,col="dodgerblue")
legend("bottomright", c("(Fn, F_Fitted)","X = Y"), cex=0.9, fill=c("black","dodgerblue"))
#QQ plot for lognormal
lognormal_quantile_fitted <- vector()
for (i in 1:n){
  lognormal_quantile_fitted[i] = qlnorm(i/(n+1),meanlog = lognormal_fit_parameters[1],sdlog =
                                         lognormal_fit_parameters[2])
}
plot(x=sorted.log.FloodInsurance,y=lognormal_quantile_fitted, main="Lognromal Q-Q Plot", xlab="Empirical Percentile",
     ylab="lognormal
Percentile" )
lines(x=sorted.log.FloodInsurance,y=sorted.log.FloodInsurance,col = "dodgerblue")
legend("bottomright", c("(Empirical Percentile,lognormal Percentile)","X = Y"), cex=0.9,
       fill=c("black","dodgerblue"))


#Logistic model
logistic_fit_parameters <- logistic_fit$estimate
cdf_logistic <- sort(plogis(log.FloodInsurance,location = logistic_fit_parameters[1],scale =
                              logistic_fit_parameters[2]))
n <- length(log.FloodInsurance)
#PP plot for Logistic
p <- (1 : n) / n - 0.5 / n #empirical CDF
plot(x=p,y=cdf_logistic, main="Logistic P-P Plot", xlab ="Empirical CDF", ylab = "logistic CDF")
lines(x=p,y=p,col="lightblue")
legend("bottomright", c("(Fn, F_Fitted)","X = Y"), cex=0.9, fill=c("black","lightblue"))
#QQ plot for Logistic
logistic_quantile_fitted <- vector()
for (i in 1:n){
  logistic_quantile_fitted[i] = qlogis(i/(n+1),location = logistic_fit_parameters[1],scale =
                                         logistic_fit_parameters[2])
}
plot(x=sorted.log.FloodInsurance,y=logistic_quantile_fitted, main="Logistic Q-Q Plot", xlab="Empirical Percentile",
     ylab="logistic
Percentile" )
lines(x=sorted.log.FloodInsurance,y=sorted.log.FloodInsurance,col = "lightblue")
legend("bottomright", c("(Empirical Percentile,logistic Percentile)","X = Y"), cex=0.9,
       fill=c("black","lightblue"))

#summary stats of fitted gamma
gamma_fit_parameters
gamma.shape<-gamma_fit_parameters[1]
gamma.rate<-gamma_fit_parameters[2]
gquartile_1 <- qgamma(0.25,shape = gamma_fit_parameters[1],rate = gamma_fit_parameters[2])
gquartile_1
gquartile_3 <- qgamma(0.75,shape = gamma_fit_parameters[1],rate = gamma_fit_parameters[2])
gquartile_3
gpercentile_0.05 <- qgamma(0.05,shape = gamma_fit_parameters[1],rate =
                             gamma_fit_parameters[2])
gpercentile_0.05
gpercentile_0.10 <- qgamma(0.10,shape = gamma_fit_parameters[1],rate =
                             gamma_fit_parameters[2])
gpercentile_0.10
gpercentile_0.90 <- qgamma(0.90,shape = gamma_fit_parameters[1],rate =
                             gamma_fit_parameters[2])
gpercentile_0.90
gpercentile_0.95 <- qgamma(0.95,shape = gamma_fit_parameters[1],rate =
                             gamma_fit_parameters[2])
gpercentile_0.95
gmedian <- qgamma(0.50,shape = gamma_fit_parameters[1],rate = gamma_fit_parameters[2])
gmedian
gmean <- gamma_fit_parameters[1]/gamma_fit_parameters[2]
gmean
gstd <- sqrt(gamma_fit_parameters[1]/(gamma_fit_parameters[2])^2)
gstd
gvar<-gstd^2
gvar

gcoeff_var <- gstd/gmean
gcoeff_var
gskew <- 2/sqrt(gamma_fit_parameters[1])
gskew
gkurt <- 3+6/(gamma_fit_parameters[1])
gkurt


#summary stats of fitted weibull
WB_fit_parameters
weibull.shape<-WB_fit_parameters[1]
weibull.scale<-WB_fit_parameters[2]

wquartile_1 <- qweibull(0.25,shape = WB_fit_parameters[1],scale = WB_fit_parameters[2])
wquartile_1
wquartile_3 <- qweibull(0.75,shape = WB_fit_parameters[1],scale = WB_fit_parameters[2])
wquartile_3
wpercentile_0.05 <- qweibull(0.05,shape = WB_fit_parameters[1],scale = WB_fit_parameters[2])
wpercentile_0.05
wpercentile_0.10 <- qweibull(0.10,shape = WB_fit_parameters[1],scale = WB_fit_parameters[2])
wpercentile_0.10
wpercentile_0.90 <- qweibull(0.90,shape = WB_fit_parameters[1],scale = WB_fit_parameters[2])
wpercentile_0.90
wpercentile_0.95 <- qweibull(0.95,shape = WB_fit_parameters[1],scale = WB_fit_parameters[2])
wpercentile_0.95
wmedian <- qweibull(0.50,shape = WB_fit_parameters[1],scale = WB_fit_parameters[2])
wmedian
k = WB_fit_parameters[1] # shape parameter
lambda = WB_fit_parameters[2] # scale parameter
Wmean = lambda*gamma(1 + (1/k)) # Mean
Wmean
Wvar = (lambda^2)*(gamma(1+(2/k)) - (gamma(1+(1/k)))^2); # Variance
Wvar
WStaDev = (Wvar^0.5)
WStaDev
wcoeff_var <- WStaDev/Wmean
wcoeff_var
wskew = (gamma(1+(3/k))*(lambda^3)-3*(Wmean)*(Wvar)-(Wmean^3))/(WStaDev^3)
wskew
wkurt =
  ((lambda^4)*gamma(1+(4/k))-4*(wskew)*(WStaDev^3)*(Wmean)-6*(Wmean^2)*(WStaDev^2)-(
    Wmean^4))/(WStaDev^4) 
wkurt

#summary stats of fitted normal
norm.mean<-normal_fit_parameters[1]
norm.sd<-normal_fit_parameters[2]

nquartile_1 <- qnorm(0.25,mean = normal_fit_parameters[1],sd = normal_fit_parameters[2])
nquartile_1
nquartile_3 <- qnorm(0.75,mean = normal_fit_parameters[1],sd = normal_fit_parameters[2])
nquartile_3
npercentile_0.05 <- qnorm(0.05,mean = normal_fit_parameters[1],sd = normal_fit_parameters[2])
npercentile_0.05
npercentile_0.10 <- qnorm(0.10,mean = normal_fit_parameters[1],sd = normal_fit_parameters[2])
npercentile_0.10
npercentile_0.90 <- qnorm(0.90,mean = normal_fit_parameters[1],sd = normal_fit_parameters[2])
npercentile_0.90
npercentile_0.95 <- qnorm(0.95,mean = normal_fit_parameters[1],sd = normal_fit_parameters[2])
npercentile_0.95
nmedian <- qnorm(0.50,mean = normal_fit_parameters[1],sd = normal_fit_parameters[2])
nmedian
nmean <- normal_fit_parameters[1]
nmean
nstd <-normal_fit_parameters[2]
nstd
nvar<-nstd^2
nvar
ncoeff_var <- nstd/nmean
ncoeff_var
nskew <- 0
nskew
nkurt <- 3
nkurt

#Summary stats of fitted lognormal
meanlog<-lognormal_fit_parameters[1]
sdlog<-lognormal_fit_parameters[2]
lnquartile_1 <- qlnorm(0.25,meanlog = lognormal_fit_parameters[1],sdlog =lognormal_fit_parameters[2])
lnquartile_1
lnquartile_3 <- qlnorm(0.75,meanlog = lognormal_fit_parameters[1],sdlog =lognormal_fit_parameters[2])
lnquartile_3
lnpercentile_0.05 <- qlnorm(0.05,meanlog = lognormal_fit_parameters[1],sdlog =lognormal_fit_parameters[2])
lnpercentile_0.05
lnpercentile_0.10 <- qlnorm(0.10,meanlog = lognormal_fit_parameters[1],sdlog =lognormal_fit_parameters[2])
lnpercentile_0.10
lnpercentile_0.90 <- qlnorm(0.90,meanlog = lognormal_fit_parameters[1],sdlog =lognormal_fit_parameters[2])
lnpercentile_0.90
lnpercentile_0.95 <- qlnorm(0.95,meanlog = lognormal_fit_parameters[1],sdlog =lognormal_fit_parameters[2])
lnpercentile_0.95
lnmedian <- qlnorm(0.50,meanlog = lognormal_fit_parameters[1],sdlog =lognormal_fit_parameters[2])
lnmedian
lnmean <- exp(lognormal_fit_parameters[1]+lognormal_fit_parameters[2]^2/2)
lnmean
lnvar<-(exp(lognormal_fit_parameters[2]^2-1)*exp(2*lognormal_fit_parameters[1]+lognormal_fit_parameters[2]))
lnvar
lnsd<-lnvar^0.5
lnsd
lncv<-lnsd/lnmean
lncv
lnskew<-(exp(sdlog)+2)*sqrt(exp(sdlog^2)-1)
lnkurt<-exp(4*sdlog^2)+2*exp(3*sdlog^2)+3*exp(2*sdlog^2)-6
lnkurt

#Summary stats of fitted logistic
logistic_fit_parameters
location.logis<-logistic_fit_parameters[1]
scale.logis<-logistic_fit_parameters[2]
logisquartile_1 <- qlogis(0.25,location = logistic_fit_parameters[1],scale =logistic_fit_parameters[2])
logisquartile_1
logisquartile_3 <- qlogis(0.75,location = logistic_fit_parameters[1],scale =logistic_fit_parameters[2])
logisquartile_3
logispercentile_0.05 <- qlogis(0.05,location = logistic_fit_parameters[1],scale =logistic_fit_parameters[2])
logispercentile_0.05
logispercentile_0.10 <- qlogis(0.10,location = logistic_fit_parameters[1],scale =logistic_fit_parameters[2])
logispercentile_0.10
logispercentile_0.90 <- qlogis(0.90,location = logistic_fit_parameters[1],scale =logistic_fit_parameters[2])
logispercentile_0.90
logispercentile_0.95 <- qlogis(0.95,location = logistic_fit_parameters[1],scale =logistic_fit_parameters[2])
logispercentile_0.95
logismedian <- qlogis(0.50,location = logistic_fit_parameters[1],scale =logistic_fit_parameters[2])
logismedian
logismean <- location.logis
logismean
logisvar <- scale.logis^2*location.logis^2/3
logisvar
logissd<-logisvar^0.5
logissd
logiscv<-logismean/logissd
logiscv
logis.skew<-0
logis.skew
logis.kurt<-6/5
logis.Kurt

#Anderson-Darling test
ad.test(log.FloodInsurance,pgamma, gamma.shape,gamma.rate)
ad.test(log.FloodInsurance,pweibull, weibull.shape,weibull.scale)
ad.test(log.FloodInsurance,pnorm, norm.mean,norm.sd)
ad.test(log.FloodInsurance,plnorm,meanlog,sdlog)
ad.test(log.FloodInsurance,plogis,location.logis,scale.logis)

#Kolmogorov-Smirnov Tests
ks.test(log.FloodInsurance,pgamma, gamma.shape,gamma.rate)
ks.test(log.FloodInsurance,pweibull, weibull.shape,weibull.scale)
ks.test(log.FloodInsurance,pnorm, norm.mean,norm.sd)
ks.test(log.FloodInsurance,plnorm,meanlog,sdlog)
ks.test(log.FloodInsurance,plogis,location.logis,scale.logis)




###REPORT DATA EXTRACTION

##Sample Statistics of the Claims
base.data.parameters<-c(mean(Base.FloodInsurance),var(Base.FloodInsurance),sd(Base.FloodInsurance),cv(Base.FloodInsurance),skew(Base.FloodInsurance),kurt(Base.FloodInsurance),min(Base.FloodInsurance),max(Base.FloodInsurance),quantile(Base.FloodInsurance,0.5))
log.data.parameters<-c(mean(log.FloodInsurance),var(log.FloodInsurance),sd(log.FloodInsurance),cv(log.FloodInsurance),skew(log.FloodInsurance),kurt(log.FloodInsurance),min(log.FloodInsurance),max(log.FloodInsurance),quantile(log.FloodInsurance,0.5))

raw.data.parameters<-data.frame(base.data.parameters,log.data.parameters)
rownames(raw.data.parameters)<-c("mean","var","sd","cv","skew","kurt","min","max","median")
raw.data.parameters
write.xlsx(raw.data.parameters,file='/Users/cheng/Documents/report_data.xlsx',sheetName='Parameters')
##Percentiles
base.quantile<-quantile(Base.FloodInsurance, c(0.05, 0.1, 0.25, 0.5, 0.75, 0.9,0.95))
log.quantile<- quantile(log.FloodInsurance, c(0.05, 0.1, 0.25, 0.5, 0.75, 0.9,0.95))
Percentile.df<-data.frame(base.quantile ,log.quantile)
Percentile.df
write.xlsx(Percentile.df,file='/Users/cheng/Documents/report_data.xlsx',append=TRUE,sheetName="Percentiles")
##Claim Amounts Plots
par(mfrow=c(1,2))
plot(Base.FloodInsurance,main="Scatter Plot of Original Claim Amounts")
plot(log.FloodInsurance,main="Scatter Plot of Log-Transformed Claim Amounts")
write.xlsx()

##Claim histogram 

hist (Base.FloodInsurance,
      main="Histogram for Amount", xlab="Amount",las=1)
hist (log.FloodInsurance, prob=T, breaks=seq(from=2, to =15, by=0.5),xlim=c(2,15), main="Histogram for Logged
Amount", xlab="Amount",las=1)

#MRL
plot(sorted.log.FloodInsurance,amount.mrl,type="l",xlab="Claims",ylab="Mean Excess",main="Logged Empirical
MRL")
amount.base.mrl<-emrl(sorted.Base.FloodInsurance)
plot(sorted.Base.FloodInsurance,amount.base.mrl,type="l",xlab="Claims",ylab="Mean Excess",main="Base Empirical
MRL")


##IID tests
par(mfrow=c(1,1))
plot(iid_test)

#Ljung-box test
test1
test2
test3

#fit Parameters
normal_fit_parameters
gamma_fit_parameters
WB_fit_parameters
logistic_fit_parameters

gamma_fit_mme
normal_fit_mme
weibull_fit_mme
logistic_fit_mme

#Covariacne matrix
gamma_cov
normal_cov
weibull_cov
logistic_cov

#empirical vs estimated fits

plot(normal_fit)
plot(gamma_fit)
plot(weibull_fit)
plot(logistic_fit)

hist(sorted.log.FloodInsurance,prob=TRUE, main="Empirical vs Theoretical Densities",xlab="Log Transformed Claim Data")
curve(dgamma(x,gamma_fit_parameters[1],gamma_fit_parameters[2]),col='blue',lwd=2,add=T)
curve(dweibull(x,WB_fit_parameters[1],WB_fit_parameters[2]),col='cadetblue',lwd=2,add=T)
curve(dnorm(x,mean=normal_fit_parameters[1],sd=normal_fit_parameters[2]),col='deepskyblue',lwd=2,add=T)
curve(dgamma(x,logistic_fit_parameters[1],logistic_fit_parameters[2]),col='lightblue',lwd=2,add=T)
legend("bottomright", c("Empirical","Gamma fit", "Weibull fit","Normal fit","Logistic fit"), fill=c("black","blue","cadetblue",
                                                                                     "deepskyblue","lightblue"))
log.data.parameters
