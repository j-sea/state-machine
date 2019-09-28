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

1. Create a re-usable state machine data structure employable in anyone's JavaScript projects.
2. Demonstrate how it works and a couple ways it can be used to separate code concerns.
3. Further my own, and hopefully someone else's, understanding of JavaScript and data structures.

## How does it work?
Okay. Remember when I equated state machines to _flowcharts_? If you're not familiar with what a flowchart is, it's basically a visualization of how a process works through the use of shapes and arrows. They can easily be used to visualize and plan out how a User Interface will work—which is pretty much what the front-end of a website is. That said, let's explore the user flow of a website registration process.

Imagine the **diamonds** as _actions_ taken by the user and **squares** as _pages_ the user sees. The **arrows** denote the direction in which the user is flowing through the chart. We'll start following the user's flow at the point at which they enter our website's URL into their browser.

![Registration User Flow](registration-overview.png "Registration User Flow")

As you follow along the user flow, notice how:

- The user is only ever on one _page_ at a time.
- The user can only _transition_ to another _page_ by taking an _action_.

We can take advantage of this user flow structure to create a single-page website as my team did with our [Send Care MEOW](https://jandystephens.github.io/Send-Care-MEOW/) web app.

First, let's group the _pages_ and _actions_ together into separate chunks, and we'll consider each chunk as a self-contained _state_.

![Registration State Machine](registration-chunked.png "Registration State Machine")

As you can see, we now have **three separate states**. It's important to note the states (as we've divided them) do _not_ need to know any information about the other states to operate. They're fully functioning on their own. The only information they need to know is where the _transitioning_ buttons lead (demonstrated by the muddy orange arrows).

Okay, you may be thinking it's nothing special so far, but stick with me. We're about to dive into the main feature of this state machine data structure.

## Why are states so great?
The main benefits of states are:

- They don't need to know any information about the other states to function.
- Only one state is active at a time.
- They take care of themselves and only themselves.

These points are important for separating code concerns which will save time and headaches in the future for you and your teammates. It also has the added benefit of reducing the amount of merge conflicts you have to resolve when using a version control like Git, especially if you separate the states into different files.

