# Supercharged Microservice Testing with Betamax

*Yawn*. That pretty much sums up doing functional testing for microservices where you're pulling data feeds from an external system. No matter how fast you'd like things to go, you're at the mercy of network latency and the responsiveness of the source system to get you enough data to feed into your microservices pipeline to give you the confidence that it works properly.

If you find your own life dwindling away as you run YAFT (yet another functional test) against an HTTP external source, you're not alone. This problem is not going away as microservice pipelines provide functional composition in support of mashing up data from disparate sources. 

So what can be done to quite literal save your life in these situations? On the one hand there's a nice timeslot for coffee, a short chat and possibly a hike across the Sahara (including pre-training time). On the other hand, there's [*Betamax*](http://freeside.co/betamax/).

## A (Brief) Introduction to Betamax

Betamax is a really simple system for recording HTTP interactions and then playing them back, usually in testing scenarios.

Adopting what is effectively a proxy man-in-the-middle position,  Betamax records detected HTTP interactions to a *tape* as shown below:

TBD Diagram showing Betamax as a man-in-the-middle recording HTTP interations to a Betamax tape.

The Betamax tape is a human-readable [YAML](http://www.yaml.org) file that you can amend if you choose to do so. The main purpose of the tape is to then be automatically used in place of the full, and slow, interactions the next time the test is run.

TBD diagram showing the tape being used for replay.

## The Challenge: HTTPS

I came to Betamax as someone who was running a long-winded test against an external chat-style system. My micro service's single-purpose was to retrieve 1-hour batches of conversations in the chat system, and ferrying that on to the next micro service in the chain for further processing. 

> *NOTE* This is a real 'live' system, so I've had to remove the names to protect the *innocent*.




