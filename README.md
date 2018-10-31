# CS255 Course Notes
These notes accompany: https://www.coursera.org/learn/crypto/

# Week 1

## Stream Ciphers

### One Time Pad
Def: a cipher defined over (K, M, C), pair of “efficient” algs (E,D) where for all m in M, k in K: `D(k, E(k,m)) = m`
* has perfect secrecy, is semantically secure
* efficient = runs in polynomial time
* fast encoding, but long text (1:1 as plaintext)

### Shannon’s Perfect Secrecy 
Def: a cipher (E,D) over (K, M, C) has *perfect secrecy* if, for all m0, m1 in M (equal length) and c in C: `Pr[E(k, m0)=c] = Pr[E(k, m1)=c]` where k is uniform in K.
* If i’m adversary and obtain c, without knowing k, ANY m should have yielded c, thus CT reveals no info about PT

### Pseudo Random Generators
Def: a function G: {0,1}^s in {0,1}n, where n >> s , that G(shorter-k) can be XORd with m 
* no perfect secrecy, but is unpredictable

### Predictability & Unpredictability 
Def: Predictability is G where there exists “eff” algo A, and i, s.t. `Pr[A(G(k)) | 1,…, i   = G(k) | i+1 ] >= 1/2 + non-negligible e`
Def: Unpredictability means
* from G(k)| indeces i…j , no A can infer bit i+1 for non-neg e

### Negligibility
* in practice, non-neg: `e>= 2^-30` (1GB)
* in practice, `neg: e<= 2^-80` (lifetime)
* in theory, non-neg: e(L) >= L^-d (for large L)
* in theory, neg: e(L) <= L^-d (for large L)
e.g. 2^-L is negligible bc 2^-L is smaller than (largeL)^const

### Stream Cipher Vulnerabilities
* Never use k more than once (both OTP and PRG)
* Disk encryption: don’t use stream cipher (bc hardware semantics easy to guess)
* Malleable, i.e. tampering with CT are undetected. Receiver can unknowingly get wrong msg.

## Secure Ciphers

### Advantage
Def: `ADVprg[A, G] := | Pr[A(G(k))=1] - Pr[A(r)=1] |` where k subset of K, r is truly random
* i.e. When A can distinguish when G is being used
* If ADV close to one, then A can distinguish G from rand
* Statistical test: any algo A s.t. A(x) outputs 0 (not random) or 1 (random)

### Secure PRG
Def: if there exists A s.t. ADVprg[A, G] is negligible
* An unpreditable PRG is secure
* Secure PRG is also unpredictable

### Computational indistinguishable
Def: P1 and P2 are two distr over (0,1), iff exists A s.t. `Pr[A(p0)=1] - Pr[A(p2)=1] is neg`

### Semantic Security
Def: E is sem-secure for all A, iff ADVss[A, E] is negligible
* an adversary running in a reasonable amount of time can (or cannot) distinguish one message from another once encrypted
* impt esp if input is binary, e.g. voting

### Stream Ciphers are Sem-secure
Def: `ADVss[A, E] <= 2 * ADVprg[B, G]`