For more information about separation of code concerns, check out these extremely-well-designed and free video explanations from Coursera on [Loose Coupling & High Cohesion \[6m 36s\]](https://www.coursera.org/lecture/object-oriented-design/1-3-1-coupling-and-cohesion-q8wGt) and [Separation of Concerns \[9m 05s\]](https://www.coursera.org/lecture/object-oriented-design/1-3-2-separation-of-concerns-nBqPZ). (When they talk about "Classes", imagine them as like the Constructor Functions we learned about.) I highly recommend these videos. This is something I was required to learn when I took Computer Science in college, but it wasn't explained as well as these 16 minutes explained it.

Okay, done watching them? No? **Go watch them!** It really is enlightening!

Okay, now done watching them? ...Great!

So! Now you have an inkling of the benefits we get from states, but how do we actually extract those benefits?

That comes from how we set up the **States** and the **State Machine**.

## How it all works, finally!
Let's continue our train of thought with _states_ and then move into _state machines_.

### How states work...
In this project, each state is set up as an **object** with **three functions** that our _state machine_ will utilize to make everything Just Work™:

```JavaScript
var state = {
    initialize: function (sharedData) {
        // 'sharedData' is just an object that can have properties added to it which will persist between state switches

        // Conduct permanent initializations of things like:
        // - Data shared between states that need to exist and have default values
        // - Click event handlers on static buttons
        // - Submit event handlers on static forms
        // - Other event handlers that will exist for the lifetime of the user's visit
    },
    loadState: function (previousState, switchStateFunc, sharedData) {
        // 'previousState' can be used to conditionally load things based on which state we came from
        // 'switchStateFunc(stateIdentifier)' is a function that can be called to switch to the state associated with the identifier
        // 'sharedData' is just an object that can have properties added to it which will persist between state switches

        // Conduct things like:
        // - Resetting any local data to their default values before use
        // - Attaching event handlers that should only exist inside this state, like 'keyup', 'keydown', 'mousemove', etc.
        // - Fading in or unhiding the screen associated with this state or any other transitioning-in animations
        // - Starting any timeout or interval with 'setTimeout' and 'setInterval' calls
    },
    unloadState: function (nextState, sharedData) {
        // 'nextState' can be used to conditionally unload things based on which state we're going to next
        // 'sharedData' is just an object that can have properties added to it which will persist between state switches

        // Conduct things like:
        // - Clearing or resetting any form fields that shouldn't maintain their information the next time we visit this state
        // - Detaching event handlers that should only exist inside this state, like 'keyup', 'keydown', 'mousemove', etc.
        // - Fading out or hiding the screen associated with this state or any other transitioning-out animations
        // - Clearing any timeout or interval with 'clearTimeout' and 'clearInterval' calls 
    }
};
```

Note that you are not prevented from adding additional methods or variables on the state object and are encouraged to if it would help chunk things even further.

#### Now for a tangent on information hiding!

Also note that `sharedData` is a compromise for sharing data between states. My team encountered an issue when wanting to share data between screens such as form data. One way we solved it was by using `localStorage`. Another way we solved it was by using global variables that were visible by all code everywhere. This is a compromise between the two.

**Pros:**
- It has less complexity than setting and getting things from the `localStorage` every time you need to share information.
- It restricts access to the data shared between states to **just the states within the same _state machine_**.

**Cons:**
- Manipulating **local** state data such as local arrays or objects is possible if they're accidentally stored in `sharedData`.
- Manipulating **local** state data such as local strings, numbers, booleans, etc. is possible if a method is accidentally stored in `sharedData` with Closure Scope access to the local data.

To learn more about why I consider these as cons and global variables as unwanted, refer to this additional video explanation from Coursera about [Information Hiding \[6m 04s\]](https://www.coursera.org/lecture/object-oriented-design/1-3-3-information-hiding-OwrqD). Note that at the 3m 02s mark, they start talking about access modifiers which aren't available in JavaScript. The only way to truly hide information in JavaScript is through Closure Scopes.

Without closures, we would just set common guidelines in place for each developer to adhere to so that others don't touch variables they aren't supposed to (like in Python where all information is accessible by everyone else). However, developers that don't get the memo may still end up touching data they're not supposed to, introducing a difficult-to-resolve bug as a result. You can learn about this on Coursera with their video on [Conceptual Integrity \[5m 22s\]](https://www.coursera.org/lecture/object-oriented-design/1-3-4-conceptual-integrity-AUNUT). I highly recommend this video just to get an idea about how teams can coordinate with each other.

### How state machines work...
In this project, our state machine is set up to allow access to **two functions**: one for adding states to it, and another for starting the machine with a specific state.

```JavaScript
var stateMachine = {
    new: function (customSwitchStateFunc) {
        var privateFeatures = {
            stateObjects: {},
            sharedData: {},
            currentStateIdentifier: undefined,
            customSwitchState: customSwitchStateFunc,
            
            switchState: function (nextStateIdentifier) {
                if (!privateFeatures.stateObjects.hasOwnProperty(nextStateIdentifier)) throw new Error('Next state identifier does not exist in state machine');
                
                if (privateFeatures.currentStateIdentifier !== undefined) {
                    privateFeatures.stateObjects[privateFeatures.currentStateIdentifier].unloadState(nextStateIdentifier, privateFeatures.sharedData);
                }

                if (privateFeatures.customSwitchState === undefined) {
                    privateFeatures.stateObjects[nextState].loadState(privateFeatures.currentStateIdentifier, privateFeatures.switchState, privateFeatures.sharedData);
                }
                else {
                    privateFeatures.stateObjects[nextState].loadState(privateFeatures.currentStateIdentifier, privateFeatures.customSwitchState, privateFeatures.sharedData);
                }
                
                privateFeatures.currentStateIdentifier = nextStateIdentifier;
            }
        };

        var publicFeatures = {
            addState: function (stateIdentifier, stateObject) {
                if (privateFeatures.stateObjects.hasOwnProperty(stateIdentifier)) throw new Error('State identifier already exists in state machine');
                
                stateObjects[stateIdentifier] = stateObject;
                stateObject.initialize(privateFeatures.sharedData);
            },
            startWithState: function (startingStateIdentifier) {
                if (!privateFeatures.stateObjects.hasOwnProperty(startingStateIdentifier)) throw new Error('Starting state identifier does not exist in state machine');
                
                privateFeatures.switchState(startingStateIdentifier);
            },
        };

        return publicFeatures;
    }
};
```
