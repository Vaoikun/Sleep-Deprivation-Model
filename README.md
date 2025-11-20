# Sleep-Deprivation-Model

## Authors
Dallin Wise, Luke Robinson, Tyler Pace, Vance Williams  
Brigham Young University

## Abstract
In this project, we develop and analyse ODE models of the glymphatic system, focusing on soluble and fibrillar amyloid-β, to investigate efficient strategies for obtaining sufficient sleep. Using our results, we examine whether it is possible to recover lost sleep and explore alternative sleep strategies for achieving adequate rest.

## Motivation / Background
The exact functions and physiology of sleep remain an ongoing topic of investigation in medicine. Recent findings indicate that one purpose is the clearance of metabolic waste products accumulating in the interstitial space between brain cells [[Xie](https://pubmed.ncbi.nlm.nih.gov/24136970/)]. One mechanism responsible for this is the glymphatic system, which is most active during N3 (slow-wave) sleep when the interstitial space expands [[Reddy](https://pubmed.ncbi.nlm.nih.gov/33212927/)]. This expansion allows cerebrospinal fluid to circulate through the brain, transporting metabolites into the blood and lymphatic systems for processing [[Tarasoff & Plog](https://pubmed.ncbi.nlm.nih.gov/29195051/)].

A metabolite of particular interest is amyloid-β (Aβ), which is highly correlated with Alzheimer's disease. Soluble Aβ can be cleared glymphatically, but aggregation into fibrillar plaques occurs gradually and depends on long-term soluble Aβ concentration.

Understanding how lifestyle factors influence glymphatic clearance is important, but empirical research is limited by the invasiveness of direct Aβ measurement and the lack of reliable alternatives. Our model aims to quantify how sleep patterns affect glymphatic activity and Aβ levels, both short-term (soluble) and long-term (fibrillar).

## Modeling Glymphatic Activity
We approximate glymphatic activity during sleep with a decaying sine wave:

$$
\sin(\theta t)\, e^{-\lambda t}.
$$

To shift the wave so it is always positive and starts at zero, we use:

$$
\frac{1}{2}\big(\sin(\theta t - 1.57) + 1\big)\, e^{-\lambda t}.
$$

We then define glymphatic activity as a piecewise function:

$$
G(x,t) =
\begin{cases}
\frac{1}{2}\big(\sin(\theta t - 1.57) + 1\big)\, e^{-\lambda t}\, A_s & \text{if asleep} \\
0.01\, A_s & \text{if awake}
\end{cases}
$$

## Modeling Metabolic Waste Production
Metabolic activity naturally decreases by approximately 11% during sleep and can further decrease when waste accumulates (creating a carrying capacity effect). We model production as:

$$
P(x,t) =
\operatorname{prod}
\frac{1 - A_s}{k}
\begin{cases}
0.89 & \text{if asleep} \\
1 & \text{if awake}
\end{cases}
$$
