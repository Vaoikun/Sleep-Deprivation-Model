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
\text{prod}\,
\frac{1 - A_s}{k}
\begin{cases}
0.89 & \text{if asleep} \\
1 & \text{if awake}
\end{cases}
$$

The last piece of the equation is to determine how soluble waste converts into damaging fibular waste. This is composed of three terms. The first, $c A_{s}^{2}$, represents a process called primary nucleation. This is the rate at which soluble Aβ monomers form fibrils with each other. Herein is a simplification of our model, as we categorize monomers as soluble and all polymers as non-soluble, when in fact Aβ aggregates exist on a spectrum of size and solubility. We justify this simplification with the slow rate of primary nucleation and the rapid toxification and secondary nucleation of Aβ polymers. The second mass action term, $h A_{s}A_{f}$ represents this latter process of secondary nucleation whereby Aβ monomers bind to existing polymers. This is a much lower energy process and so it happens much more rapidly, though it is dependent on the concentration of fibrillar A$\beta$. The final term is a disaggregation term representing a concentration-dependent rate of disintegration of fibrillar Aβ polymers.

$$
\text{agg}(x,t) = cA_s^{2} + hA_s A_f - gA_f
$$

$$
\frac{dA_s}{dt} = P(x,t) - G(x,t) - \text{agg}(x,t)
$$

$$
\frac{dA_f}{dt} = \text{agg}(x,t)
$$
