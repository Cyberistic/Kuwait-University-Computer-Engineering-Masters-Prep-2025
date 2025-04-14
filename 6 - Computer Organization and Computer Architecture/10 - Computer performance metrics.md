
## Basic Performance Measures

### Clock Speed (Frequency)

- Measured in Hz (cycles per second)
- Higher frequency = more instructions executed per second
- Modern CPUs: 2-5 GHz typical

### Execution Time

- Time to complete a specific task
- Can be divided into:
  - CPU Time: Time spent executing instructions
  - I/O Time: Time waiting for input/output

### MIPS (Million Instructions Per Second)

$$
MIPS = \frac{\text{Instruction Count}}{\text{Execution Time} \times 10^6}
$$

### CPI (Cycles Per Instruction)

$$
CPI = \frac{\text{Clock Cycles}}{\text{Instruction Count}}
$$

## Amdahl's Law

Amdahl's Law predicts the theoretical speedup when improving part of a system.

$$
Speedup = \frac{1}{(1-p) + \frac{p}{s}}
$$

Where:

- p = proportion of execution time that can be improved
- s = speedup factor of the improvement

### Examples

1. If 40% of a program can be parallelized with 4 cores:

   $$
   \begin{align*}
   Speedup &= \frac{1}{(1-0.4) + \frac{0.4}{4}} \\
   &= \frac{1}{0.6 + 0.1} \\
   &= \frac{1}{0.7} \\
   &\approx 1.43\times \text{ faster}
   \end{align*}
   $$

2. Making a part 10× faster that takes 80% of execution time:
   $$
   \begin{align*}
   Speedup &= \frac{1}{(1-0.8) + \frac{0.8}{10}} \\
   &= \frac{1}{0.2 + 0.08} \\
   &= \frac{1}{0.28} \\
   &\approx 3.57\times \text{ faster}
   \end{align*}
   $$

> [!tip] Key Insight
> Amdahl's Law shows why it's hard to get large speedups by improving only part of a system.
> Even making 80% of a program infinitely fast (s → ∞) only gives a 5× speedup:
> $$\lim_{s \to \infty} \frac{1}{0.2 + \frac{0.8}{s}} = \frac{1}{0.2} = 5$$

## Performance Comparison

### Relative Performance

$$
\text{Performance Ratio} = \frac{\text{Execution Time}_A}{\text{Execution Time}_B}
$$

### Example

If Computer A takes 100 seconds and Computer B takes 150 seconds:

$$
\text{Performance Ratio} = \frac{150}{100} = 1.5
$$

Therefore, Computer A is 1.5 times faster than Computer B.

## Response Time vs Throughput

1. **Response Time**

   - Time from start to completion
   - Important for individual tasks
   - Measured in time units

2. **Throughput**
   - Tasks completed per unit time
   - Important for servers/batch processing
   - Measured in tasks/second

> [!note] Remember
> Improving response time doesn't always improve throughput and vice versa.
