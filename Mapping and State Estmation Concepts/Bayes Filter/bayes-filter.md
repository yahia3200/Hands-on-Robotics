# Bayes Filter

## Recursive State Estimation

Define a robot **global** position  $ x_{t} $ knowing it is postion in previous time step $ x_{t - 1}$ and our control comands

Idealy our postion $ x_{t} $ can be caculated from our motion equtions but due to uncertainty we have to use readings from our sensors to correct our beliefs. In this case $ x_{t} $ will be a random variable with some distribution and our goal is to update this distribution at each team step

## What is Bayes Filter

It is a framework for recursive state estimation

## Recursive Bayes Filter Derivation

We want to fint an exception for our belief about our postion $bel(x_t)$ with respect to $bel(x_{t - 1})$

Fourmaly our belief can be writen as $bel(x_t) = p(x_t | z_{1:t}, u_{1:t})$ where $z_{1:t}$ is our sensors measurements and $u_{1:t}$ is our control commands

Using Bayes' rule we can rewrite $bel(x_t)$ as
$$ bel(x_t) = \eta \ p(z_t|x_t, z_{1:t-1}, u_{1:t})\ p(x_t|z_{1:t-1}, u_{1:t})$$
Now use can use the Markov assumption which states that given i know the state of the system what happend in the past don't give me inforamtions about what will happen in the future. This translates to our case to that given we know our postion $x_t$ to estimiate the likelihood of an observation we don't need $z_{1:t-1}$ and $u_{1:t-1}$

Then our $bel(x)$ can be rewritin as
$$bel(x) = \eta \ p(z_t|x_t, u_t) \ p(x_t| z_{1:t-1}, u_{1:t})$$

Now using the total probability theorem we all add a new variable $x_{t-1}$ to the secoand part of the equation and itegrate over it

$$bel(x) = \eta \ p(z_t|x_t, u_t) \ \int p(x_t| x_{t-1},z_{1:t-1} u_{1:t}) \ p(x_{t -1}| z_{1:t-1} u_{1:t})dx_{t -1}$$

We can use the Markov assumption again to simplify the first part of the integral

$$bel(x) = \eta \ p(z_t|x_t, u_t) \ \int p(x_t| x_{t-1},u_{t}) \ p(x_{t -1}| z_{1:t-1} u_{1:t})dx_{t -1}$$

We will make another assumption that the feature control command will not add any inforamtion about our postion now. we can now rewrite the equation
$$bel(x) = \eta \ p(z_t|x_t, u_t) \ \int p(x_t| x_{t-1},u_{t}) \ p(x_{t -1}| z_{1:t-1} u_{1:t - 1})dx_{t -1}$$

Now we can exprese our $bel(x_t)$ as a function of $bel(x_{t -1})$
$$bel(x) = \eta \ p(z_t|x_t, u_t) \ \int p(x_t| x_{t-1},u_{t}) \ bel(x_{t - 1}) dx_{t -1}$$

## Insights about Bayes Filter

The Bayes filter can be written as a two step process

- Prediction Step
$$\overline{bel}(x_t) = \int p(x_t| x_{t-1},u_{t}) \ bel(x_{t - 1}) dx_{t -1}$$
This step is called prediction because it takes our previous beleif and advances it to the feature using our control commands. To transfer our control commands to a postion estimate we need a **Motion Model** which tell us our postion given the last postion and the control commands which is $p(x_t| x_{t-1},u_{t})$.
$ \space $
- Correction Step
$$bel(x_t) = \eta \ p(z_t|x_t, u_t) \ \overline{bel}(x_t)$$
In this step we use our data that we get from the sensors to correct our beliefs about our postion. For this we need to measure how likely to get this data given our postion and control commands. Yhis defined by our **Observation Model** (also called measurement or sensors model) which is $ p(z_t|x_t, u_t) $
  
## Using The Bayes Filter

Depending on the assumption we will make about our motion and observation models and how we want to represant the probability distributions in parametric or non parametric form we will get different realization for the bayes filter such as:

- Kalman Filter and EKF
Which assumes that all probability distributions are gaussian and linear (EKF can deal with non linearity using taylor expansion)
$ \space $

- Particle Filter
Uses non-prametric distributions and can be used with any models but requires sampling

## Reference

- Bayes Filter: Cyrill Stachniss Lecture
- Probabilistic Robotics Book: Chapter 2
