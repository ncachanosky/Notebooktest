---
# Title, summary, and page position.
linktitle: "The IS-LM Model"
weight: 2

# Page metadata.
title: The IS-LM Model
date: "2018-09-09T00:00:00Z"
type: book  # Do not modify.
---

---

{{< icon name="python" pack="fab" >}} {{% staticref "Python/The IS-LM model.py" %}}Download Python file.{{% /staticref %}}

---

## Introduction

The IS-LM (or Hicks-Hansen) model shows the combinations of interest rate (vertical axis) and income (horizontal axis) for which the goods market (IS) and the loan market (LM) are in equilibrium. There is one particular combination of interest rate and income that is consistent with equilibrium in both markets simultaneously. 

In the graphic version of the model, all the points in the **IS schedule** represent equilibrium in the goods market. Similarly, all the points in the **LM schedule** represents equilibrium in the loanable funds market.

This model treats the price level as exogenous (given and fixed). In this sense, the IS-LM model is applicable in the context of substantia idle resources, when changes in income have no effect on the price level.

---

## The IS schedule (Investment-Saving)

The IS schedule is derived from the equilibrium condition where output $(Y)$ equals spending:

$$
\begin{align}
output &= spending \\\\[10pt]
Y &= C + I + G + (X - Z)
\end{align}
$$

where $C$ is household consumption, $I$ is private investment, $G$ is the level of government spending, $X$ is exports, and $Z$ is imports. We can treat variables $G$ and $X$ as exogenous variables. The former is defined by policy makers, the latter is given by economic conditions in the rest of the world. A more detailed exposition, such as the Mundell-Fleming model, would also take into consideration the exchange rate, and exports will also be dependent (to some extent) of domestic economic policy.

Assume that $C$ follows a keynesian consumption function:

$$
\begin{equation}
 C = a + b \cdot (Y - T)
\end{equation}
$$

where $a \geq 0$ is the level of autonomous consumption (independent of the level of income), $b \in (0, 1)$ is the marginal propensity to consume, and $T$ is the dollar-amount of taxes.

Assume now a simple linear relationship between investment and the interest rate $i$:

$$
\begin{equation}
    I = I_0 - di
\end{equation}
$$

where $I_0$ represents the level of investment when $i = 0$ (the intercept) and $d$ is the rate at which $I$ falls when $i$ increases (the slope).

We can also assume that imports follow a similar functional form than household's domestic consumption:

$$
\begin{equation}
    Z = \alpha + \beta \cdot (Y - T)
\end{equation}
$$

where $\alpha$ is the autonomous level of imports and $\beta$ is the marginal propensity to import.

To derive the IS schedule we need to use the consumption, investment, and import functions and solve for $i$ from the equilibrium condition:

$$
\begin{align}
    Y &= C + I + G + (X - Z) \\\\[20pt]
    Y &= \underbrace{[a + b(Y-T)]}_{C} + \underbrace{[I_0 - di]}\_{I} + G + [X - \underbrace{(\alpha + \beta (Y-T))}\_{Z}] \\\\[10pt]
    i\_{IS} &= \underbrace{\frac{(a - \alpha) - (b - \beta)T + I_0 + G + X}{d}}\_{\text{intercept}} - \underbrace{\frac{1 - b + \beta}{d}}\_{\text{slope}} Y
\end{align}
$$

Note that the larger $\alpha$ and $\beta$, the lower the intercept. This means that at the same level of $i$, income will be lower. Note also that the intercept and the slope of the IS schedule is sensitive to the value of $d$. The more (less) sensitive investment is to $d$, the more (less) horizontal the IS schedule looks. Finally, note that the IS schedule is a straight line.

The following code plots the IS schedule using the above information. The first part of the code imports the required `Python` packages. The second part of the code defines the parameters and arrays. The third part of the code defines and populates the IS schedule. The fourth part of the code plots the IS schedule.

