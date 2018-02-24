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

Skull finder takes place on a 7×7 square board of tiles, all of which are initially concealed. You can't see what they really have underneath. The player(s) only have access to one side of the board. When notating the board, I will use the convention that the bottom side of the tiles illustrated is the side closest to where the toons start, and so the top is then the closest to the opposite side, which is the place the player is trying to get to:

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

While it is true that at the beginning all of the tiles are concealed, what is "concealed" by each tile is determined beforehand, before you ever get to start playing the game. The contents concealed by each covered tile is binary (meaning there are only two possibilities for each one). Either a tile has a mine, or it doesn't have one. Well, that's actually a bit of a lie. Tiles that don't have mines on them give more information if you manage to uncover them. Every uncovered tile (that doesn't have a mine on it) either displays an integer, or displays nothing (which is the same as displaying the number `0` ("zero")). This integer indicates exactly how many tiles that are adjacent to that tile have mines. When I say that two tiles are "adjacent", I mean that those two tiles share one or more of their corners. Below, I've marked a tile with an "`X`", and marked all tiles that are adjacent to that tile with an "`a`", for "adjacent":

```
 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|a|a|a|_|_|_|
|_|a|X|a|_|_|_|
|_|a|a|a|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
```

The integer that an uncovered tile displays is uniquely determined by the underlying (binary) state of the board (viz. which tiles have mines and which don't) and the position of that tile within the board. That means there's only one possible number that any given tile in a given position can display, given a certain layout of mines on the rest of the board. Let's look at a sample (smaller than the real 7×7 board) board with all of the mines displayed (we will denote a mine with an asterisk "`*`"):

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

Take a look at that `2` on the bottom there. That `2` of course means that exactly 2 of the tiles adjacent to it are hot. Since it's an edge tile, it only witnesses 5 other tiles. 3 of those 5 tiles are revealed, so you know they are cold. Since there are only 2 tiles left adjacent to it that could be hot, then those two tiles *must* be hot. Therefore, we can rewrite this board in our minds like so:

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

If you look at the lower `1`-tile, you'll notice that it only witnesses one concealed tile; therefore that tile must be hot. That is what I mean by "tiles that are shown to be hot by a single `1`-tile". So we rewrite:

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

Here's a simple case of using an indirect proof. By "indirect" I mean using partial information, like "I know that exactly one of these tiles is hot", instead of absolute information like "I know that this tile and this tile are hot". Here we treat only the left side of the example board as not being snipped:

```
    ?????
   _ _ _ _
W |_|_|_|_| ?
A |_|_|_|_| ?
L |1|1|_|_| ?
L |r|r|_|_| ?
    ?????
```

If we look at the leftmost `1`-tile, it only witnesses 5 other tiles due to its status as an edge tile. And only 2 of those 5 are concealed. Therefore, exactly 1 of those 2 tiles is hot, and the other is then cold. We don't know which one is the hot and which one is the cold, but that's OK. Looking at the rightmost `1`-tile, we see that it witnesses both of the tiles that we know encompass exactly 1 hot tile. Therefore the 1 hot tile that this rightmost `1`-tile witnesses must be one of those two, and *cannot be any of the other ones*. So we have then proved that the 3 other concealed tiles that it witnesses are cold and can reveal them:

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

```
 _ _ _ _ _ _ _
|*|_|_|_|_|_|_|
|_|_|_|*|_|_|_|
|_|*|_|_|*|_|_|
|_|_|_|_|_|_|*|
|_|_|_|*|*|_|_|
|_|*|_|_|_|_|_|
|_|_|_|_|_|_|_|

 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|2|1|
|_|_|2|2|2|1|0|
|1|1|1|0|0|0|0|

 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|

 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|1|1|_|_|_|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|

 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|1|1|2|_|_|_|_|
|1|1|2|_|_|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|

 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|2|_|_|_|_|
|1|1|2|_|_|_|_|
|1|1|2|_|_|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|

 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|2|_|_|_|_|
|1|1|2|_|t|_|_|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|

 _ _ _ _ _ _ _
|_|_|_|_|_|_|_|
|_|_|_|_|_|_|_|
|_|_|2|_|_|_|_|
|1|1|2|3|3|_|_|
|1|1|2|*|*|2|1|
|1|*|2|2|2|1|0|
|1|1|1|0|0|0|0|

```

║
