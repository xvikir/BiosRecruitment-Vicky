# CryptoHack — Python Basics Write-ups

Solutions to the CryptoHack introductory challenges covering Python fundamentals and encoding/XOR concepts.

---

## Task 2 — XOR Decoding (Python Zen)

**Concept:** XOR each byte of an integer array with a fixed key (`0x32`) to recover the plaintext.

```python
import sys
# import this

if sys.version_info.major == 2:
    print("You are running Python 2, which is no longer supported. Please update to Python 3.")

ords = [81, 64, 75, 66, 70, 93, 73, 72, 1, 92, 109, 2, 84, 109, 66, 75, 70, 90, 2, 92, 79]

print("Here is your flag:")
print("".join(chr(o ^ 0x32) for o in ords))
```

**Flag:** `crypto{z3n_0f_pyth0n}`

---

## Task 3 — ASCII Conversion

**Concept:** Convert a list of decimal ASCII values into their corresponding characters using `chr()`.

```python
nums = [99,114,121,112,116,111,123,65,83,67,73,73,95,112,114,49,110,116,52,98,108,51,125]

result = ""

for n in nums:
    result = result + chr(n)

print(result)
```

> **Extra:**
```python
Result ="crypto{ASCII_pr1nt4bl3}"
nums = []
for ch in Result:
    nums.append(ord(ch))
print(nums)
```

**Flag:** `crypto{ASCII_pr1nt4bl3}`

> **Extra:** Reversed the operation using `ord()` to convert the flag string back into its ASCII integer list — confirming the encoding is bijective.

---

## Task 4 — Hex Decoding

**Concept:** Decode a hex string to bytes using `bytes.fromhex()`, then decode to UTF-8.

```python
hex_string = "63727970746f7b596f755f77696c6c5f62655f776f726b696e675f776974685f6865785f737472696e67735f615f6c6f747d"

decoded_bytes = bytes.fromhex(hex_string)

print(decoded_bytes)

print(decoded_bytes.decode())
```

> **Extra:**
```python
decoded_bytes = "crypto{You_will_be_working_with_hex_strings_a_lot}".encode()

hex_string = decoded_bytes.hex()

print(hex_string)
```

**Flag:** `crypto{You_will_be_working_with_hex_strings_a_lot}`

> **Extra:** Re-encoded the flag back to hex using `.encode().hex()` to verify round-trip consistency.

---

## Task 5 — Base64 Encoding

**Concept:** Convert a hex string to bytes, then encode those bytes as Base64.

```python
import base64
hex_string = "72bca9b68fc16ac7beeb8f849dca1d8a783e8acf9679bf9269f7bf"
decoded_bytes = bytes.fromhex(hex_string)
base_64string = base64.b64encode(decoded_bytes)
print(base_64string.decode())
```

**Flag:** `crypto/Base+64+Encoding+is+Web+Safe/`

---

## Task 6 — Integer to Bytes

**Concept:** Use `long_to_bytes()` from `pycryptodome` to convert a large integer directly into its byte/string representation.

```python
from Crypto.Util.number import long_to_bytes

num = 11515195063862318899931685488813747395775516287289682636499965282714637259206269

text = long_to_bytes(num)

print(text)
```

**Flag:** `crypto{3nc0d1n6_4ll_7h3_w4y_d0wn}`

---

## Task 7 — Single-Byte XOR

**Concept:** XOR each character in a string against a fixed value (`13`) to produce the output — demonstrating that XOR is its own inverse.

```python
text = "label"
result = ""

for ch in text:
    new_char = chr(ord(ch) ^ 13)
    result += new_char

print(result)
```

**Output:** `aloha`

---

## Task 8 — XOR Associativity

**Concept:** Recover chained XOR keys using the associativity property of XOR. Given `KEY1`, `K2⊕K1`, and `K2⊕K3`, recover `KEY2` and `KEY3`, then XOR all three against the encrypted flag.