```python
#%% *** CELL 1 ***
"============================================================================"
"1|IMPORT PACKAGES"
import numpy             as np   # Package for scientific computing
import matplotlib.pyplot as plt  # Matplotlib is a 2D plotting library


"============================================================================"
"2|DEFINE PARAMETERS AND ARRAYS"
# Code parameters
dpi = 300
# IS parameters
Y_size = 100
a      =  20    # Autonomous consumption
b      =   0.2  # Marginal propensity to consume
alpha  =   5    # Autonomous imports
beta   =   0.2  # Marginal propensity to import
T      =   1    # Taxes
I      =  10    # Investment intercept (when i = 0)
G      =   8    # Government spending
X      =   2    # Exports (given)
d      =   5    # Investment slope wrt to i
# Arrays
Y = np.linspace(0, Y_size, Y_size*2)  # Array values of Y


"============================================================================"
"3|DEFINE AND POPULATE THE IS-SCHEDULE"
def i_IS(a, alpha, b, beta, T, I, G, X, d, Y):
    i_IS = ((a-alpha)-(b-beta)*T + I + G + X - (1-b+beta)*Y)/d
    return i_IS

def Y_IS(a, alpha, b, beta, T, I, G, X, d, i):
    Y_IS = ((a-alpha)-(b-beta)*T + I + G + X - d*i)/(1-b+beta)
    return Y_IS

i = i_IS(a, alpha, b, beta, T, I, G, X, d, Y)


"============================================================================"
"4|PLOT THE IS-SCHEDULE"
### AXIS RANGE
y_max = np.max(i)
x_max = Y_IS(a, alpha, b, beta, T, I, G, X, d, 0)
axis_range = [0, x_max, 0, y_max]                        

### BUILD PLOT
fig, ax = plt.subplots(figsize=(10, 8), dpi=dpi)
ax.set(title="IS SCHEDULE", xlabel=r'Y', ylabel=r'r')
ax.plot(Y, i, "k-")
### AXIS
ax.yaxis.set_major_locator(plt.NullLocator())   # Hide ticks
ax.xaxis.set_major_locator(plt.NullLocator())   # Hide ticks
### SETTINGS
plt.axis(axis_range)
plt.show()
```

![Fig_01](Fig_01.png)

---

## The LM schedule (Liquidity preference-Money supply)

To build the LM schedule we need money supply $(M^S)$ and money demand $(M^D)$. Money supply is an exogenous variable (e.g. a policy decision of the central bank). We can build a money demand expression following Keynes three sources of money demand:
1. precautionary reasons (e.g. save for  a rainy-day), (2)
2. to perform transaction
3. speculation reasons. 
   
Expressing $M^S$ and $M^D$ in real terms:

$$
\begin{align}
    \frac{M^S}{P} &= \frac{M^S_0}{P} \\\\[10pt]
    \frac{M^D}{P} &= c_1 + c_2 Y - c_3 i
\end{align}
$$

Where each term represent precautionary, transaction, and speculation reasons to demand money respectively. Note that the amount of money needed for transactions depends on the level of income. The interest rate has a negative relationship with money demand due to Keynes' expectations theory (not covered here).

Since in equilibrium $M^S = M^D$, the LM schedule can be expressed in the following way:

$$
\begin{align}
    \frac{M^S_0}{P} &= \frac{M^D}{P}  \\\\[10pt]
    \frac{M^S_0}{P} &= c_1 + c_2 Y - c_3 i  \\\\[10pt]
    i_{LM} &= \underbrace{\frac{1}{c_3} \left(c_1 - \frac{M^S_0}{P} \right)}_\text{intercept} + \underbrace{\frac{c_2}{c_3}}_\text{slope} Y
\end{align}
$$

Note that if the interest rate has no effect on money demand $(c_3 \to 0)$, then the LM schedule becomes inelastic (vertical).

We can plot the LM schedule in a similar way we did for the IS schedule.

