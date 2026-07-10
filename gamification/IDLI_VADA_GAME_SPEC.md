# Idli–Vada–Sambhar Game
## Functional Specification & Implementation Plan

**Version:** 1.1

> **Note:** This specification incorporates:
> - Random starting player every round.
> - Multi-select phrase buttons (Idli/Vada/Sambhar).
> - React-only implementation with no backend.

## Objective

Build a browser-based educational game that teaches:
- Multiples
- Common Multiples
- Least Common Multiple (LCM)

The player competes against two computer-controlled children:
- User
- Tenali
- Birbal

The experience should feel like children sitting in a circle and playing together.

---

## Technology Stack

### Frontend
- React 19
- JavaScript (ES6)
- Vite
- Functional Components
- React Hooks
- Plain CSS

### Backend
- None
- No database
- No authentication
- Everything runs locally inside React.

---

## Inputs

Support two modes.

### Two-number mode
- a
- b

Words:
- Idli
- Vada

### Three-number mode
- a
- b
- c

Words:
- Idli
- Vada
- Sambhar

Validation:
- Integers only
- 2–100
- No duplicates
- Third number optional

---

## Mapping

- a → Idli
- b → Vada
- c → Sambhar

---

## Replacement Rules

- Multiple of a → Idli
- Multiple of b → Vada
- Multiple of c → Sambhar
- Multiple of a & b → Idli Vada
- Multiple of a & c → Idli Sambhar
- Multiple of b & c → Vada Sambhar
- Multiple of a, b & c → Idli Vada Sambhar
- Otherwise say the number.

Always display phrases in the order:

Idli → Vada → Sambhar

---

## Starting Player Rotation

A round consists of exactly one turn for each player.

At the beginning of every round:

1. Randomly choose the starting player.
2. Rotate the player order.
3. Continue counting (numbers never reset).

Example:

Round 1:
User → Tenali → Birbal

Round 2:
Birbal → User → Tenali

Round 3:
Tenali → Birbal → User

The same player may start consecutive rounds.

---

## User Interface

Top section:
- Input boxes for a, b, c
- Start Game button

Middle:
- User
- Tenali
- Birbal

Highlight the active player.

Current turn displays:
- Current player
- Current count

### Answer Controls

Buttons:

- Number
- Idli
- Vada
- Sambhar

If Number is selected, show a numeric textbox.

Phrase buttons are toggle buttons.

Examples:

- Idli
- Idli + Vada
- Idli + Sambhar
- Vada + Sambhar
- Idli + Vada + Sambhar

Press **Submit** to answer.

---

## Computer Players

Tenali and Birbal always answer correctly.

Delay:
- Random 600–900 ms.

---

## Mistake Handling

Incorrect answer:

- Show message.
- Allow one retry.

If second attempt is wrong:
- Reveal correct answer.
- Continue game.

Track:
- Correct
- Mistakes
- Retries
- Accuracy

---



## End Condition

Two-number mode:
- Stop at first Idli Vada (LCM(a,b))

Three-number mode:
- Stop at first Idli Vada Sambhar (LCM(a,b,c))

Display:

- Congratulations
- LCM discovered
- Continue
- Play Again

---

## Continue Mode

Continue counting indefinitely after the first LCM if the user chooses Continue.

---


## Implementation Phases

### Phase 1
- Input screen
- Two/three-number mode
- Create three players User , Tenali and Birbal
- Create all the required UI buttons described above

### Phase 2
- Assume User is always the starting player
- Create an engine that goes in a cyclic way as described above
- Stop if user gives wrong input


### Phase 3

- Implement Random starting player
- Implement Retry logic
- Implement end game / Continue mode


### Phase 4
- UI polish

---

## Acceptance Tests

1. 3,5 stops at 15.
2. 2,4 stops at 4.
3. 3,5,7 stops at 105.
4. Retry works.
5. Wrong twice reveals answer.
6. History updates correctly.
7. Accuracy computed correctly.
8. Starting player rotates randomly.
9. Toggle buttons correctly create compound phrases.

---

## Future Enhancements

- Voice input
- Speech synthesis
- Speed increases
- Teacher dashboard
- Leaderboards
- Mobile app
- Additional food themes
