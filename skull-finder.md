# Dr. Jonathan Helianthicus Doe's Rough and Ready, Quick and Dirty Guide to Not Failing at Skull Finder (and Minesweeper in General)

Hi there. I'm Jon Doe, and this is **not** the definitive guide on how to play minesweeper and minesweeper-like games. However, I've seen my fair share of innocent toons get nabbed by laser cogs during a friendly game of Skull Finder, and many more constantly frustrated and agonizing over what tiles to pick, often resorting to guessing even when not strictly necessary.

So, without further ado, let's learn a few things about this puzzle that pops up so often in DA Offices.

## What Skull Finder is

Skull Finder is a version of minesweeper found in DA Offices A, B, C, and D, which can be found in Lawbot HQ and are analogous to the factories of Sellbot HQ and the mints of Cashbot HQ. If you're already familiar with minesweeper, then the main differences that Skull Finder sports are these:

* The goal is not to uncover all tiles that do not have mines. Instead, the player only needs to uncover mines until there is a mine-free path from the starting side of the board to the opposite side.
* You cannot freely uncover any tile you want on the board. Since you uncover tiles by stepping on them with your toon, you can only uncover those that you have a clear path to. This makes the game a bit more difficult, since you cannot freely select tiles even when you want to do so to give yourself more info.
* You don't get a bird's eye view of the board, only the perspective of what your toon can see. Using the <kbd>Tab</kbd> key to switch the camera mode can be helpful, but it is ultimately always more awkward than the usual minesweeper game and makes it more difficult to play.
* If you uncover a tile with a mine on it and therefore lose, you not only lose the game but are forced (along with the rest of your toon team) to fight a round of laser cogs for no reward. This is very frustrating and is the whole reason for trying to play intelligently in the first place.
* The row of tiles closest to the players (the side that the toons start on) is guaranteed to be mine-free and can thus be uncovered right away at the beginning of play. This is in order to prevent players from losing right away at the beginning without any way to prevent it.

## The rules of Skull Finder

Skull finder takes place on a 7×7 square board of tiles, all of which are initially concealed. You can't see what they really have underneath. The player(s) only have access to 1 side of the board. When notating the board, I will use the convention that the bottom side of the tiles illustrated is the side closest to where the toons start, and so the top is then the closest to the opposite side, which is the place the player is trying to get to:

```
    End button
   _ _ _ _ _ _ _
  |_|_|_|_|_|_|_|
W |_|_|_|_|_|_|_| W
A |_|_|_|_|_|_|_| A
L |_|_|_|_|_|_|_| L
L |_|_|_|_|_|_|_| L
  |_|_|_|_|_|_|_|
  |_|_|_|_|_|_|_|

   Start location
```

The "end button" is a large square red button that successfully completes the puzzle when you step on it. The sides denoted "`WALL`" above are impassable walls; no tiles can be accessed from those sides.

In order to uncover tiles, you step on them with your toon. This is the only kind of action you take during the game.

