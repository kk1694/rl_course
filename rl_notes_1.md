# Reinforcement Learning Notes 1

(Edited version at https://krisztiankovacs.com/reinforcement_learning/2018/09/19/rl-notes-1.html)

These notes are based on [Siraj Raval's reinforcement learning course](https://www.youtube.com/watch?v=fRmZck1Dakc). This week was about basic concepts of markov decision processes (MDPs).

First, what is reinforcement learning (RL)? It is an area of machine learning concerned with how software agents ought to take actions in an environment so as to maximize some notion of cumulative reward (from [Wikipedia](https://en.wikipedia.org/wiki/Reinforcement_learning)). For example, an AI might learn to play chess by playing many games, and updating its strategy based on its experiences.

## Basic Concepts

A **state** is a possible condition the agent can be in the given environment.

**Markov decision processes (MDPs)** refer to settings where the current state *s* contains all the available information about the future from *s*. In other words, the future from *s* is conditionally independent of the past given that we know s. 

For example, think of chess. How we arrived at a particular board position doesn't matter; the current board position contains all we need to know to continue the game.

**Actions** are the decisions the agent can take. We can denote the set of actions in the state *s* as *a(s)*.

**Rewards**, what the agent is trying to obtain, are real valued functions. They can be functions from a given state, an action, or action-state combinations. 

## Toy Example: Gridworld

![](/assets/img/rl_mario){:width="500"}


The states are the squares that Mario can be on. The actions are moving up, down, left, right. There are two states with rewards: the top right with +1, and below that with -1. To make the game end, we can regard these two as terminal states: the game ends if you land on these steps.

The example has the Markov property: how you end up on a square doesn't influence the future of the game.

## Rewards and Policies

As the definition above mentioned, the agent will be trying to maximize 'some notion of cumulative reward'. In some settings, it makes sense to maximize the cumulative reward, but generally we also add discounting, trying to maximize the discounted cumulative future reward:

$$R_t = \sum_t^{\infty}\gamma^i r_i$$,

where $$\gamma$$ is the discount factor (say, 0.9), and $$r_i$$ is the reward at a given time step.

A **policy** is a way of acting. Denoted as $$\pi(s)$$, it maps a state to an action. Think of it as a strategy. (This definition applies to deterministic policies, but can easily be extended to stochastic policies as well.)

The optimal policy, $$\pi^*$$, is simply the one that maximizes *R*.

## Value Functions

So how do we find the optimal policy? Value functions help us do that.

There are two types of value functions: *state value functions* and *action value functions*. Informally, these give the 'value' of being in a given state, or of doing a given action.

Mathematically, the state value function is

$$V^{\pi}(s) = E[R_t | s_t = s]$$

the expected (discounted cumulative future) reward from being in *s* under policy $$\pi$$. 

Similarly, the action value function is

$$ Q^{\pi}(a, s) = E[R_t | s_t = s, a_t = a]$$

In other words, the expected reward from being in *s*, doing *a*, under policy $$\pi$$. Note that both functions are dependent on the policy.

## A Simple Bellman Equation

So how to we compute these value functions? That's what the Bellman equations are for. They allow us to express the value of a state (or action) from the value of other states (or actions), allowing us to iteratively solve for the optimal policy. 

In a deterministic environment, the Bellman equation for the state value function is

$$ V^*(s) = max_a(r(s, a, s') + \gamma V^*(s')) $$

where $$V^*$$ is the value function under optimal policy, $$r(s, a, s')$$ is the reward for taking action *a* in state *s* and ending up in *s'*, and $$V^*(s')$$ is the value of state *s'*.

In other words, we take the action that maximizes our reward plus the discounted value of the state we end up in.

## Gridworld Again

Let's compute the (optimal) state value function for our toy example. Working iteratively backward (using $$\gamma = 0.9$$, we can fill in our grid:

![](/assets/img/rl_mario_sol){:width="500"}

Obviously, this kind of brute-force approach won't work in more complicated situations where we can't go over all state-action combinations. It's still a useful way to organize our thinking.

## Resources

Besides the lecture material, I relied [the following](https://joshgreaves.com/reinforcement-learning/understanding-rl-the-bellman-equations/) guide to the Bellman equation and the [wikipedia](https://en.wikipedia.org/wiki/Reinforcement_learning) entry for RL.
