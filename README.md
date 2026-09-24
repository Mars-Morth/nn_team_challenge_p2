# Round 2: Recover the Vault 🔐

**Time limit: 45 minutes**

## The scenario

The former Nimbus Gadgets data engineer's old laptop has been found. It's
locked behind **five challenges** they left as a "proof of work" — solve all
five, assemble the pieces they give you, and you'll have the full **access
code** to unlock the vault.

This round is pure Python — no messy data files, just logic, strings, and a
bit of cleverness. Work together: split the puzzles across your team of 4,
but you'll need everyone's piece to assemble the final code.

## Rules

- Any standard Python (no special libraries required — everything here is
  solvable with core Python).
- Each puzzle gives you one **piece** of the final access code (instructions
  below tell you exactly what to extract).
- Submit each puzzle's answer as you solve it — you get points immediately,
  no need to wait until the end.
- The final code is assembled by combining your 5 pieces in order, separated
  by dashes: `PIECE1-PIECE2-PIECE3-PIECE4-PIECE5`
- First team to submit the fully correct final code gets a speed bonus.

---

## Puzzle 1 — The Cipher (warm-up)

The old laptop's password hint was left as a Caesar cipher — every letter in
the original message was shifted forward by some fixed number of places in
the alphabet (wrapping Z back to A). The problem is, whoever left this didn't
tell us the shift amount.

```
AOL VSK SHWAVW PZ SVJRLK ILOPUK MPCL JOHSSLUNLZ
```

There are only 25 possible shifts (1–25) — and you're not doing this by hand.
Write code that tries every shift and prints out all 25 candidate
decodings, then have a human eyeball which one is actual English.

(Tip: a function that takes a shift amount and returns the decoded string,
called in a loop from 1 to 25, is all you need.)

**Submit:** the decoded plaintext message, and the shift amount that worked.

**Piece 1** = the first letter of the **third word** of the decoded message.

---

## Puzzle 2 — The Rogue ID

Order IDs should each appear exactly once. Somewhere in this list, one ID has
been duplicated — a sign the export process glitched. Find it.

```python
order_ids = [1015, 1006, 1023, 1017, 1040, 1017, 1011, 1016, 1012, 1036,
             1037, 1025, 1009, 1001, 1020, 1034, 1013, 1022, 1039, 1033,
             1019, 1031, 1028, 1018, 1027, 1008, 1030, 1029, 1032, 1003,
             1002, 1014, 1038, 1024, 1007, 1035, 1005, 1004, 1026, 1010,
             1021]
```

**Submit:** the duplicated order ID.

**Piece 2** = the duplicated ID itself (as a number).

---

## Puzzle 3 — The Access Log

The laptop's login history is stored below as raw log lines. Someone was
clearly trying to brute-force their way in from one particular IP address.

Find the **IP address with the most `LOGIN_FAILED` entries**, and how many
failed attempts it has.

```
2025-03-14 09:24:41 LOGIN_FAILED user=sbrown ip=192.168.1.22
2025-03-14 09:31:04 LOGIN_FAILED user=jrossi ip=192.168.1.22
2025-03-14 09:16:50 LOGIN_SUCCESS user=sbrown ip=192.168.1.14
2025-03-14 10:08:36 LOGIN_FAILED user=jrossi ip=10.0.0.9
2025-03-14 09:32:12 LOGIN_FAILED user=jrossi ip=192.168.1.22
2025-03-14 09:37:01 LOGIN_FAILED user=mdavis ip=192.168.1.22
2025-03-14 10:16:04 LOGIN_FAILED user=kjohnson ip=10.0.0.9
2025-03-14 10:02:20 LOGIN_SUCCESS user=jrossi ip=192.168.1.35
2025-03-14 10:07:18 LOGIN_FAILED user=mdavis ip=10.0.0.9
2025-03-14 09:59:59 LOGIN_SUCCESS user=sbrown ip=192.168.1.35
2025-03-14 09:05:54 LOGIN_FAILED user=sbrown ip=192.168.1.14
2025-03-14 09:50:00 LOGIN_FAILED user=kjohnson ip=192.168.1.35
2025-03-14 09:17:28 LOGIN_SUCCESS user=mdavis ip=192.168.1.14
2025-03-14 10:18:13 LOGIN_SUCCESS user=jrossi ip=10.0.0.9
2025-03-14 09:19:05 LOGIN_SUCCESS user=kjohnson ip=192.168.1.14
2025-03-14 09:26:39 LOGIN_FAILED user=fnguyen ip=192.168.1.22
2025-03-14 09:20:38 LOGIN_FAILED user=fnguyen ip=192.168.1.22
2025-03-14 09:11:11 LOGIN_FAILED user=fnguyen ip=192.168.1.14
2025-03-14 09:46:14 LOGIN_SUCCESS user=mdavis ip=192.168.1.22
2025-03-14 10:12:06 LOGIN_FAILED user=jrossi ip=10.0.0.9
2025-03-14 09:41:37 LOGIN_SUCCESS user=kjohnson ip=192.168.1.22
2025-03-14 09:54:41 LOGIN_FAILED user=jrossi ip=192.168.1.35
2025-03-14 09:09:32 LOGIN_FAILED user=sbrown ip=192.168.1.14
```

(Tip: you don't need regex — splitting each line on spaces and `=` will get
you a long way. Regex is fine too if your team knows it.)

**Submit:** the IP address and its failed-login count.

**Piece 3** = the **last number group** of that IP address (e.g. if the IP
were `10.0.0.9`, the piece would be `9`).

---

## Puzzle 4 — The Countdown Lock

The vault's timer lock uses the **Collatz conjecture**: starting from a
number, if it's even you halve it, if it's odd you triple it and add 1 — you
repeat until you reach 1. The lock needs to know how many *steps* it takes
starting from **27**.

Rule reminder:
- If `n` is even: `n = n / 2`
- If `n` is odd: `n = 3n + 1`
- Stop when `n` reaches 1. Count how many steps it took to get there.

**Submit:** the number of steps for starting value 27.

**Piece 4** = the step count itself.

---

## Puzzle 5 — The Ledger Match

Buried in the laptop's files is a note from an old audit: *"Two transactions
on the same day were flagged — they add up to exactly $153.47. Find them."*

Here are that day's transactions:

```python
txn_ids = ["001", "002", "003", "004", "005", "006", "007", "008", "009",
           "010", "011", "012", "013", "014", "015", "016", "017", "018"]
amounts = [88.46, 90.53, 85.64, 61.15, 59.51, 44.87, 54.12, 19.34, 24.70,
           46.68, 27.23, 47.59, 10.15, 15.46, 92.32, 44.65, 52.60, 71.88]
```

Find the **two** transactions (by ID) whose amounts add up to exactly
**$153.47**. There's only one pair that works.

(A straightforward approach: loop through every possible pair of
transactions and check if their amounts sum to the target. With only 18
transactions, even checking all pairs is fast — no clever trick needed here,
just careful looping.)

**Submit:** the two transaction IDs and their amounts.

**Piece 5** = the transaction ID of the **larger** of the two matching
amounts (e.g. if the pair were `007` at $54.12 and `013` at $99.35, the
piece would be `013`).

---

## Assemble the final code

```
FINAL CODE = Piece1-Piece2-Piece3-Piece4-Piece5
```

Submit your team's full final code as soon as you have all five pieces.

Good luck — the vault is waiting. 🔓
