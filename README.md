# Sleep-Deprivation-Model
# Αuthers: Dallin Wise, Luke Robinson, Tyler Pace, Vance Williams
# Brigham Young University

# Abstract:
In this project, we develop and analyse ODE models of the glymphatic system, focusing on soluble and fibrillar amyloid-β, to investigate efficient strategies for obtaining sufficient sleep. Using our results, we examine whether it is possible to recover lost sleep and explore alternative sleep strategies for achieving adequate rest.

# Motivation/Background:
The exact functions and physiology of sleep remain an ongoing topic of investigation and discussion in medicine. However, recent findings have indicated that its purpose is, in part, to enable regular clearance of the metabolic waste products, or metabolites, which naturally accumulate in the interstitial space between brain cells. [[Xie](https://pubmed.ncbi.nlm.nih.gov/24136970/)] One of the mechanisms that makes this clearance possible is known as the glymphatic system. This system is primarily active during sleep, specifically N3 or slow-wave sleep, when the interstitial space between brain cells expands. [[Reddy](https://pubmed.ncbi.nlm.nih.gov/33212927/)] This expansion and the ensuing glymphatic activity flood the brain with cerebrospinal fluid, which, by various pathways, carries metabolites into the blood and lymphatic system for processing. [[Tarasoff, Plog](https://pubmed.ncbi.nlm.nih.gov/29195051/)]

One metabolite of special interest is known as amyloid-β (Aβ), which is significant for its well-documented correlation with neurological disorders, including Alzheimer's disease (AD). In its soluble form, Aβ can be cleared from the interstitial fluid by the glymphatic system. However, by an aggregation process, Aβ monomers bind into toxic plaques characteristic of AD. This aggregation process is the subject of ongoing study, but is typically gradual and subject to a variety of conditions, including long-term concentration of soluble Aβ in the brain. 

An important question to consider is how lifestyle choices and other conditions impact the glymphatic system's efficacy in Aβ clearance. Although this is important to understanding AD pathology, empirical research on the matter is limited by the invasive nature of direct Aβ measurements from the cerebrospinal fluid and the lack of reliable alternative means.\cite{Huang} Herein lies the aim of our model: to quantify the effects of lifestyle choices on glymphatic activity and resulting Aβ concentration in the brain. In particular, we explore the effects of differing sleep patterns on both soluble Aβ levels in the short term and fibrillar Aβ buildup in the long term.

As mentioned above, we used a compartment model for soluble and fibrillar A$\beta$ levels in the brain. Our research indicated the interstitial space in the brain during REM was nearly identical to wakefulness. \cite{Reddy} Since glymphatic activity primarily depends on space for CSF to flow in and clear metabolites, we simplified to modeling deep sleep. Deep sleep occurs most during the earlier hours of sleep, primarily during the first 4-5 hours. As the night goes on, sleep becomes more dominated by REM. \cite{Eric} Using a decaying sine wave to model glymphatic activity during sleeping hours was natural. In it's most  basic form, we have 

$$\sin(\theta * t) * e^{-\lambda * t},$$
where $\theta$ is the frequency and $\lambda$ is the exponential decay constant. To make a realistic implementation, we need to shift the sine wave to be always positive and start at zero. Thus, the final iteration of glymphatic activity is

$$\frac{1}{2}(\sin(\theta t  - 1.57) + 1) * e^{-\lambda * t}$$

We can then construct a piecewise function for glymphatic activity to model sleeping and waking hours 

$$G(x,t) = \left\{\begin{array}{cc}
\frac{1}{2}(\sin(\theta t  - 1.57) + 1) * e^{-\lambda * t} * A_{s} & \text{if asleep}\\
.01 * A_s & \text{if awake}
\end{array}\right.$$

We used a similar method to construct a piecewise function to model the production of waste in brain cells based on the metabolic activity of the brain. In addition to a natural 11\% decrease during sleep, metabolic activity can also be slowed by an over-abundance of waste in the brain. This inverse relationship creates a carrying capacity on waste particles. Thus, production becomes

$$P(x,t) = \operatorname{prod} \frac{1 - A_{s}}{k} * \left\{ \begin{array}{cc}
.89 & \text{if asleep}\\
    1 & \text{if awake}
\end{array}\right.$$
