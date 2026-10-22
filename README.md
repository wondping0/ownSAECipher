<div align="center">

# ownSAECipher

**A toy shift-and-XOR bit cipher, written as a target for a unit-test plan.**

![Python](https://img.shields.io/badge/Python-3-38E1FF?style=flat-square&labelColor=0A1428&logo=python&logoColor=38E1FF)
![pytest](https://img.shields.io/badge/tests-pytest-38E1FF?style=flat-square&labelColor=0A1428)

</div>

---

## Overview

This is the author's implementation of a **Unit Test Plan** for an encryption module.
The module is a home-made scheme built from nothing but bit rotations and XOR
(original note: *"I make some encryption scheme with just shift and xor kind of bits"*).

It is a learning exercise. Shifts and XOR with a repeating key are linear operations, so this is a toy, not a secure cipher.

## How the cipher works

`OWNSAE(XorKey, ShiftKey1, ShiftKey2)` works on the message as a string of bits, using 17 rounds:

1. The XOR key is repeated to the length of the message and XORed in.
2. Then, 16 more times:
   - rotate the message bits by `ShiftKey1`;
   - rotate the key bits by `ShiftKey2`;
   - XOR the rotated key into the message.
3. The result is returned as a hex string.

`decrypt` rebuilds the 17 key states, then undoes the steps in reverse order (XOR, then rotate back by `ShiftKey1`).

The CLI in `main.py` uses `XorKey = b'keykeykeykeykey'`, `ShiftKey1 = 17` and `ShiftKey2 = 91`.

## Files

| File | Description |
| --- | --- |
| `main.py` | The `OWNSAE` class (bit helpers, `encrypt`, `decrypt`) plus an interactive menu to encrypt or decrypt |
| `test.py` | pytest suite: 22 test cases covering each method with a valid input, an invalid input and an empty input |
| `report.txt` | Saved pytest output from the author's run (Python 3.10.6, pytest 6.2.5) |

## Test plan and results

Each helper (`BitsPad8`, `BytesToBits`, `BitsToBytes`, `xorBits`, `shifterRight`, `shifterLeft`) gets three tests: a normal case, an invalid input and an empty input.
`encrypt` and `decrypt` get a known-answer test (`"testing"` ↔ `e742e6cf68a158`) and an invalid-input test.

The invalid-input tests expect the message `"Input tidak valid"` ("invalid input"). `main.py` has no input validation, so those cases fail.
The saved `report.txt` records **14 passed, 8 failed**, and all 8 failures are the invalid-input cases.

## Requirements

- Python 3 (standard library only for `main.py`)
- [`pytest`](https://pypi.org/project/pytest/) to run the tests

## Usage

Interactive encrypt/decrypt menu:

```bash
python main.py
```

Run the test plan:

```bash
pip install pytest
pytest test.py
```

Use the class directly:

```python
from main import OWNSAE

own = OWNSAE(b'keykeykeykeykey', 17, 91)
ct = own.encrypt(b"testing")   # 'e742e6cf68a158'
pt = own.decrypt(ct)           # 'testing'
```

---

<div align="center">

Part of [wondping0](https://github.com/wondping0)'s cryptography notebooks · [wondping0.github.io](https://wondping0.github.io)

</div>
