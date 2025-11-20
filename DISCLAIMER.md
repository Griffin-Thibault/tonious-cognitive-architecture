# DISCLAIMER

This repository describes **a cognitive architecture**, not a model.

Tonious is **not** a trained model, not a fine-tune, and not a new dataset.  
It is a **memory and reasoning framework** that wraps around any existing LLM  
to guide its behavior in a structured, predictable way.

## What Tonious Actually Provides

- A **Tree-of-Life (TOL)**–inspired memory graph that stores interactions,
  video moments, summaries, and symbolic representations.
- A **control layer** that routes user input through predictable stages
  (General Mode, Video Mode, Recall Mode).
- A **separate memory system** external to the model, which serves to
  guide the LLM the same way human cognition is guided by:
  1. **Perception** (video → Trinity → moments)  
  2. **Short-term reasoning** (LLM prompt-level thinking)  
  3. **Long-term memory** (TOL graph)  
- A method for **replicating the human input → interpretation → memory → output** loop using
  text, structured logs, and symbolic nodes.

## What Tonious Is NOT

- ❌ A new LLM  
- ❌ A fine-tuned or trained model  
- ❌ A dataset for training  
- ❌ A “general intelligence” system  
- ❌ A guarantee of factual accuracy  

Tonious does **not** improve intelligence through weights.  
It improves **behavior** through structure, context routing, and persistent memory.

## Intended Use

This architecture is designed so that **any developer** can:
- Plug in their own small or large model.
- Provide it with structured perception (via Trinity moments).
- Give it a stable memory system (TOL).
- Achieve more coherent reasoning and recall **without** training.

## No Guarantees

This project is experimental and provided *as-is*.  
It does not guarantee:
- perfect accuracy,  
- perfect recall,  
- or any specific model behavior.

Actual performance depends on the underlying LLM and hardware.

## Summary (in plain English)

Tonious is a *mind scaffold* for LLMs:  
a structured memory + perception + reasoning pipeline that helps a model  
act more like a system and less like a single isolated predictor.

It is architecture, not intelligence.


