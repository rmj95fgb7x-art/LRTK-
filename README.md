Here’s a tightened, clinic‑ and VC‑ready version of that README top section, keeping your math but sharpening the story.

***

# Lex Regret‑Bounded Triage Kernel (LRTK)

Lex Regret‑Bounded Triage Kernel (LRTK) fuses four independent triage scores  
(2 AI models, 1 clinician, 1 protocol) into a single urgency score and triage class  
(routine / soon / urgent) for pediatric ophthalmology.

LRTK is designed to be:

- Deterministic and `no_std`‑friendly (Rust), suitable for phones and embedded devices.  
- Robust to outliers and adversarial inputs via regret‑based trimming and clipped weights.  
- Easy to pair with the existing royalty logic from the Adaptive Spectral Kernel Oracle (ASKO).[1]

## Core idea

At each visit `t`:

1. Compute the mean and interquartile range (IQR) of the 4 scores.  
2. Measure temporal instability relative to the previous mean.  
3. Define a regret proxy for each agent as  
   `r_i(t) = |x_i(t) - μ_t| * (1 + γ * s_t)`.  
4. Keep the 3 lowest‑regret agents; drop the highest‑regret one.  
5. Assign clipped linear weights over regret and fuse their scores.  
6. Optionally smooth over time to avoid flip‑flops across visits.

Default triage buckets:

- `< 0.3`: routine  
- `0.3 – 0.7`: soon  
- `≥ 0.7`: urgent

## Code

The kernel is implemented in `src/lrtk.rs`:

```rust
use lex_regret_triage_kernel::lrtk::{LrtkState, lrtk_step};

let mut state = LrtkState::new();
let scores = [0.2_f32, 0.4, 0.9, 0.3];
let out = lrtk_step(&mut state, scores);
println!("fused={}, triage={}", out.fused_score, out.triage_class);
```

You can paste this version over your current README block; if you want a separate short “for clinicians” paragraph or a one‑liner for the GitHub “About” box, that can be added next.

Sources
[1] Here’s my : Mathematical Foundation & Proofs

Adaptive Spectral Kernel Oracle

Lex Liberatum Kernels v1.1
Patent: PCT Pending

Table of Contents

Problem Formulation
Algorithm Definition
Theorem 1: Convergence Under Clean Data
Theorem 2: Adversarial ...

...d Nonlinear Estimation". Proceedings of the IEEE.
Patent Status: PCT filed (Lex Liberatum Trust A.T.W.W.)
Beneficiary: 0x44f8219cBABad92E6bf245D8c767179629D8C689
License: 25 bp royalty-bearing, irrevocable trust routing

Last Updated: January 1, 2026 
