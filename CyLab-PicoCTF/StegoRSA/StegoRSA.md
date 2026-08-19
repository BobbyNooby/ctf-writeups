# StegoRSA: Cryptography, Easy (picoCTF 2026)

- **Competition:** picoCTF 2026
- **Category:** Cryptography
- **Difficulty:** Easy
- **Author:** Yahaya Meddy

## Statement

A message has been encrypted using RSA. The public key is gone... but someone might have been careless with the private key. Can you recover it and decrypt the message?

## Thought Process

1. I got a `flag.enc` and a `image.jpg`. I'm assuming this is an attempt to decrypt RSA with the image as the key.
2. Let's use `exiftool` on the image:

```
ExifTool Version Number         : 13.55
File Name                       : image.jpg
Directory                       : .
File Size                       : 21 kB
File Modification Date/Time     : 2026:08:19 11:41:56+01:00
File Access Date/Time           : 2026:08:19 11:42:01+01:00
File Inode Change Date/Time     : 2026:08:19 11:42:00+01:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Comment                         : 2d2d2d2d2d424547494e2050524956415445204b45592d2d2d2d2d0a4d49494576514942... (long hex string, truncated here)
Image Width                     : 512
Image Height                    : 512
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 512x512
Megapixels                      : 0.262
```

The `Comment` field is one long hex string.
3. That looks like hex. Let's do hex to ascii. It returns a PEM private key:

```
-----BEGIN PRIVATE KEY-----
MIIEvQIBADANBgkqhkiG9w0BAQEFAASCBKcwggSjAgEAAoIBAQCtVf6CDwFYrEnH
k4R/8NMG0Sb8m6mVUy4y1LAX8nbYlJI6llRp0gvi1wRbsoDdmZ48oCFBd26rlc01
EXA3xrN/Fw4gcITyxtqNsgq2r2x2dAOuZW6MxI1948iLDE4VAR+H2U8iJiQq5z3Y
51tjaS+JlW/rvu/W3rdDbrkSXgju/wmmMNmIR6z7kzn5l/twq3mXJxkOZZLzs7Bt
hzrDlH/JgUhWr0gxfR2wdvwQft6QJ82iXadqzH4C5ZEVVO350aJUta7QPao4R+11
ZWjtln5mssQUEAaM6DjYbEM66Uy3iplESy2Nsx0Qnk+LmogEQcRhXSq00HgOeBMO
Z0VHZPLHAgMBAAECggEACG3dIY//PcOrFtR6pgodCQDUx4X+Wi+gWIJ1ScTVuLSI
4+Z5lmfLgi14ncjxcVVOF56l31wiep+fSgxeC6hTBEQnwLYYEQJQkIFu+fFP8fa0
Ux/Fn3zTcKLKFtDzXxwd32pW6c83BQsXu9uMWyo7UJJ+zdUMLsPH37SbtWPzRUP3
CBhTRrE7fpeKuCptCVUiq61lXfmNX4kBwYcAyC4wsA27kWlntzdLEnpxnVaAfOt1
jv/xxcf2x9Ua0y034/WiLJ09JViQDmm6yDBsJggOlKKV/D0aLelvOpHMtk1xXO3f
rLWJi54WrRBJJMBk0R3XIEWSaGScxQVPDvEPeHN6QQKBgQC1ETs0f4jxPXDMr9pg
iNs8qk7UqMxKECUKmZ5os3ne4EU06Mif+lNRNNZmNxOnFTKF860ByLVP0PQVZ7Xf
I7q/orc2OlFQS0P972j0WJRzG2PCSU+dKHwwhgKlZ8k1hA8VOI4WW6dbo4iZehJp
U3Zqt0gOvf39R875GcrvcNuN5wKBgQD1Ea17V+fs/zIlVtJ5RKgnp1tvvkkXUg5D
TgZbFbqmMWoNjcrvLI86Tq700ALjBgHaUkY7n9zRg84WNBWIrVJlhufrKhGCkqpb
KsyiVOQdEi7oLpcXIxFAoV79pVOv3x7ubnFSi1Ff+mHDeuBu5UZkBHlNSY6CrW6J
gjdpjqYYIQKBgFhMxuqbJ1U9+TxYpc5d70xuYXMjvjyAExBQSggVPmGKTTW4L96U
XP1FHylJwrPAipr4cm5kSsdZxy6JHRBshC3gVCiF2BGoIsg7cJt4dyyLNuMQjVq+
25FuSOwQ6PbIJ/LZWbFdkQgHgB4YgdILebwhFWrbDHnwAudHxMdv6iIRAoGALckD
tEuUFP8Ii1lRMT7We7IUryfJ2AWIjKKDJXlFyc7plWasR0r351jT7wD9yRRSPEuq
u3D+fFY3poZMj6ByCG3P3muZod9s3GN+n8VkaNoA0XgC2lu+2WhMqu68V9tDmCAi
I93LcjcBFNhcHdvP7te3Ie1gJqHoSOB/IcV42oECgYEAj7DgOtLsNeDJOZhVwG0H
gzPgcC2eY2Q5ugW5vObtrEXJrEyo188E1tEuDeLnOJZUbsDKdGcRTRjEAazfChfE
XG0xDG9TSv9sq3hz9IvESH4Tp8ccfoK8+ulqTCCvz5g6l90B2qOpF7/6J8DuMNxn
N/OwBNoNPzMla2CJmbCsbGc=
-----END PRIVATE KEY-----
```

4. Now let's use that to decode the flag. First, save the key and check if it's valid:

```bash
nano key.pem
openssl rsa -in key.pem -check -noout
```

5. Then we try to decrypt:

```bash
openssl pkeyutl -decrypt -inkey key.pem -in flags.enc -out flag.txt
cat flag.txt
```

Boom, `flag.txt` is the flag.

## Flag

```
picoCTF{rs4_k3y_1n_1mg_51611ab8}
```