```python
#%% *** CELL 2 ***
"============================================================================"
"5|DEFINE PARAMETERS"
# LM Parameters
c1 = 1000       # Precautionary money demand
c2 = 10         # Transaction money demand
c3 = 10         # Speculation money demand
Ms = 20000      # Nominal money supply
P  = 20         # Price level


"============================================================================"
"6|DEFINE AND POPULATE THE LM-SCHEDULE"
def i_LM(c1, c2, c3, Ms, P, Y):
    i_LM = (c1 - Ms/P)/c3 + c2/c3*Y
    return i_LM

i = i_LM(c1, c2, c3, Ms, P, Y)


"============================================================================"
"7|PLOT THE LM-SCHEDULE"
### AXIS RANGE
y_max = np.max(i)
axis_range = [0, Y_size, 0, y_max]

### BUILD PLOT
fig, ax = plt.subplots(figsize=(10, 8), dpi=dpi)
ax.set(title="LM SCHEDULE", xlabel=r'Y', ylabel=r'r')
ax.plot(Y, i, "k-")
ax.yaxis.set_major_locator(plt.NullLocator())   # Hide ticks
ax.xaxis.set_major_locator(plt.NullLocator())   # Hide ticks
### SETTINGS
plt.axis(axis_range)
plt.show()
```

![Fig_02](Fig_02.png)

---

## Equilibrium

Note that the LM and IS do not share any variable. This means that any shock shifts only one of the schedules. This construction conveniently separates monetary policy as shifts of the LM schedule from fiscal policy as shifts of the IS schedule.

There is a pair $(Y^*, i^*)$ that produces the "general" equilibrium (contingent to parameters defining positive values for $Y^*$ and $i^*$). If IS = LM, then:

$$
\begin{align}
    i_{LM} =& i_{IS} \\\\[10pt]
    \frac{1}{c_3} \left(c_1 - \frac{M^S_0}{P} \right) - \frac{c_2}{c_3} Y =& \frac{(a-\alpha)-(b-\beta)T + I_0 + G + X}{d} - \frac{1-b+\beta}{d} Y \\\\[10pt]
    Y^* =& \left[ \frac{(a-\alpha)-(b-\beta)T + I_0 +G+ X}{d} + \frac{1}{c_3} \left(\frac{M^S_0}{P} - c_1 \right) \right] \left[\frac{1 - b + \beta}{d} - \frac{c_2}{c_3} \right]^{-1}
\end{align}
$$

And the interest rate of equilibrium would be:

$$
\begin{align}
    i^* =& \frac{1}{c_3} \left(c_1 - \frac{M^S_0}{P} \right) + \frac{c_2}{c_3} Y^*  \\\\
    i^* =& \frac{1}{c_3} \left(c_1 - \frac{M^S_0}{P} \right) + \frac{c_2}{c_3} \left(\frac{1-b+\beta}{d} - \frac{c_2}{c_3} \right)^{-1} \cdot \\\\[10pt]
         & \left[\frac{(a-\alpha)-(b-\beta)T + I_0 + G + X}{d} + \frac{1}{c_3} \left(c_1-\frac{M^S_0}{P} \right) \right]
\end{align}
$$

The next code plots the IS-LM model and locates four arrows showing the dynamics of the model when the economy is out of equilibrium. The <span style="color:blue">IS</span> and <span style="color:red">LM</span> schedules and their dynamics are denoted in <span style="color:blue">blue</span> and <span style="color:red">red</span> respectively.