```python
# Step 1: Convert hex strings to bytes
KEY1 = bytes.fromhex("a6c8b6733c9b22de7bc0253266a3867df55acde8635e19c73313")
K2_XOR_K1 = bytes.fromhex("37dcb292030faa90d07eec17e3b1c6d8daf94c35d4c9191a5e1e")
K2_XOR_K3 = bytes.fromhex("c1545756687e7573db23aa1c3452a098b71a7fbf0fddddde5fc1")
FLAG_XOR_ALL = bytes.fromhex("04ee9855208a2cd59091d04767ae47963170d1660df7f56f5faf")

# Step 2: Recover KEY2
KEY2 = bytes(a ^ b for a, b in zip(K2_XOR_K1, KEY1))

# Step 3: Recover KEY3
KEY3 = bytes(a ^ b for a, b in zip(K2_XOR_K3, KEY2))

# Step 4: Recover the FLAG
FLAG = bytes(a ^ b ^ c ^ d for a, b, c, d in zip(FLAG_XOR_ALL, KEY1, KEY2, KEY3))

# Step 5: Print the flag
print(FLAG.decode())
```

**Flag:** `crypto{x0r_i5_ass0c1at1v3}`

---

## Task 9 — Brute-Force Single-Byte XOR

**Concept:** Brute-force all 256 possible single-byte XOR keys against a hex-encoded ciphertext. Visually inspect results for a readable flag.

```python
F1 = "73626960647f6b206821204f21254f7d694f7624662065622127234f726927756d"
key1 = bytes.fromhex(F1)
print(key1.decode())

for key in range(256):

    result = ""

    for byte in key1:
        result += chr(byte ^ key)

    print("Key:", key, "->", result)
```

**Flag (Key = 16):** `crypto{0x10_15_my_f4v0ur173_by7e}`

---

## Task 10 — Repeating-Key XOR (Vigenère-style)

**Concept:** Use a known plaintext attack — since CryptoHack flags always start with `crypto{`, XOR the ciphertext prefix against that known prefix to recover the key, then decrypt the full message with the repeating key.

```python
from itertools import cycle

# given ciphertext (hex string)
cipher_hex = "0e0b213f26041e480b26217f27342e175d0e070a3c5b103e2526217f27342e175d0e077e263451150104"

# convert hex → bytes
cipher = bytes.fromhex(cipher_hex)

# known plaintext prefix from CryptoHack flag format
known_plain = b"crypto{"

# recover first part of the key
key_part = bytes([cipher[i] ^ known_plain[i] for i in range(len(known_plain))])

print("Recovered key part:", key_part)

# full key (repeating XOR key)
key = b"myXORkey"

# decrypt ciphertext using repeating key
plaintext = bytes([c ^ k for c, k in zip(cipher, cycle(key))])

print("Decrypted message:", plaintext.decode())
```

**Flag:** `crypto{1f_y0u_Kn0w_En0uGH_y0u_Kn0w_1t_4ll}`

---

## Summary

| Task | Topic | Flag |
|------|-------|------|
| 2 | XOR with fixed key | `crypto{z3n_0f_pyth0n}` |
| 3 | ASCII → char | `crypto{ASCII_pr1nt4bl3}` |
| 4 | Hex decoding | `crypto{You_will_be_working_with_hex_strings_a_lot}` |
| 5 | Base64 encoding | `crypto/Base+64+Encoding+is+Web+Safe/` |
| 6 | Integer to bytes | `crypto{3nc0d1n6_4ll_7h3_w4y_d0wn}` |
| 7 | Single-byte XOR | `aloha` |
| 8 | XOR associativity | `crypto{x0r_i5_ass0c1at1v3}` |
| 9 | Brute-force XOR | `crypto{0x10_15_my_f4v0ur173_by7e}` |
| 10 | Repeating-key XOR | `crypto{1f_y0u_Kn0w_En0uGH_y0u_Kn0w_1t_4ll}` |
