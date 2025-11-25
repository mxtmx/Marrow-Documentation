# Retries

So, you’ve coded your autonomous routine to near perfection, and it works 90% of the time. That's great! But what about the other 10%? Are you comfortable with leaving that up to chance and risking underperforming in a critical match?

Imagine this scenario: in a deciding match, your auto sequence fails to grab a game element properly. Without any smart error-detection system, that single failure could cost your team the win.

This is where **a retry system** comes into play.



## Real-World Example

A perfect example for this comes from Hypernova 28596’s 2024–2025 INTO THE DEEP autonomous:

{% embed url="https://www.youtube.com/watch?v=xNpInsjkM50" %}

* **At 0:05** – Their first transfer fails. Instead of giving up, the robot automatically retries closing the claw a few more times, eventually securing the sample and completing the transfer. Without retries, the robot would have continued as if the transfer succeeded, scoring nothing.
* **At 0:18** – Their first attempt to intake a sample from the submersible doesn’t work. The robot re-detects, realigns, and tries again until it succeeds in picking up and scoring the sample.

As you can see, these “smart retries” mean the difference between a poor auto run and a consistently reliable one. Even recovering just a couple more points can be the tiebreaker in close matches.



## Using Marrow

To make this easier, Marrow provides a [`RetryCommand`](../../core-features/retrycommand.md) that handles the retry logic for you.

For a full breakdown of how to use it in your own code, see the [`RetryCommand` reference page](../../core-features/retrycommand.md).