```python
#%% *** CELL 3 ***
"============================================================================"
"8|DEFINE NEW PARAMETERS"
# IS new parameters
a = 100              # Autonomous consumption
b = 0.2              # Marginal propensity to consume
alpha = 5            # Autonomous imports
beta  = 0.15         # Marginal propensity to import
T = 1                # Taxes
I = 10               # Investment intercept (when i = 0)
G = 20               # Government spending
X = 5                # Exports (given)
d = 2                # Investment slope wrt to i
# LM new parameters
c1 = 2500            # Precautionary money demand
c2 = 0.75            # Transaction money demand
c3 = 5               # Speculation money demand
Ms = 23500           # Nominal money supply
P  = 10              # Price level


"============================================================================"
"9|CALCULATE EQUILIBRUM VALUES"

iIS = i_IS(a, alpha, b, beta, T, I, G, X, d, Y)
iLM = i_LM(c1, c2, c3, Ms, P, Y)

Y_star1 = ((a-alpha) - (b-beta)*T + I + G + X)/d
Y_star2 = (1/c3) * (c1 - Ms/P)
Y_star3 = (1 - b + beta)/d + (c2/c3)
Y_star  = (Y_star1 - Y_star2)/Y_star3

i_star1 = (1/c3)*(c1 - Ms/P)
i_star2 = (c2/c3)*Y_star
i_star  = i_star1 + i_star2


"============================================================================"
"10|PLOT THE IS-LM model"
### AXIS RANGE
y_max = np.max(iIS)
axis_range = [0, Y_size, 0, y_max]

### BUILD PLOT
fig, ax = plt.subplots(figsize=(10, 8), dpi=dpi)
ax.set(title="IS-LM MODEL", xlabel=r'Y', ylabel=r'r')
ax.plot(Y, iIS, "b-")
ax.plot(Y, iLM, "r-")
### AXIS
ax.yaxis.set_major_locator(plt.NullLocator())  # Hide ticks
ax.xaxis.set_major_locator(plt.NullLocator())  # Hide ticks
### ARROWS AND EQUILIBRIUM POINT
ax.arrow(25, i_star, 10,  0, head_length=2, head_width=1, color='b', alpha=0.7)
ax.arrow(25, i_star,  0, -2, head_length=2, head_width=1, color='r', alpha=0.7)
ax.arrow(Y_star, 20, 10,  0, head_length=2, head_width=1, color='b', alpha=0.7)
ax.arrow(Y_star, 20,  0, 10, head_length=2, head_width=1, color='r', alpha=0.7)
ax.arrow(85, i_star,-10,  0, head_length=2, head_width=1, color='b', alpha=0.7)
ax.arrow(85, i_star,  0,  2, head_length=2, head_width=1, color='r', alpha=0.7)
ax.arrow(Y_star, 55,-10,  0, head_length=2, head_width=1, color='b', alpha=0.7)
ax.arrow(Y_star, 55,  0,-10, head_length=2, head_width=1, color='r', alpha=0.7)
plt.plot(Y_star, i_star, 'ko')                      # Equilibrium point
plt.axvline(x=Y_star, ls=':', color='k', alpha=0.5) # Equilibrium line
plt.axhline(y=i_star, ls=':', color='k', alpha=0.5) # Equilibrium line
### LABELS
plt.text(95, 16, "IS", color='b')
plt.text(95, 47, "LM", color='r')
### SETTINGS
plt.axis(axis_range)
plt.show()
```

![Fig_03](Fig_03.png)

---

## Dynamics

The arrows in the above figure shows that the model is stable. The economy always converge back to equilibrium. The code below shows the dynamics towards equilibrium starting from four disequilibrium points: A in <span style="color:blue">blue</span>, B in <span style="color:red">red</span>, C in <span style="color:green">green</span>, and D in <span style="color:olive">olive</span>. For the illustration purposes of this example, ten iterations starting from each point is enough to show the dynamics towards equilibrium.

Horizontal changes $(Y)$ is calculated with the IS function. Vertical changes $(i)$ is calculated with the LM function (see the arrows in the above figure). Starting from a disequilibrium point, the location of the next point is calculated with these two functions. Adding time period $(t = 0, ..., 10)$, where $Y_{0}$ and $i_{0}$ are arbitrary initial values, then:

$$
\begin{align}
    Y_{t} &= \frac{(a-\alpha) - (b-\beta)T + I_0 + G + X}{1-b+\beta} - \frac{d}{1-b+\beta} i_{t-1} \\\\[10pt]
    i_{t} &= \frac{1}{c_3} \left(c_1 - \frac{M^S_0}{P} \right) + \frac{c_2}{c_3} Y_{t-1}
\end{align}
$$

