# OpeningSearchChess
Project for a certain computer science course in a certain university for the 2025 academic school year.

Contributors:
- Guanlin Chen
- Shang Sun
- Jerry Zhang

# Installation
Have `python` installed to some reasonable version

`git clone` this

do `pip install -r requirements.txt` inside the project folder (with these files)

Then run `main.py` to start.

# Sourcing and Using datasets
The chess datasets were taken from [lichess.com games database](https://database.lichess.org/) , it is fine to have your own `.pgn` files from anywhere you want.

In `main.py`, as it was designed with some existing sample datasets, you may need to remove some existing starter code, with new `<tc>` being a timecontrol of your choice:

```python3
def select_dataset():
  # Remove everything else that was here before
  return "path_to_your_chess_database.pgn", <tc>
```

# Usage
Run `main.py`. You can use stuff like VSCode or PyCharm to do so, or use the terminal directly (`python main.py`).

We chose to make the ChessExplorer use filesystem commands to navigate through moves due to a filesystem's tree-like nature. If you are familiar with the terminal, you may find the ChessExplorer easy to use.

## Starting
These instructions assume that main.py has not been modified (i.e., you’re using the provided sample code). Follow the on-screen prompts as they appear.
- **Maximum # of opening moves:** Determines the maximum “levels,” or the number of moves deep the ChessExplorer will analyze in a game. We’ve set a limit of 5 moves for performance reasons, but this can be changed by editing the max_moves function in `main.py`.
- **Dataset to load:** Selects which sample dataset the program will analyze.

## Traversing
Upon finishing the loading and processing, you are presented with a prompt, and a help menu of commands:

| COMMAND | DESCRIPTION | EXAMPLE | 
| -------- | ------------ | --------- |
|`ls [asc\|desc\|played]` | **List commonly played moves** from the current board state. Optional filter based on playrate. | `ls desc` |
|`cd (move)` | **Play the given moves** in algebraic notation, if legal. Can chain moves as a `/`-seperated list. | `cd c4/e5`|
|`cd ..` | **Undo** last move.| `cd ..` |
|`cd ~` | **Reset** to initial game state. | `cd ~` |
|`stats [tc]` | **Display data** (winrate, playrate) for current board state. Optional timecontrol override for using said timecontrol's data instead of using the `settc` command. | `stats 180`|
|`help`| Displays help menu | `help` |
|`settc (tc)` | **Set global default timecontrol** (for `ls`, `stats`, etc.) to given timecontrol. | `settc 300` |
|`timecontrols` | **Display common timecontrols**. Just a reminder for folks like me who don't know chess. |`timecontrols`|
|`tree`| **Display tree of next possible moves** from current board state. Alternative to `ls` with less data, but more deeper levels. | `tree`|

The prompt contains information about the current board state (what moves were played, in order) and allows you to input one of the commands above. It looks like a string of
moves in algebraic notation, seperated by a slash `/` and ends with a colon `:`. For examples:
- `/:` No moves have been played.
- `/e4/e5/Nf3/Nc6:` The listed moves have been played, in order of their listing from left-to-right. This sequence corresponds with the opening _King's Knight Opening: Normal Variation_.

Suppose you want to calculate the statistics for games with timecontrol 300 seconds (5 minutes), finding the most common move played after the first move of `Nf3`:

```
/dunno/where/you/started/so/: cd ~
/: settc 300
Set global timecontrol to 300.
/: cd Nf3
/Nf3: ls desc
NEXT MOVE     PLAYRATE    WHITE WIN     BLACK WIN     DRAW       NAME
d5            32.62%      45.65%        54.35%        0.0%       Zukertort Opening
...           ...         ...           ...           ...        ...
...           ...         ...           ...           ...        ...
/Nf3:
```
So it seems that _for the used game dataset(s) and games with a timecontrol of 300_, the most common move played after a first move of `Nf3` is `d5`, with ~33% players facing said situation playing `d5`.

# Places for Improvement
- Instead of precomputing all statistics, calculate them dynamically from the current move sequence, or cache results after the first calculation to avoid wasted work on unexplored paths.
- The nested dictionary setup in ChessData works but isn’t elegant. It was designed for handling multiple time controls, though a cleaner structure would likely perform better.
- Keeping filesystem-like commands (ls, cd) wasn’t ideal. More thematic ones (nextmoves, play, reset) would fit better but needed extra testing time.
- Using python-chess to show a text-based board would be a nice addition, as the move history already provides enough data to support it.
