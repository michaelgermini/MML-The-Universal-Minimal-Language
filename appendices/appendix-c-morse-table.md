# Appendix C — Complete Morse Code Table (ITU)

## International Morse Code (ITU-R M.1677-1)

The Morse code used by MML follows international standards defined by the ITU (International Telecommunication Union). This table presents all supported characters with their Morse equivalent.

### Latin alphabet (A-Z)

| Letter | Morse Code | Dots/Dashes | Letter | Morse Code | Dots/Dashes |
|--------|-----------|-------------|--------|------------|-------------|
| **A** | ·- | 1 dot, 1 dash | **N** | -. | 1 dash, 1 dot |
| **B** | -··· | 1 dash, 3 dots | **O** | --- | 3 dashes |
| **C** | -·-· | 1 dash, 1 dot, 1 dash, 1 dot | **P** | ·--· | 1 dot, 2 dashes, 1 dot |
| **D** | -·· | 1 dash, 2 dots | **Q** | --·- | 2 dashes, 1 dot, 1 dash |
| **E** | · | 1 dot | **R** | ·-· | 1 dot, 1 dash, 1 dot |
| **F** | ··-· | 2 dots, 1 dash, 1 dot | **S** | ··· | 3 dots |
| **G** | --· | 2 dashes, 1 dot | **T** | - | 1 dash |
| **H** | ···· | 4 dots | **U** | ··- | 2 dots, 1 dash |
| **I** | ·· | 2 dots | **V** | ···- | 3 dots, 1 dash |
| **J** | ·--- | 1 dot, 3 dashes | **W** | ·-- | 1 dot, 2 dashes |
| **K** | -·- | 1 dash, 1 dot, 1 dash | **X** | -··- | 1 dash, 2 dots, 1 dash |
| **L** | ·-·· | 1 dot, 1 dash, 2 dots | **Y** | -·-- | 1 dash, 1 dot, 2 dashes |
| **M** | -- | 2 dashes | **Z** | --·· | 2 dashes, 2 dots |

### Digits (0-9)

| Digit | Morse Code | Dots/Dashes | Digit | Morse Code | Dots/Dashes |
|-------|------------|-------------|-------|------------|-------------|
| **0** | ----- | 5 dashes | **5** | ····· | 5 dots |
| **1** | ·---- | 1 dot, 4 dashes | **6** | -···· | 1 dash, 4 dots |
| **2** | ··--- | 2 dots, 3 dashes | **7** | --··· | 2 dashes, 3 dots |
| **3** | ···-- | 3 dots, 2 dashes | **8** | ---·· | 3 dashes, 2 dots |
| **4** | ····- | 4 dots, 1 dash | **9** | ----· | 4 dashes, 1 dot |

### Punctuation and symbols

| Symbol | Morse Code | Dots/Dashes | Symbol | Morse Code | Dots/Dashes |
|--------|------------|-------------|--------|------------|-------------|
| **.** (period) | ·-·-· | 1p,1d,1p,1d | **(** (opening parenthesis) | -·--· | 1d,1p,2d,1p |
| **,** (comma) | --··-- | 2d,2p,2d | **)** (closing parenthesis) | -·--·- | 1d,1p,2d,1p,1d |
| **?** (question mark) | ··--·· | 2p,2d,2p | **'** (apostrophe) | ·----· | 1p,4d,1p |
| **'** (single quote) | ·-··-· | 1p,1d,2p,1d,1p | **"** (double quote) | ·-··-· | 1p,1d,2p,1d,1p |
| **/** (slash) | -··-· | 1d,2p,1d,1p | **:** (colon) | ---··· | 3d,3p |
| **;** (semicolon) | -·-·-· | 1d,1p,1d,1p,1d | **=** (equals) | -···- | 1d,3p,1d |
| **+** (plus) | ·-·-· | 1p,1d,1p,1d,1p | **-** (minus) | -····- | 1d,4p,1d |
| **_** (underscore) | ··--·- | 2p,2d,1p,1d | **$** (dollar) | ···-··- | 3p,1d,2p,1d |
| **@** (at) | ·--·-· | 1p,2d,1p,1d,1p | **&** (ampersand) | ·-··· | 1p,1d,3p |

### Special signs and prosigns

| Sign | Morse Code | Meaning |
|------|------------|---------|
| **/** | -··-· | Letter separation (slash) |
| **/** | ··--·· | Alternative question mark |
| **/** | ·-·- | Alternative period |
| **/** | ·----· | Opening quote |
| **/** | -·--· | Closing quote |
| **/** | ·-··- | Apostrophe |
| **/** | -·-·- | Alternative semicolon |
| **/** | ---··· | Alternative colon |

### Prosign codes (procedure signals)

| Prosign | Morse Code | Meaning |
|---------|------------|---------|
| **AA** | ·-·- | New line |
| **AR** | ·-·- | End of transmission |
| **AS** | ·-··· | Wait |
| **BK** | -···-·- | Break (interrupt) |
| **BT** | -···- | Separation of new groups |
| **CL** | -·-··-· | Closing (end of communication) |
| **CQ** | -·-··-·- | General call |
| **CT** | -·-··- | Received five words |
| **DE** | -·· | From (sender) |
| **K** | -·- | Invitation to transmit |
| **KN** | -·--· | Specific invitation |
| **R** | ·-· | Received (positive) |
| **SK** | ···-·- | End of transmission |
| **SN** | ···-· | Understood |
| **SOS** | ···---··· | Distress signal |

## Signal duration

### Time units
- **Dot (·)**: 1 unit
- **Dash (-)**: 3 units
- **Space between elements**: 1 unit
- **Space between letters**: 3 units
- **Space between words**: 7 units

### Transmission speed
- **Beginner**: 5-10 words/minute (WPM)
- **Experienced operator**: 15-25 WPM
- **Record**: 75 WPM (Ted McElroy, 1939)

## Adaptation for MML

### Priority characters
MML prioritizes short Morse characters for frequent tags:
- **T**: - (1 dash) → Very fast
- **E**: · (1 dot) → Ultra fast
- **H**: ···· (4 dots) → Fast despite length
- **M**: -- (2 dashes) → Efficient

### MMLC optimizations
The compressed MMLC version uses digits (0-9) which have short Morse codes:
- **1**: ·---- (1+4 = 5 units)
- **2**: ··--- (2+3 = 5 units)
- **3**: ···-- (3+2 = 5 units)
- **4**: ····- (4+1 = 5 units)
- **5**: ····· (5 units)

### Practical transmission
For optimal MML transmission:
1. Use prosigns to structure communication
2. Group similar elements
3. Repeat critical information
4. Use abbreviated codes for frequent terms

---

**Reference**: ITU-R M.1677-1 (10/2009) - International recommendation for radiotelegraphy emissions in the hectometric band. This table complies with international standards for Morse transmission in telecommunications.

