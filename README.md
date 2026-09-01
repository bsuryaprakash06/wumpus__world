# ExpNo 9: Solve Wumpus World Problem Using Python Demonstrating Inferences from Propositional Logic

| **Name**                       | Surya Prakash B  |
| ------------------------------ | - |
| **Register Number** | 212224230281  |

## Aim

To solve the **Wumpus World Problem** using Python and demonstrate inference from **Propositional Logic**.

---

# Problem Description

## Wumpus World

The **Wumpus World** is a classic Artificial Intelligence problem used to demonstrate:

* Knowledge representation
* Propositional logic
* Logical inference
* Intelligent agents
* Decision making
* Reasoning in an uncertain environment

The Wumpus World consists of a grid containing different objects and hazards:

* **Wumpus**
* **Pit**
* **Gold**
* **Breeze**
* **Smell / Stench**
* **Safe cells**

The agent starts from the upper-left corner and attempts to reach the Gold while avoiding dangerous cells.

![Wumpus World Representation](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/cd6b68dc-c79f-4dcb-8126-04da90d65912)

---

# Wumpus World Representation

The program uses a `4 × 4` grid:

```text
+---------+---------+---------+---------+
|  Safe   | Breeze  |   PIT   | Breeze  |
+---------+---------+---------+---------+
|  Smell  |  Safe   | Breeze  |  Safe   |
+---------+---------+---------+---------+
| WUMPUS  |  GOLD   |   PIT   | Breeze  |
+---------+---------+---------+---------+
|  Smell  |  Safe   | Breeze  |   PIT   |
+---------+---------+---------+---------+
```

The agent starts at:

```text
(0, 0)
```

The objective is to reach:

```text
GOLD
```

without falling into a Pit or encountering the Wumpus.

---

# Percepts

The agent receives information from its current environment.

## Breeze

A **Breeze** indicates that a Pit may be present in a neighboring cell.

```text
Breeze → Possible nearby Pit
```

## Smell / Stench

A **Smell** indicates that the Wumpus may be present in a neighboring cell.

```text
Smell → Possible nearby Wumpus
```

These percepts allow an intelligent agent to reason about hidden dangers.

---

# Propositional Logic

Propositional Logic can be used to represent facts about the Wumpus World.

For example:

```text
P(x,y) → There is a Pit at position (x,y)

W(x,y) → There is a Wumpus at position (x,y)

B(x,y) → There is a Breeze at position (x,y)

S(x,y) → There is a Smell at position (x,y)
```

The agent can use logical rules to infer information about neighboring cells.

For example:

```text
B(x,y) → Pit exists in one of the adjacent cells
```

and:

```text
S(x,y) → Wumpus exists in one of the adjacent cells
```

---

# Agent Actions

The agent can move in four directions.

| Input | Action     |
| ----- | ---------- |
| `u`   | Move Up    |
| `d`   | Move Down  |
| `l`   | Move Left  |
| `r`   | Move Right |

The program checks the boundaries of the `4 × 4` grid before allowing a movement.

---

# Arrow

The agent starts with one arrow:

```python
arrow = True
```

When the agent encounters a `Smell`, it can choose whether to throw the arrow.

If the arrow hits the Wumpus:

```text
Wumpus killed!
```

The player receives:

```text
+1000 points
```

After the arrow is used:

```python
arrow = False
```

The arrow cannot be used again.

---

# Scoring

| Event            |   Score |
| ---------------- | ------: |
| Kill Wumpus      | `+1000` |
| Find Gold        | `+1000` |
| Waste Arrow      |   `-10` |
| Encounter Wumpus | `-1000` |
| Fall into Pit    | `-1000` |

---

# Algorithm

1. Initialize the Wumpus World grid.
2. Set the player's starting position to `(0,0)`.
3. Initialize the arrow as available.
4. Set the initial score to `0`.
5. Ask the player to select a movement direction.
6. Check whether the movement is valid.
7. Move the player to the selected cell.
8. Display the current location.
9. Check whether the current cell contains a `Smell`.
10. If a Smell is detected:

    * Ask whether the player wants to throw the arrow.
    * Ask for the arrow direction.
    * Check the neighboring cell.
    * Kill the Wumpus if it is present.
11. Check whether the player has reached the Wumpus.
12. If the player reaches the Wumpus:

    * Decrease the score.
    * End the game.
13. Check whether the player has reached the Gold.
14. If Gold is found:

    * Increase the score by `1000`.
    * Display the winning message.
    * End the game.
15. Check whether the player has fallen into a Pit.
16. If the player falls into a Pit:

    * Decrease the score.
    * End the game.
17. Continue until the player wins or loses.

---

# Python 3 Program

