# A4 Write-up

**Name: Ophir Avsian**
**Onyen:oavsian**
**Game URL:http://bridges-games.herokuapp.com/assignments/4/Ophir_Avsian** 
**Bomb rule I played with (SHRINK or GAME_OVER):**

Three questions, 20 points. A few sentences each. Where a question asks for a
number, show where it came from.

---

## Question 1: The tail pointer (7 points)

Your `LinkedQueue` keeps a `tail` reference. Suppose you deleted it and found
the back of the queue by starting at `head` and following `next` until you ran
out of nodes.

What would `enqueue` cost then, in Big-O? The game calls `enqueue` once per
tick and runs about 7 ticks a second. For a snake 500 cells long, roughly how
many nodes would `enqueue` alone visit each second, and would you notice?

Now do the same for the autograder's speed test. It fills a queue with
400,000 items, then does 400,000 more rounds of dequeue-then-enqueue, so the
queue stays at 400,000. Roughly how many nodes would those enqueues visit?

```
Without the tail reference, enqueue would be O(n) because it would have to start at head and loop through the whole queue to find the last node.
For a 500 cell snake running at about 7 ticks per second, that would be around 500 * 7 = 3,500 node visits per second. 
I probably would not notice that while playing the game, but for the autograder, though, there are 400,000 enqueues on a queue that stays around 400,000 items long, so that would be about 400,000 * 400,000 = 160 billion node visits, which would be extremely slow.
```

---

## Question 2: The one method allowed to walk (7 points)

`contains` is O(n), and the game calls it through `snake.occupies(...)`. Read
`GameState.tick()` and `GameState.freeCell()` and find every call.

On a tick where the snake eats an apple, roughly how many nodes do those
`contains` calls visit in total, for a snake of length n on the 30 by 30
board? Give an expression in n. Then name a data structure from later in this
course that would make "is the snake on this cell?" fast, and say what it
would cost to keep it up to date as the snake moves.

```
On a tick where the snake eats an apple, the tick method first uses the occupies method to check the next cell, which can visit about n nodes. The freeCell method then checks all 30 * 30 = 900 cells on the board, using the occupies method for each one. This gives roughly n + 900n = 901n node visits. 
A HashSet would make checking whether a cell is occupied O(1) on average because it can look up the cell directly instead of walking through the snake. We could keep it updated by adding the new head and removing the old tail as the snake moves, which are also O(1) on average.
```

---

## Question 3: Which end is the head? (6 points)

The snake's head is the **back** of the queue, and its tail end is the
**front**. Explain why, using what happens to the body on each tick.

Then suppose you flipped it, so the head was the front. Which operation would
each tick need that `LinkedQueue` does not have, and why is that operation hard
to make O(1) on a singly linked list?

```
The snake's head is at the back of the queue because each tick adds a new head to the back with enqueue and removes the old tail from the front with dequeue and both operations are O(1). 
If the head were at the front instead, each tick would need to remove the last node from the back, which LinkedQueue does not support. In a single linked list, nodes only point forward, so we would have to walk through the list to find the node right before the last one, making the operation O(n) instead of O(1).
```
