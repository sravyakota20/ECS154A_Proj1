# Project 1 README

Student: Sravya Kota(919987625)

## Project Status
* Finished Part 1 of the project
* Finished Part 2 of the project (fixed bug)
* Finished Part 3 of the project  (has bugs as well) Update (Fixed bug now works there was one wrong connection)
## Known Issues
* Part 2 has a small bug where the value of the gray code for 0xA and 0xB got interchanged as there are many wires I am unable to tell where I went wrong, I checked the connection for g03(or gate) but don't seem to find any error (Went through wiring fixed the issue)
* In Part 3 the bug is the number 7 doesn't light up for some reason the rest of the ones do (Bug Fixed)

## References
* [Build an XOR gate from AND/NOT](https://electronics.stackexchange.com/questions/487529/build-an-xor-gate-from-and-not)
* [Some other link](https://stackoverflow.com)

* While working on the equity comparison (E) part of the circuit, I initially used an XOR gate instead of an XNOR gate. I realized the issue after watching this video (https://www.youtube.com/watch?v=Q0Mu3pgd1Ak), where the presenter used an XNOR gate. Since I didn’t want to disturb the existing circuit, I added a NOT gate after the XOR gate to create an XNOR gate.

## Generative AI Use
I used ChatGPT for Prompt 1, and ZZZZZ for Prompt 2....

### Prompt 1
How do you create an XOR from only and/or/not gates?

### Response 1
To create an XOR (exclusive OR) gate using only AND, OR, and NOT gates, you can follow these steps. The XOR gate outputs true only when the inputs are different.

The truth table for XOR is:

| A | B | A XOR B |
|---|---|---------|
| 0 | 0 |    0    |
| 0 | 1 |    1    |
| 1 | 0 |    1    |
| 1 | 1 |    0    |

The Boolean expression for XOR can be derived as:\
A &oplus; B = (A &and; &not; B) &or; (&not; A &and; B)

This can be broken down into the following steps:

1. Create \(&not; A\) and \(&not; B\) using NOT gates.
1. Create \(A &and; &not; B\) and \(&not; A &and; B\) using AND gates.
1. Combine the results of the two AND gates using an OR gate.

Here is the step-by-step logic construction:

1. **NOT gates** to get \(&not; A\) and \(&not; B\):\
   NOT(A) &rarr; &not; A\
   NOT(B) &rarr; &not; B

1. **AND gates** to get \(A &and; &not; B\) and \(&not; A &and; B\):\
   A &and; &not; B &rarr; AND(A, &not; B)\
   &not; A &and; B &rarr; AND(&not; A, B)

1. **OR gate** to get the final result:\
   (A &and; &not; B) &or; (&not; A &and; B) &rarr; OR(AND(A, &not; B), AND(&not; A, B))

Putting it all together in a circuit diagram:

1. Input \(A\) goes to a NOT gate to produce \(&not; A\).
1. Input \(B\) goes to a NOT gate to produce \(&not; B\).
1. Input \(A\) and the output of the NOT gate for \(B\) go to an AND gate to produce \(A &and; &not; B\).
1. Input \(B\) and the output of the NOT gate for \(A\) go to an AND gate to produce \(&not; A &and; B\).
1. The outputs of the two AND gates go to an OR gate to produce the final output \(A &oplus; B\).

In terms of Boolean logic:\
XOR(A, B) = \(A &and; &not; B\) &or; \(&not; A &and; B\) 

This way, you have constructed an XOR gate using only AND, OR, and NOT gates.

### Changes 1
I needed XNOR, so changed it to:\
&not;\(\(A &and; &not; B\) &or; \(&not; A &and; B\)\)
---
### MY PROMPTS


### Prompt 1 (Comparator — equality check)

**My prompt:** In Logisim Evolution is E = XNOR(X2,Y2) * XNOR(X1,Y1) * XNOR(X0,Y0) a correct 3-bit equality?
**Assistant’s summary:** Yes. Using XNOR per bit is the canonical equality test because `XNOR(a,b)=1 ⇔ a=b`. Chaining the three bitwise equalities with a 3-input AND yields `E=1` iff all three pairs match. In Logisim you can either place the XNOR gate directly or use XOR with **Negate output? = Yes**. For completeness, this is equivalent to the AND/OR/NOT form `E = (X2Y2+¬X2¬Y2)(X1Y1+¬X1¬Y1)(X0Y0+¬X0¬Y0)`. A quick sanity check is to verify `E=1` on (000,000) and (101,101), and `E=0` on (110,111).

**What I changed:** Swapped my AND/OR/NOT XNORs for XOR with negated output and rewired the eq-gates to use the **raw** Xi/Yi (no inverted inputs).

---

### Prompt 2 (Comparator — less-than gating)

**My prompt:** Is N = (~X2 * Y2) + (eq2 * ~X1 * Y1) + (eq2 * eq1 * ~X0 * Y0) correct for unsigned X<Y?
**Assistant’s summary:** Yes. This is the standard lexicographic comparator: decide at the MSB first; if equal, fall through to the next bit. Term1 catches `0<1` at bit2; Term2 only triggers if bit2 is equal (`eq2=1`) and detects `0<1` at bit1; Term3 requires both top bits equal then checks the LSB. It assumes **unsigned** inputs and that `eqi` are **equality of the raw bits** (e.g., `eq1 = XNOR(X1,Y1)`). It’s efficient in gates and matches the behavior of Logisim’s built-in comparator. Recommended tests: (010,011) → N=1, (010,001) → N=0, (111,000) → N=0.

**What I changed:** I had mistakenly fed ~X into the equality gates; I rewired eq2/eq1/eq0 to compare the raw Xi,Yi.
---

### Prompt 3 (Gray — SID=5, G3 only)

**My prompt:** For SID=5, which minterms drive G3 high?
**Assistant’s summary:** For the custom table (column 5), **G3 = Σ m(3,4,5,6,7,8,9,10)**. Importantly, **m11 is not included** because input `1011₂` maps to `0000` (so every Gray bit is 0 there). A good two-case self-test is `0xA (1010₂) → G=1000` (needs **m10** in G3) and `0xB (1011₂) → G=0000` (proves **m11** is not in G3). If either fails, check minterm numbering (m# equals B3B2B1B0 as a binary index) and confirm your Gray input pins match Main’s splitter order.

**What I changed:** I had G3 wired to m11 and missed m10; I removed m11 and added m10. Now 0xA→1000, 0xB→0000. (The bug mentioned in the known issues)

---

### Prompt 4 (FiveSeven — single case check)

**My prompt:** With V=1,W=0,X=0,Y=1,Z=1 (i.e., 10011), which digit and segments should light?

**Assistant’s summary:** Two verticals with Z=1 encodes digit **7** (`5 + count(vert)=5+2`). In the design: `K2` must assert (use the term `V·Y·¬W·¬X` among K2’s six minterms), `D7 = K2·Z` must be 1, and segments **A,B,C** must include `D7` in their OR lists. If nothing lights, check (1) that specific K2 AND term is present, (2) `D7` uses **Z** (not `~Z`), and (3) A/B/C each include `D7`. Toggling this input also validates your splitter→(V,W,X,Y,Z) wiring order.

**What I changed:** I was missing the V·Y·¬W·¬X minterm inside K2; added it and confirmed D7 is wired into A/B/C. (Also the bug mentioned in thr known issues)

---

### Prompt 5 (FiveSeven — segment formulas confirmation)

**My prompt:** Are these seven-segment or lists correct for active high?
A= D0+D2+D3+D5+D6+D7+D8+D9,
B= D0+D1+D2+D3+D4+D7+D8+D9,
C= D0+D1+D3+D4+D5+D6+D7+D8+D9,
D= D0+D2+D3+D5+D6+D8+D9,
E= D0+D2+D6+D8,
F= D0+D4+D5+D6+D8+D9,
G= D2+D3+D4+D5+D6+D8+D9.

**Assistant’s summary:** Yes. Those are the standard active-high encodings for digits 0–9 when you have a **one-hot D0..D9**. They assume a common-cathode style seven-segment (logic 1 = segment on). If your hardware was active-low, you’d just invert each segment output at the end. A quick validation: digit 0 lights A,B,C,D,E,F (not G); digit 7 lights A,B,C only; digit 9 lights A,B,C,D,F,G.

**What I changed:** Nothing kept these exact or lists and verified them with spot tests (0,7,9).

---

**Prompt 6 (Gray — G2 list check):**
My OR for G2 is m0+m1+m2+m3+m4+m5+m6+m15. Is that the correct SID=5 set?
**Assistant’s summary:** Yes; those eight minterms match the table’s column-5 G2 pattern. Test `0x0→0111` and `0xF→0110` to verify the edges.
**What I changed:** Confirmed (As I was not sure of my calculations) no wiring changes.