```python
#%% *** CELL 4 ***
"============================================================================"
"11|CALCULATE THE DYNAMICS FOR DISEQUILIBRIUM POINTS"
iterations= 10

" |STARTING POINT A"
YA, iA = 10, 50   # Initial values

A_i_LM = np.zeros(iterations)
A_Y_IS = np.zeros(iterations)

A_Y_IS[0] = YA
A_i_LM[0] = iA

for j in range(1, iterations):
    A_Y_IS[j] = Y_IS(a, alpha, b, beta, T, I, G, X, d, A_i_LM[j-1])
    A_i_LM[j] = i_LM(c1, c2, c3, Ms, P, A_Y_IS[j-1])
    
" |STARTING POINT B"
YB, iB = 80, 55   # Initial values

B_i_LM = np.zeros(iterations)
B_Y_IS = np.zeros(iterations)

B_Y_IS[0] = YB
B_i_LM[0] = iB

for j in range(1, iterations):
    B_Y_IS[j] = Y_IS(a, alpha, b, beta, T, I, G, X, d, B_i_LM[j-1])
    B_i_LM[j] = i_LM(c1, c2, c3, Ms, P, B_Y_IS[j-1])   
    
" |STARTING POINT C"
YC, iC = 70, 20   # Initial values

C_i_LM = np.zeros(iterations)
C_Y_IS = np.zeros(iterations)

C_Y_IS[0] = YC
C_i_LM[0] = iC

for j in range(1, iterations):
    C_Y_IS[j] = Y_IS(a, alpha, b, beta, T, I, G, X, d, C_i_LM[j-1])
    C_i_LM[j] = i_LM(c1, c2, c3, Ms, P, C_Y_IS[j-1])    

" |STARTING POINT D"
YD, iD = 30, 25   # Initial values

D_i_LM = np.zeros(iterations)
D_Y_IS = np.zeros(iterations)

D_Y_IS[0] = YD
D_i_LM[0] = iD

for j in range(1, iterations):
    D_Y_IS[j] = Y_IS(a, alpha, b, beta, T, I, G, X, d, D_i_LM[j-1])
    D_i_LM[j] = i_LM(c1, c2, c3, Ms, P, D_Y_IS[j-1])


"============================================================================"
"12|PLOT THE IS-LM model"
### AXIS RANGE
y_max = np.max(iIS)
axis_range = [0, Y_size, 0, y_max]

### BUILD FIGURE
fig, ax = plt.subplots(figsize=(10, 8), dpi=dpi)
ax.set(title="IS-LM MODEL", xlabel=r'Y', ylabel=r'r')
ax.plot(Y, iIS, "k-")
ax.plot(Y, iLM, "k-")
### EQUILIBRIUM POINT AND LINES
ax.plot(Y_star, i_star, "ko")
ax.axvline(x=Y_star, ls=':', color='k')
ax.axhline(y=i_star, ls=':', color='k')
### DYNAMICS
ax.plot(YA, iA, "bo")                     # Starting point A
ax.plot(A_Y_IS, A_i_LM, "b--", alpha=0.7)
ax.plot(YB, iB, "ro")                     # Starting point B
ax.plot(B_Y_IS, B_i_LM, "r--", alpha=0.7)
ax.plot(YC, iC, "go")                     # Starting point C
ax.plot(C_Y_IS, C_i_LM, "g--", alpha=0.7)
ax.plot(YD, iD, "co")                     # Starting point C
ax.plot(D_Y_IS, D_i_LM, "c--", alpha=0.7)
### LABELS
ax.text(95, 14, "IS")
ax.text(95, 47, "LM")
ax.text(YA-3, iA+1, "A", color="b")
ax.text(YB+3, iB  , "B", color="r")
ax.text(YC-3, iC  , "C", color="g")
ax.text(YD  , iD-3, "D", color="c")
### AXIS
ax.yaxis.set_major_locator(plt.NullLocator())   # Hide ticks
ax.xaxis.set_major_locator(plt.NullLocator())   # Hide ticks
### SETTINGS
plt.axis(axis_range)
plt.show()
```

![Fig_04](Fig_04.png)