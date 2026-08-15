# Binary Digits — Forensics, Easy (picoCTF 2026)

- **Competition:** picoCTF 2026
- **Category:** Forensics
- **Difficulty:** Easy
- **Author:** Yahaya Meddy

## Statement

This file doesn't look like much... just a bunch of 1s and 0s. But maybe it's not just random noise. Can you recover anything meaningful from this?

## Thought Process

The challenge gives us a `digits.bin` file.

1. Seems simple enough. We can decode binary into text to find meaning.
2. First we decode this raw binary into `ascii/utf-8` in CyberChef. We got a really long string of 1s and 0s.
3. Let's try decoding that string again. We get a sample of the raw bytes:

```text
c3 bf c3 98 c3 bf c3 a0 00 10 4a 46 49 46
... (bytes not captured when the note was rewritten) ...
0b c3 bf c3 84 00 c2 b5 10 00 02 01 03 03 02 04
03 05 05 04 04 00 00 01 7d 01 02 03 00 04 11 05
12 21 31 41 06 13 51 61 07 22 71 14 32 c2 81 c2
91 c2 a1 08 23 42 c2 b1 c3 81 15 52 c3 91 c3 b0
24 33 62 72 c2 82 09 0a 16 17 18 19 1a 25 26 27
28 29 2a 34 35 36 37 38 39 3a 43 44 45 46 47 48
49 4a
```

The sample starts with `FF D8 FF E0` (the `c3 bf c3 98 c3 bf c3 a0` bytes) followed by the text `JFIF` — that means it's a JPG.
4. So we use [CyberChef](https://gchq.github.io/CyberChef) to convert from binary to a JPEG image.

![Binary_Digits-flag_image](Binary_Digits-flag_image.png)

Boom, flag in the image.

## Flag

```
picoCTF{h1d3n_1n_th3_b1n4ry_3d2e65ba}
```