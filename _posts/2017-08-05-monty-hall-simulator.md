---
layout: post
---

The [Monty Hall problem](https://en.wikipedia.org/wiki/Monty_Hall_problem) is an infamous probability brain teaser. Here is the description of the problem:

> Suppose you're on a game show, and you're given the choice of three doors:
> Behind one door is a car;
> behind the others, goats.
> You pick a door, say No. 1, and the host, who knows what's behind the doors,
> opens another door, say No. 3, which has a goat.
> He then says to you, "Do you want to pick door No. 2?"
> Is it to your advantage to switch your choice?

The answer is that you should **_always switch_** from your first choice. So many people struggle with believing this result because it's counterintuitive. Thanks to the [simulator](https://github.com/vaskoz/monty) I wrote in Go, you can convince yourself by witnessing a random simulation of this game. But this simulator goes well beyond the three doors of the original problem. You configure the simulator with the number of doors, reveals by the host, and games.

# Installation

``` go get github.com/vaskoz/monty ```

Now run the program's help.

```
$GOPATH/bin/monty -h
Usage of /Users/vasko/gocode/bin/monty:
  -doors int
    	how many doors in the game (default 3)
  -games int
    	how many games to simulate (default 1000)
  -reveals int
    	how many doors are revealed (default 1)
```

# Default game

By default, without arguments, the program runs the original game as described above.

```
$GOPATH/bin/monty
monty-2017/08/05 22:28:55 First choice wins: 342
monty-2017/08/05 22:28:55 Second choice wins: 658
monty-2017/08/05 22:28:55 Didn't win: 0
```

The default game simulation matches the predicted result of 1/3 for your first pick and 2/3 for switching your pick.

# Custom games

Play the game with 100 doors and reveal 98.

```
$GOPATH/bin/monty -doors 100 -games 100 -reveals 98
monty-2017/08/05 22:30:26 First choice wins: 2
monty-2017/08/05 22:30:26 Second choice wins: 98
monty-2017/08/05 22:30:26 Didn't win: 0
```

Reveal one fewer.

```
$GOPATH/bin/monty -doors 100 -games 100 -reveals 97
monty-2017/08/05 22:31:19 First choice wins: 1
monty-2017/08/05 22:31:19 Second choice wins: 43
monty-2017/08/05 22:31:19 Didn't win: 56
```

Play the game with 1000 doors and reveal 500.

```
$GOPATH/bin/monty -doors 1000 -games 1000 -reveals 500
monty-2017/08/05 22:32:30 First choice wins: 1
monty-2017/08/05 22:32:30 Second choice wins: 2
monty-2017/08/05 22:32:30 Didn't win: 997
```

# Conclusion

Simulating games demonstrates probabilities in action. Experimental evidence of a random simulation should help build an intuition for problems like this.
