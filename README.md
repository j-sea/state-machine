# State Machine
This repo is dedicated to a simple implmentation of a finite state machine (FSM) data structure.

## What is a State Machine?
According to [Wikipedia](https://en.wikipedia.org/wiki/Finite-state_machine):
> A state machine is a mathematical model of computation. It is an abstract machine that can be in exactly one of a finite number of **states** at any given time. The FSM can change from one state to another in response to some external inputs; the change from one state to another is called a **transition**. An FSM is defined by a list of its states, its initial state, and the conditions for each transition.

> Simple examples are **vending machines**, which dispense products when the proper combination of coins is deposited, **elevators**, whose sequence of stops is determined by the floors requested by riders, **traffic lights**, which change sequence when cars are waiting, and **combination locks**, which require the input of a sequence of numbers in the proper order.

### I like to think of state machines as employable _flowcharts_.
And that's how I had been using them to conduct my early homeworks in the Full-Stack Coding Camp.

## Goals of this Project
I have a few goals with this project:

1. Create a re-usable data structure employable in anyone's JavaScript projects.
2. Demonstrate how it works and a couple ways it can be used to separate code concerns.
3. Further my own, and hopefully anyone else's, understanding of JavaScript and data structures.

## How does it work?
Okay. Remember when I equated state machines to _flowcharts_? Let's think about how a flowchart works. For our purposes, let's explore a user flow of a website registration process.

Imagine the diamonds as _actions_ taken by the user and squares as _pages_ the user sees.

![Registration User Flow](registration-overview.png "Registration User Flow")

As you can follow along the user flow, notice how:

- The user is only ever on one _page_ at a time.
- The user can only _transition_ to another _page_ by taking an _action_.

