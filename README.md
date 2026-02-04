README.txt
===========

Title:
Enhance LLM Math Reasoning with Reinforcement Learning

Author:
Ji Dung Lo
Department of Computer Science and Engineering
Santa Clara University
2026

------------------------------------------------------------
Overview
------------------------------------------------------------
This thesis investigates how reinforcement learning (RL) can be used to improve
step-by-step mathematical reasoning in large language models (LLMs) without relying
on human-annotated chains of thought.

The core challenge addressed in this work is the sparsity of outcome-only reward
signals in reasoning tasks, where models receive feedback only based on final-answer
correctness. Such sparse supervision makes credit assignment difficult and often
leads to brittle or shortcut reasoning strategies.

To address this issue, this thesis proposes a quiz-augmented reinforcement learning
framework that provides denser, automated intermediate feedback while remaining
fully scalable and annotation-free.

------------------------------------------------------------
Key Ideas
------------------------------------------------------------
1. Treat mathematical reasoning as a long-horizon decision-making problem, where
   intermediate steps critically affect final outcomes.

2. Increase reward density by introducing automatically generated intermediate
   quizzes that probe partial reasoning states.

3. Avoid human-annotated process supervision by using programmatic, task-aligned
   reward signals.

4. Focus reinforcement learning on weak reasoning segments through chunk-level
   saliency estimation and targeted optimization.

------------------------------------------------------------
Methodology Summary
------------------------------------------------------------
The training pipeline consists of two stages:

1. Supervised Fine-Tuning (SFT)
   - Models are initialized via parameter-efficient fine-tuning (LoRA) on MetaMathQA,
     a synthetic dataset derived from GSM8K and MATH.
   - This stage provides a strong initialization with structured reasoning outputs.

2. Reinforcement Learning Post-Training
   - Reinforcement learning is applied using Group Relative Policy Optimization (GRPO).
   - The reward function combines:
       (a) Step bonus: encourages explicit reasoning structure.
       (b) Exam reward: evaluates final-answer correctness using EM/F1 metrics.
       (c) Quiz reward: evaluates intermediate reasoning via automatically generated quizzes.
   - Rewards are fully automated and computed after full sequence generation.

------------------------------------------------------------
Automatic Reward Design
------------------------------------------------------------
The total reward for a generated solution is defined as:

    R(q, o) = R_step + R_exam + λ_quiz · clip(R_quiz, −C, C)

This design balances final-answer accuracy with intermediate reasoning quality,
while preventing reward hacking and maintaining training stability.

------------------------------------------------------------
Experimental Setup
------------------------------------------------------------
- Models: Qwen2.5-1.5B, Qwen2.5-3B, Qwen2.5-7B
- Datasets: GSM8K, MATH (training splits), MetaMathQA (SFT initialization)
- Hardware: Single NVIDIA A100 SXM 80GB GPU
- All experiments use parameter-efficient adaptation due to hardware constraints.

------------------------------------------------------------
Evaluation Benchmarks
------------------------------------------------------------
Models are evaluated on:
- GSM8K
- MATH
- MMLU
- AIME 2025

Metrics include Pass@1, Pass@k, Exact Match (EM), F1, and Consensus@64 where applicable.

------------------------------------------------------------
Main Findings
------------------------------------------------------------
- Quiz-augmented reinforcement learning consistently improves mathematical reasoning
  accuracy compared to supervised fine-tuning alone.
- Denser, step-level reward signals improve credit assignment and reasoning robustness.
- Simple, well-aligned reward designs are sufficient; overly complex reward engineering
  provides diminishing returns.
- Meaningful reasoning improvements are achievable under realistic hardware constraints.

------------------------------------------------------------
Intended Audience
------------------------------------------------------------
This thesis is intended for researchers and practitioners interested in:
- Reinforcement learning for reasoning
- Large language model post-training
- Automated alternatives to process supervision
- Math and long-horizon reasoning benchmarks

------------------------------------------------------------
Notes
------------------------------------------------------------
This repository contains the LaTeX source for the thesis, figures illustrating the
reward structure and training pipeline, and appendices with prompt templates used
for solution generation and quiz-based supervision.