```python
wumpus = [
    ["Safe", "Breeze", "PIT", "Breeze"],
    ["Smell", "Safe", "Breeze", "Safe"],
    ["WUMPUS", "GOLD", "PIT", "Breeze"],
    ["Smell", "Safe", "Breeze", "PIT"]
]


# Initial variables
row = 0
column = 0
arrow = True
player = True
score = 0


# Game loop
while player:

    choice = input(
        "Press u to move up\n"
        "Press d to move down\n"
        "Press l to move left\n"
        "Press r to move right\n"
    )


    # Move Up
    if choice == "u":

        if row != 0:
            row -= 1
        else:
            print("Move denied")

        print(
            "Current location:",
            wumpus[row][column],
            "\n"
        )


    # Move Down
    elif choice == "d":

        if row != 3:
            row += 1
        else:
            print("Move denied")

        print(
            "Current location:",
            wumpus[row][column],
            "\n"
        )


    # Move Left
    elif choice == "l":

        if column != 0:
            column -= 1
        else:
            print("Move denied")

        print(
            "Current location:",
            wumpus[row][column],
            "\n"
        )


    # Move Right
    elif choice == "r":

        if column != 3:
            column += 1
        else:
            print("Move denied")

        print(
            "Current location:",
            wumpus[row][column],
            "\n"
        )


    # Invalid movement
    else:

        print("Move denied")


    # Check for smell
    if wumpus[row][column] == "Smell" and arrow:

        arrow_choice = input(
            "Do you want to throw an arrow?\n"
            "Press y to throw\n"
            "Press n to save your arrow\n"
        )


        if arrow_choice == "y":

            arrow_throw = input(
                "Press u to throw up\n"
                "Press d to throw down\n"
                "Press l to throw left\n"
                "Press r to throw right\n"
            )


            # Throw arrow Up
            if arrow_throw == "u":

                if row > 0 and wumpus[row - 1][column] == "WUMPUS":

                    print("Wumpus killed!")

                    score += 1000

                    print("Score:", score)

                    wumpus[row - 1][column] = "Safe"

                else:

                    print("Arrow wasted...")

                    score -= 10

                    print("Score:", score)


            # Throw arrow Down
            elif arrow_throw == "d":

                if row < 3 and wumpus[row + 1][column] == "WUMPUS":

                    print("Wumpus killed!")

                    score += 1000

                    print("Score:", score)

                    wumpus[row + 1][column] = "Safe"

                else:

                    print("Arrow wasted...")

                    score -= 10

                    print("Score:", score)


            # Throw arrow Left
            elif arrow_throw == "l":

                if column > 0 and wumpus[row][column - 1] == "WUMPUS":

                    print("Wumpus killed!")

                    score += 1000

                    print("Score:", score)

                    wumpus[row][column - 1] = "Safe"

                else:

                    print("Arrow wasted...")

                    score -= 10

                    print("Score:", score)


            # Throw arrow Right
            elif arrow_throw == "r":

                if column < 3 and wumpus[row][column + 1] == "WUMPUS":

                    print("Wumpus killed!")

                    score += 1000

                    print("Score:", score)

                    wumpus[row][column + 1] = "Safe"

                else:

                    print("Arrow wasted...")

                    score -= 10

                    print("Score:", score)

            else:

                print("Invalid arrow direction.")


            # Arrow can be used only once
            arrow = False


    # Check Wumpus
    if wumpus[row][column] == "WUMPUS":

        score -= 1000

        print(
            "\nWumpus here!!"
            "\nYou Die"
            "\nYour score is:",
            score
        )

        break


    # Check Gold
    if wumpus[row][column] == "GOLD":

        score += 1000

        print(
            "\nCongratulations!"
            "\nYou found the GOLD!"
            "\nYou escaped the Wumpus World!"
            "\nYour score is:",
            score
        )

        break


    # Check Pit
    if wumpus[row][column] == "PIT":

        score -= 1000

        print(
            "\nAhhhhh!!!!"
            "\nYou fell in the pit."
            "\nYour score is:",
            score
        )

        break
```

---

# Understanding the Program

## 1. Wumpus World Grid

The environment is stored as a nested list:

```python
wumpus = [
    ["Safe", "Breeze", "PIT", "Breeze"],
    ["Smell", "Safe", "Breeze", "Safe"],
    ["WUMPUS", "GOLD", "PIT", "Breeze"],
    ["Smell", "Safe", "Breeze", "PIT"]
]
```

A cell can contain:

```text
Safe
Breeze
Smell
PIT
WUMPUS
GOLD
```

---

# 2. Initial State

The agent starts at:

```python
row = 0
column = 0
```

Therefore:

```text
Position = (0,0)
```

The agent also has:

```python
arrow = True
```

which means one arrow is available.

The initial score is:

```python
score = 0
```

---

# 3. Movement

The agent moves using row and column coordinates.

```text
Up    → row - 1
Down  → row + 1
Left  → column - 1
Right → column + 1
```

For example:

```python
if choice == "d":
    if row != 3:
        row += 1
```

This moves the agent one cell downward.

---

# 4. Boundary Checking

The program prevents the player from leaving the grid.

For example:

```python
if row != 0:
    row -= 1
else:
    print("Move denied")
```

If the player is already in the first row, moving upward is not allowed.

Similarly, the maximum row and column index is `3`.

---

# 5. Smell Detection

The program checks:

```python
if wumpus[row][column] == "Smell" and arrow:
```

