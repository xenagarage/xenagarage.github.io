---
layout: post
title: Hello from the garage
subtitle: The lights are on, and there's a GPU bill to keep small.
tags: [meta]
---

This is a notebook for things I'm building and figuring out, written down so the second time is faster than the first.

Right now that means **fine-tuning LoRA adapters for Kaggle Gemma hackathons on a student budget**, and then learning what happens when you have to *serve* a model to more than one person at a time.

Expect posts like:

- squeezing free GPU hours out of Kaggle and Colab
- why a run on a big GPU wasn't as fast as it should have been
- what "4B" and "bf16" actually mean for your memory budget
- going from `model.generate()` in Flask to a real inference engine

Short, practical, with numbers. That's the plan.
