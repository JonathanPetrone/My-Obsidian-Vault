#### Q: bcrypt cost factor: 12-14 is current best practice (start with 12) can you explain this?
##### A: The Cost Factor

The cost factor is an exponent that determines the number of iterations bcrypt performs:

- **Cost of 12** = 2^12 = 4,096 iterations
- **Cost of 14** = 2^14 = 16,384 iterations

Each increment doubles the computation time and memory usage.