If the current cell contains a Smell and the arrow is still available, the player is asked whether to use it.

This represents the percept:

```text
Smell
  ↓
Possible nearby Wumpus
```

---

# 6. Shooting the Arrow

The program checks the neighboring cell.

For example, when shooting down:

```python
if row < 3 and wumpus[row + 1][column] == "WUMPUS":
```

If the Wumpus is found:

```python
score += 1000
```

and:

```python
wumpus[row + 1][column] = "Safe"
```

The Wumpus is removed from that location.

---

# 7. Gold

When the player reaches Gold:

```python
if wumpus[row][column] == "GOLD":
```

the score is increased:

```python
score += 1000
```

and the game terminates using:

```python
break
```

---

# 8. Pit

If the player enters a Pit:

```python
if wumpus[row][column] == "PIT":
```

the player loses:

```python
score -= 1000
```

and the game ends.

---

# 9. Wumpus

If the player enters the Wumpus cell:

```python
if wumpus[row][column] == "WUMPUS":
```

the player dies and loses:

```python
score -= 1000
```

---

# Propositional Logic Inference

The important AI concept demonstrated by Wumpus World is **inference**.

The agent observes a percept and uses it to reason about possible hidden states.

For example:

```text
             Breeze
                |
                ↓
      Possible nearby Pit
```

Similarly:

```text
             Smell
                |
                ↓
    Possible nearby Wumpus
```

The agent can combine multiple observations to eliminate impossible locations.

For example, if a cell has no Breeze:

```text
No Breeze
    ↓
No adjacent Pit
```

And if a cell has no Smell:

```text
No Smell
    ↓
No adjacent Wumpus
```

These logical relationships form the basis of a knowledge-based agent.

---

# Example of Inference

Suppose the agent is at:

```text
(1,1)
```

and observes:

```text
Breeze
```

Its adjacent cells are:

```text
(0,1)
(2,1)
(1,0)
(1,2)
```

The agent can infer:

```text
At least one adjacent cell may contain a Pit.
```

Similarly, if it detects:

```text
Smell
```

it can infer:

```text
At least one adjacent cell may contain the Wumpus.
```

This is an example of reasoning from **percepts to hidden environmental information**.

---

# Sample Input and Output

![Sample Input and Output 1](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/8696111a-a4a7-47cb-ba4b-43a4ef88573f)

![Sample Input and Output 2](https://github.com/natsaravanan/19AI405FUNDAMENTALSOFARTIFICIALINTELLIGENCE/assets/87870499/4be5bf06-79fa-4fa0-9334-38a33f06060b)

---

# Example Output

When the player finds the Gold:

```text
Current location: GOLD

Congratulations!
You found the GOLD!
You escaped the Wumpus World!
Your score is: 1000
```

If the player falls into a Pit:

```text
Current location: PIT

Ahhhhh!!!!
You fell in the pit.
Your score is: -1000
```

If the player encounters the Wumpus:

```text
Current location: WUMPUS

Wumpus here!!
You Die
Your score is: -1000
```

---

# Advantages

* Demonstrates the concept of an intelligent agent.
* Demonstrates basic propositional reasoning.
* Shows how percepts can provide information about hidden objects.
* Provides a simple interactive environment.
* Demonstrates decision making using environmental information.

---

# Limitations

This implementation is a **simplified Wumpus World simulation**.

The world is predefined:

```python
wumpus = [
    ["Safe", "Breeze", "PIT", "Breeze"],
    ["Smell", "Safe", "Breeze", "Safe"],
    ["WUMPUS", "GOLD", "PIT", "Breeze"],
    ["Smell", "Safe", "Breeze", "PIT"]
]
```

The player manually chooses the movements.

It does not implement a complete automated propositional-logic inference engine.

A more advanced implementation could maintain a knowledge base containing propositions such as:

```text
P(1,2)
W(2,1)
B(1,1)
S(2,0)
```

and automatically infer safe and unsafe cells.

---

# Key Concepts

| Concept                 | Meaning                                           |
| ----------------------- | ------------------------------------------------- |
| **Wumpus World**        | Environment used to study intelligent agents      |
| **Agent**               | Entity that perceives and acts in the environment |
| **Percept**             | Information obtained from the environment         |
| **Breeze**              | Indicates a possible nearby Pit                   |
| **Smell / Stench**      | Indicates a possible nearby Wumpus                |
| **Pit**                 | Dangerous location                                |
| **Wumpus**              | Dangerous creature                                |
| **Gold**                | Goal/reward                                       |
| **Propositional Logic** | Logic for representing facts                      |
| **Inference**           | Deriving new information from known facts         |
| **Knowledge Base**      | Collection of facts and rules                     |
| **Action**              | Movement or arrow operation                       |

---

# Result

Thus, the **Wumpus World Problem** was successfully implemented using Python. The program demonstrates an agent navigating a grid environment, perceiving **Breeze** and **Smell**, using an arrow to eliminate the Wumpus, avoiding Pits, finding Gold, and demonstrating the basic concept of **Inference using Propositional Logic**.