While it is true that at the beginning all of the tiles are concealed, what is "concealed" by each tile is determined beforehand, before you ever get to start playing the game. The contents concealed by each covered tile is binary (meaning there are only 2 possibilities for each one). Either a tile has a mine, or it doesn't have one. Well, that's actually a bit of a lie. Tiles that don't have mines on them give more information if you manage to uncover them. Every uncovered tile (that doesn't have a mine on it) either displays an integer, or displays nothing (which is the same as displaying the number `0` ("zero")). This integer indicates exactly how many tiles that are adjacent to that tile have mines. When I say that two tiles are "adjacent", I mean that those two tiles share 1 or more of their corners (this is actually just a special case of [Chebyshev distance, or L∞ metric](https://en.wikipedia.org/wiki/Chebyshev_distance)). Below, I've marked a tile with an "`@`", and marked all tiles that are adjacent to that tile with an "`a`", for "adjacent":

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|a|a|a|_|_|_|
|_|a|@|a|_|_|_|
|_|a|a|a|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
```

The integer that an uncovered tile displays is uniquely determined by the underlying (binary) state of the board (viz. which tiles have mines and which don't) and the position of that tile within the board. That means there's only 1 possible number that any given tile in a given position can display, given a certain layout of mines on the rest of the board. Let's look at a sample (smaller than the real 7×7 board) board with all of the mines displayed (we will denote a mine with an asterisk "`*`"):

```
 _ _ _ _
|_|_|_|_|
|_|*|_|_|
|_|_|_|*|
|*|*|_|_|
```

Now let's try filling in the integers that should be displayed by the non-mine tiles when they are uncovered. Starting at the top-left and going from left-to-right, top-to-bottom, the first tile only has 3 adjacent tiles. Only 1 of those (the one situated to its bottom-right) has a mine on it, so that tile should display a 1:

```
 _ _ _ _
|1|_|_|_|
|_|*|_|_|
|_|_|_|*|
|*|*|_|_|
```

If we do this for every non-mine tile, we will get:

```
 _ _ _ _
|1|1|1|0|
|1|*|2|1|
|3|3|3|*|
|*|*|2|1|
```

Note that we're writing a "`0`" for the tile in the top-right corner, but in-game it would actually just not display a number. Also note that when you uncover one of these "zero" tiles that has no mines adjacent to it, the game will automatically uncover all of its adjacent tiles for you. This can cause a cascade of automatic tile uncovering, and if you're *really* lucky, this can even happen at the beginning of the game in such a way that the puzzle is already solved!

If the way that you win is by getting a mine-free path from the starting side to the opposite side and following the path to the big red button, then the way that you lose is by uncovering a tile that has a mine on it. In Skull Finder, a mine is represented by a skull-and-crossbones, hence the name of the game.

As noted above, the bottom row of tiles (the side that the toons start on) is guaranteed to be mine-free and can thus be uncovered right away at the beginning of play.

## Some terminology

Before talking about strategies to play Skull Finder, it's useful to first define some extra terminology to make it easier to talk about:

We say that a tile is **hot** when it has a mine. Conversely, we say that a tile is **cold** when it doesn't have a mine. We say that a tile is **warm** if, given the information from the currently revealed tiles, there is no way to prove that it is hot or that it is cold (the technical term for this is that it is [underdetermined](https://en.wikipedia.org/wiki/Underdetermination)).

We call a tile a **corner tile** if it lies on the corner of the board (there are exactly 4 such tiles on every board). We call a tile an **edge tile** if it doesn't lie on a corner, but it lies on an edge (there are exactly 20 such tiles on every board, since the boards are 7×7).

When talking about revealed tiles, we say that a revealed tile **witnesses** all of its adjacent tiles. For example, one might say: "I know for a fact that this tile with a `1` on it witnesses this hot tile here, so all other tiles that it witnesses *must* be cold".

We say that the board is **insoluble** whenever no tiles that can be revealed (i.e. tiles that are concealed but are accessible by your toon) can be proven to be cold, and there is not already a winning path revealed. This effectively means that the way towards victory is underdetermined, and if a board really is insoluble (it can be quite difficult to tell if it is), then you have no choice but to guess.

When referring to any tiles on the board, we can use coordinates to specify exactly which one we mean. Since gameplay goes from bottom to top, we label the bottom row as row 1, and we label the leftmost column as column 1. Then we can use ordered pairs of the form `(row, column)` to denote positions. For example, `(3, 5)` is displayed as "`@`" below:

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|@|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
```

When drawing out boards, the contents of each square is denoted as follows:

* `_` - Concealed tile.
* `*` - Hot tile.
* `0`, `1`, `2`, etc. - Revealed tile displaying this integer (or displaying nothing, in the case of `0`).
* `r` - Revealed tile whose displayed integer doesn't matter.
* `t` - Tempting but so far still concealed tile.
* `x` - Hypothetically hot tile.
* `~` - Underdetermined tile.

Other symbols used:

* `¬` - Logical negation/logical NOT.
* `∧` - Logical conjunction/logical AND.
* `∨` - Logical disjunction/logical OR.
* `⊕` - Logical XOR/EOR/exclusive or.
* `{a, b, c}` - The set containing the elements `a`, `b`, and `c`.

## General strategy

The first thing to do when coming across a Skull Finder puzzle is to run across all of the front-row tiles (the tiles in the row closest to where you start) and reveal them. As noted above, none of these can have mines (due to the design of the game).

Keep in mind that **Skull Finder/minesweeper is essentially a game of proofs**. All decisions that you make (i.e. tiles that you choose to reveal) should be based on knowledge that you have obtained by deductive proof, even if you don't verbalize or write down those proofs formally. Only in extreme cases (the board happens to be insoluble, or looks *so* difficult/possibly insoluble that it's more efficient to take the risk of guessing) should you ever reveal a(ny) tile(s) that you cannot prove to be cold.

Once that's done, a good general strategy for revealing tiles is to *go for the low-hanging fruit first*. What I mean is that you should look for the places where you can most easily and quickly prove that certain accessible tiles are cold, and then reveal those first. Keep doing that, and only start looking for more difficult proofs when you run out of easier ones. Low-hanging fruit much more often involves tiles that only witness very few concealed tiles. More specifically, there is a certain method for proving that tiles are cold which is perhaps the simplest. This method involves looking for tiles that witness a/some tile(s) that you *already know* are hot, and picking the ones that display an integer equal to the number of hot tiles you already know that they witness. This sounds a little convoluted, but it's really very easy to apply. Take this board for example:

```
 _ _ _ _
|_|_|_|_|
|_|_|_|_|
|_|_|2|_|
|_|1|2|1|
```

Take a look at that `2` on the bottom there. That `2` of course means that exactly 2 of the tiles adjacent to it are hot. Since it's an edge tile, it only witnesses 5 other tiles. 3 of those 5 tiles are revealed, so you know they are cold. Since there are only 2 tiles left adjacent to it that could be hot, then those 2 tiles *must* be hot. Therefore, we can rewrite this board in our minds like so:

```
 _ _ _ _
|_|_|_|_|
|_|_|_|_|
|_|*|2|*|
|_|1|2|1|
```

Now look at the other `2`. That `2` witnesses both of the tiles that we know for a fact to be hot, and since it has a `2` on it, none of its other adjacent tiles can be hot. So we can reveal those! (Here we denote a revealed tile where the number it displays doesn't matter by "`r`"):

```
 _ _ _ _
|_|_|_|_|
|_|r|r|r|
|_|*|2|*|
|_|1|2|1|
```

And we can actually do the same thing for the `1` on the left. The `1` on the right isn't so interesting, since it and all of its adjacent tiles are either revealed or known to be hot. But the `1` on the left witnesses 1 of the tiles we know to be hot, so we can deduce that all of its other adjacent tiles must be cold:

```
 _ _ _ _
|_|_|_|_|
|_|r|r|r|
|r|*|2|*|
|r|1|2|1|
```

One of the most important things about playing Skull Finder/minesweeper is that **you must keep track of all tiles that you know to be hot**, and find as many such tiles as you can. Generally this means holding the info in your head, but when playing Skull Finder, the way that you look around and see the board on the screen is quite disorienting, and in any case your memory may only take you so far. Because of this, keeping track of which tiles are proven to be hot is a matter of both keeping it in your head *as well as* re-deriving the proofs on the fly to re-prove them to yourself.

Additionally, make sure that you **take it easy**. In some cases you might not care too much, and other toons are often rushing to get all the puzzles (and everything else in the DA Office) done as fast as possible. Nevertheless, Skull Finder is a game of concentration. It requires you to remember things and to solve lots of subproblems on the fly, and most importantly, **one mistake means instantly losing the entire game**. Even if you're an expert skull finder, it often pays to double- (and triple- or quadruple-) check your work before revealing tiles. Measure twice; cut once.

**Don't go for diagonal tiles**. What I mean is this...

```
 _ _ _
|_|_|_|
|_|!|d|
|r|r|!|
```

Here, the tiles marked with "`r`" are the only revealed tiles. That makes the "`d`" tile a "diagonal" tile, since it is adjacent to a revealed tile but you can't get there directly by walking over an edge that it shares with a revealed tile; you would have to either reveal some other tile(s) first (like the "`!`" ones), or go diagonally from one tile corner to another. If the "`d`" tile is one that you proved to be cold, it can be tempting to try to approach it diagonally, bypassing both "`!`" tiles. But unless you can prove that both "`!`" tiles are cold, do not do that! Unless you really are some kind of ninja-like super-toon with the dexterity of a caffeinated mountain goat, you **will** mess it up. And when you do, chances are there will be a skull-and-crossbones and some laser cogs waiting to greet you.

## Specific strategies/case analysis

### Direct use of 1-tiles

Because `1` is the smallest number that a tile can reveal while still giving useful information, it gives the most pointed and specific information about the board around it. In any case, boards tend to be "sparse" in some sense, so they generally have a lot of `1`s. As such, the most abundant low-hanging fruits tend to be tiles that are shown to be hot by a single `1`-tile, and tiles that are shown to be cold by a single `1`-tile in combination with 1 known-hot tile. Let's break down an example that has both of these cases. The boards drawn to illustrate this example are snipped from a larger board, so the edges of the square aren't meaningful:

```
 _ _ _ _
|_|_|_|_|
|r|1|_|r|
|r|1|r|r|
|r|r|r|r|
```

If you look at the lower `1`-tile, you'll notice that it only witnesses 1 concealed tile; therefore that tile must be hot. That is what I mean by "tiles that are shown to be hot by a single `1`-tile". So we rewrite:

```
 _ _ _ _
|_|_|_|_|
|r|1|*|r|
|r|1|r|r|
|r|r|r|r|
```

Now that we have a tile that we proved to be hot, the upper `1`-tile comes in handy. Since it witnesses that hot tile, all other tiles that it witnesses must be cold. That is what I mean by "tiles that are shown to be cold by a single `1`-tile in combination with 1 known-hot tile". So we can reveal some more tiles now:

```
 _ _ _ _
|r|r|r|_|
|r|1|*|r|
|r|1|r|r|
|r|r|r|r|
```

### Two 1-tiles in a row, one being an edge tile

Here's a simple case of using an indirect proof. By "indirect" I mean using partial information, like "I know that exactly 1 of these tiles is hot", instead of absolute information like "I know that this tile and this tile are hot". Here we treat only the left side of the example board as not being snipped:

```
    ?????
   _ _ _ _
W |_|_|_|_| ?
A |_|_|_|_| ?
L |1|1|_|_| ?
L |r|r|_|_| ?
    ?????
```

If we look at the leftmost `1`-tile, it only witnesses 5 other tiles due to its status as an edge tile. And only 2 of those 5 are concealed. Therefore, exactly 1 of those 2 tiles is hot, and the other is then cold. We don't know which one is the hot and which one is the cold, but that's OK. Looking at the rightmost `1`-tile, we see that it witnesses both of the tiles that we know encompass exactly 1 hot tile. Therefore the 1 hot tile that this rightmost `1`-tile witnesses must be one of those 2, and *cannot be any of the other ones*. So we have then proved that the 3 other concealed tiles that it witnesses are cold and can reveal them:

```
    ?????
   _ _ _ _
W |_|_|_|_| ?
A |_|_|r|_| ?
L |1|1|r|_| ?
L |r|r|r|_| ?
    ?????
```

## Example game walkthrough

Here we're going to do a step-by-step walkthrough of a sample Skull Finder game. Let's see what we get when we start out:

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
```

Oh, right. OK. We have to reveal the bottom row first:

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|2|1|
|_|_|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

Ok, so right away we know from `1`-tile at `(1, 3)` that `(2, 2)` is hot. Therefore, since `(1, 1)` is a `1`-tile and witnesses that hot tile, all other tiles it witnesses are cold. So we reveal all of those (in this case just 1 tile):

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

We got lucky here, and the tile we just revealed is also a `1`-tile, so we can apply the same trick again:

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|1|1|_|_|_|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

And it looks like we got lucky yet again, so we apply the trick yet again:

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|1|1|2|_|_|_|_|
|1|1|2|_|_|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

Here we can apply the "two 1-tiles in a row, one being an edge tile" case mentioned above. The `1`-tile at `(4, 1)` witnesses that exactly 1 of `(5, 1)` and `(5, 2)` is hot. Since the `1`-tile at `(4, 2)` witnesses both of those tiles, all the other tiles it witnesses must be cold. In this case that's just 1 concealed tile, so we reveal it:

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|2|_|_|_|_|
|1|1|2|_|_|_|_|
|1|1|2|_|_|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

Taking a look at the `2`-tile at `(2, 4)`, we see that it only witnesses 2 concealed tiles, therefore both of those tiles must be hot.

Looking at the `1`-tile at `(3, 7)`, it witnesses that exactly 1 of `(4, 6)` and `(4, 7)` is hot. Since the `2`-tile at `(3, 6)` witnesses those 2 tiles as well as `(3, 5)` (which we just determined is hot), that counts for both hot tiles that it witnesses, therefore all other tiles it witnesses are cold. In this case that's only 1 concealed tile. Unfortunately, *this is a diagonal tile*. It's tempting, but don't go for it! Especially considering that at least 1 of the 2 tiles you would have to bypass is known to be hot, you're very likely to wind up fighting skelecogs by going for that one. We denote it here with "`t`", for "tempting":

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|2|_|_|_|_|
|1|1|2|_|t|_|_|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

Luckily there is an easy way that we will be able to reveal that tempting tile anyways. Looking at the `2`-tile at `(3, 3)`, we see that there are already 2 tiles that it witnesses that we know to be hot. Therefore all other tiles it witnesses are cold, and in this case that's just 1 concealed tile at `(4, 4)`. Since that tile shares an edge with our "tempting" tile that we know to be cold, we can safely reveal both of them:

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|2|_|_|_|_|
|1|1|2|3|3|_|_|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

Looking at the `3`-tile at `(4, 4)`, we see that it already witnesses 2 tiles we know to be hot. Therefore exactly 1 of the remaining tiles it witnesses is hot; in this case there's just 2 concealed tiles, `(5, 4)` and `(5, 5)`. Then, the `3`-tile to its right (at `(4, 5)`) witnesses both of those known-hot tiles as well as the two that `(4, 4)` told us contains exactly 1 hot tile. So then these 3 hot tiles occupy all the possible hot tiles that `(4, 5)` witnesses, and all others that it witnesses must be cold. We reveal both of those:

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|2|_|_|2|_|
|1|1|2|3|3|3|_|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

Notice that now the `1`-tile at `(3, 7)` implies that `(4, 7)` is hot:

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|2|_|_|2|_|
|1|1|2|3|3|3|*|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

Looking at the `3`-tile at `(4, 6)`, we see that it witnesses 2 known-hot tiles, meaning that of the 2 other concealed tiles it witnesses (`(5, 5)` and `(5, 7)`), exactly 1 is hot. Looking at the `2`-tile at `(5, 6)`, we see that it witnesses both of those tiles as well as one tile that we know to be hot. So all other tiles that it witnesses must be cold, and in this case that's a whopping 3 concealed tiles! Let's label the ones we're gonna reveal:

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|r|r|r|
|_|_|2|_|_|2|_|
|1|1|2|3|3|3|*|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

Now, here I'm going to split this off into two different games, based on two different mine layouts. One possibility is the much easier one, where we reveal those 3 tiles and get something like this:

```
 _ _ _ _ _ _ _
|_|_|_|_|1|0|0|
|_|_|_|_|2|1|0|
|_|_|2|_|_|2|1|
|1|1|2|3|3|3|*|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

And that's it for that one! There's now a clear path from row 1 all the way through row 7; column 6 has no mines in it whatsoever.

Or, we could get quite unlucky, and reveal the 3 tiles only to end up with something like this:

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|2|2|1|
|_|_|2|_|_|2|_|
|1|1|2|3|3|3|*|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

This board is quite perplexing, and as we'll see here in a moment, this board is actually insoluble. What this calls for is a bit of case analysis. Typically you only have to resort to full-blown case analysis when things get a bit tricky.

We've seen a pattern where we can often pick out a pair of 2 concealed tiles and prove that exactly 1 of them is hot. Let's call these "exclusive pairs". Looking along row 5, we see that there are 4 such pairs, and they all overlap! Let's label the concealed tiles in row 5 to make this easier:

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|2|2|1|
|a|b|2|c|d|2|e|
|1|1|2|3|3|3|*|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

Looking at the `1`-tile at `(4, 1)`, we can see that `a` and `b` form an exclusive pair. Looking at the `2`-tile at `(4, 3)`, we see that `b` and `c` form an exclusive pair. Looking at the `3`-tile at `(4, 4)`, we see that `c` and `d` form an exclusive pair. Looking at the `3`-tile at `(4, 6)`, we see that `d` and `e` form an exclusive pair. We can even write this symbolically, where hot = true and cold = false:

```
a ⊕ b
b ⊕ c
c ⊕ d
d ⊕ e
```

So then, focusing just on `c`, `d`, and `e`, we see that if `d` is hot, then both `c` and `e` are cold. Call this case 1. If `d` is cold, then both `c` and `e` must be hot. Call this case 2. Case 1 implies further that `a` is cold and `b` is hot. Conversely, case 2 implies that `a` is hot and `b` is cold. We can write these two cases out, denoting a hypothetically hot tile with "`x`":

```
[Case 1]
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|2|2|1|
|_|x|2|_|x|2|_|
|1|1|2|3|3|3|*|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|

[Case 2]
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|2|2|1|
|x|_|2|x|_|2|x|
|1|1|2|3|3|3|*|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

If we could eliminate one of these cases by showing that it leads to a contradiction, we would be right on our way. Unfortunately, we can't do so out of hand. The easiest way to look for contradictions is to go through each one of the "`x`" tiles and look at all 8 of its adjacent tiles, checking if the status (usually the integer it displays) of each one is consistent with all of **its** adjacent 8 tiles.

We could try looking into higher rows. Below I've marked the tiles that are far too underdetermined (i.e. they are only witnessed by 0 or 1 revealed tile(s)) to get any information about with "`~`":

```
 _ _ _ _ _ _ _
|~|~|~|~|_|_|_|
|~|~|~|_|2|2|1|
|_|_|2|_|_|2|_|
|1|1|2|3|3|3|*|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

This leaves 4 concealed tiles that are not one of `{a, b, c, d, e}` and also are not too underdetermined to reason about. Namely, `(6, 4)`, `(7, 5)`, `(7, 6)`, and `(7, 7)`. Let's label those as well:

```
 _ _ _ _ _ _ _
|~|~|~|~|g|h|i|
|~|~|~|f|2|2|1|
|_|_|2|_|_|2|_|
|1|1|2|3|3|3|*|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

Right away we can tell from the `2`-tile at `(6, 5)` that *at most* 1 of `{f, g, h}` is hot. Likewise, using the `2`-tile to the right at `(6, 6)` we can tell that at most 1 of `{g, h, i}` is hot. From this, we can generate 6 cases:

```
[Case I] (¬f ∧ ¬g ∧ ¬h ∧ ¬i)
 _ _ _ _ _ _ _
|~|~|~|~|_|_|_|
|~|~|~|_|2|2|1|
|_|_|2|_|_|2|_|
|1|1|2|3|3|3|*|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

Looking at this case (case I), we see that it is not compatible with case 1, since that would leave the `1`-tile at `(6, 7)` with exactly 0 adjacent hot tiles, which is a contradiction. For simplicity we will denote "case 1" as `c1`, "case I" as `cI`, etc. So we would notate this as `¬(cI ∧ c1)`. Checking for compatibility with case 2, we see that it is also not compatible with case 2 since it would leave the `2`-tile at `(6, 5)` with only 1 adjacent hot tile. So `¬(cI ∧ c1) ∧ ¬(cI ∧ c2)`, i.e. `¬(cI∧c1 ∨ cI∧c2)` (by De Morgan), which can be reduced to `¬cI` since `{c1, c2}` is exhaustive (meaning exactly 1 of them must be the case). So it can't be case I!

```
[Case II] (f ∧ ¬g ∧ ¬h ∧ ¬i)
 _ _ _ _ _ _ _
|~|~|~|~|_|_|_|
|~|~|~|x|2|2|1|
|_|_|2|_|_|2|_|
|1|1|2|3|3|3|*|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```

Looking at this case, we see that

```
[Case III] (¬f ∧ g ∧ ¬h ∧ ¬i)
 _ _ _ _ _ _ _
|~|~|~|~|x|_|_|
|~|~|~|_|2|2|1|
|_|_|2|_|_|2|_|
|1|1|2|3|3|3|*|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|

[Case IV] (¬f ∧ ¬g ∧ h ∧ ¬i)
 _ _ _ _ _ _ _
|~|~|~|~|_|x|_|
|~|~|~|_|2|2|1|
|_|_|2|_|_|2|_|
|1|1|2|3|3|3|*|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|

[Case V] (¬f ∧ ¬g ∧ ¬h ∧ i)
 _ _ _ _ _ _ _
|~|~|~|~|_|_|x|
|~|~|~|_|2|2|1|
|_|_|2|_|_|2|_|
|1|1|2|3|3|3|*|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|

[Case VI] (f ∧ ¬g ∧ ¬h ∧ i)
 _ _ _ _ _ _ _
|~|~|~|~|_|_|x|
|~|~|~|x|2|2|1|
|_|_|2|_|_|2|_|
|1|1|2|3|3|3|*|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|
```





From this case analysis it is fairly safe to conclude that this board is insoluble, and therefore you *should* attempt to guess (wisely). Of course, in a real formal proof we would have to go a bit further than this, but hopefully you get the idea.
