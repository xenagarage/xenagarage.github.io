---
layout: post
title: My H200 run was only 2.7× faster than my laptop
subtitle: When a big GPU gives a small speedup, the GPU is mostly waiting.
tags: [gpu, lora, unsloth]
---

My LoRA adapter trained in 40 minutes on my laptop and 15 minutes on an H200. That sounds like a win, until you remember an H200 should be **10× faster or more**. A 2.7× speedup means the GPU spent most of the run waiting.

## Where the time actually went

1. **Downloading the model.** A fresh cloud machine can spend 5+ minutes on it, and it counts against your "training time."
2. **Padding.** Without sequence packing, short examples get padded and the GPU does real math on padding tokens.
3. **Tiny batches.** A batch of 4 on a 141 GB GPU leaves most of it idle.
4. **4-bit quantization I didn't need.** QLoRA saves memory, but dequantizing costs speed. On a big GPU, plain bf16 LoRA is faster.

## The fixes

- **Unsloth**: fused kernels and chunked cross-entropy (a big deal with Gemma's ~262k vocabulary), about 2× faster
- `packing=True`
- a bigger `per_device_train_batch_size` with less gradient accumulation
- `max_length` capped at what the data needs
- a persistent volume, so the model downloads once
- a 10–20% data subset for experiments

## The numbers

| run | train time | tokens/s | peak memory |
|---|---|---|---|
| baseline | | | |
| + unsloth | | | |
| + packing | | | |
| + big batch | | | |

*(fill in from LABS.md, Labs 2–3)*

Code: [llm-train-serve-lab](https://github.com/xenagarage/llm-train-serve-lab) *(private for now)*
