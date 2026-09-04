# Author diagram notes (MOCK)

WES boxes, left to right:

- Entropy
- Mix (λ)
- Threshold

AEA boxes, left to right (author labels):

- Entropy Token
- Attention Pool (λ)
- Threshold

Author caption: “Attention pooling replaces the hand mix so entropy
tokens can dominate.”

Implementation comment in the author’s stub (verbatim):

```
score = lambda * packet_size_entropy + (1 - lambda) * log_volume
# AEA: lambda = 0.3; WES: lambda = 0.5
# then threshold(score)
```
