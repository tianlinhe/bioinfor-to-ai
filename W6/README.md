# Transformer Basics
## Clarification about transformer & ESM-2

### 1. The Transformer is the "Skeleton" (Architecture)
"Transformer" is not a specific model; it is a design blueprint.

* The blueprint specifies that there should be cylinders, a crankshaft, and spark plugs arranged in a specific way.
* In AI, the "Transformer blueprint" specifies there should be Self-Attention layers, Feed-Forward layers, and Layer Normalization.   

### 2. ESM-2 is the "Specific Engine" (The Weights)
ESM-2 is a specific instance of that blueprint.

* Meta took the "Transformer blueprint" and built an engine with 8 million parts (parameters).
* They then "fueled" it by training it on millions of protein sequences until those 8 million parameters learned the patterns of biology.

### 3. Why we call it a "Transformer Module"
When you use the code from transformers import EsmModel, you are doing two things:

* Loading the Blueprint: You are telling PyTorch to build a "Transformer-style" container.
* Loading the Brain (ESM-2): You are pouring those 8 million pre-trained numbers (the parameters) into that container.

## Transformer Basics and analogy in biology
A Transformer uses Self-Attention to create a link between every single base, remember these three roles:
* The Query ($Q$): "What am I looking for?" (Current base)
* The Key ($K$): "What information do I contain?" (Other bases)
* The Value ($V$): "If I am relevant, what information do I contribute?"The Transformer calculates a score by comparing $Q$ and $K$. If they "match" biologically, it takes the information from $V$.

## Multi-head attention explained in 3Blue1Brown video

If a single "Attention Head" is a specialist looking for one specific relationship (like an adjective modifying a noun), Multi-Head Attention is a panel of experts working in parallel to understand the full complexity of a sequence.

* Parallel Processing: Instead of one large calculation, the model runs many smaller attention mechanisms (heads) simultaneously. For example, GPT-3 uses 96 heads per layer [21:00].
* Diverse Perspectives: Each head has its own set of tunable weights (Query, Key, and Value matrices). This allows different heads to "specialize" in different patterns [22:04]:
  * Head 1 might focus on Grammar (e.g., matching verbs to subjects).
  * Head 2 might focus on Structural Logic (e.g., identifying if the text is a poem or a lab report).
  * Head 3 might focus on Long-range References (e.g., connecting a protein's start codon to its distant stop codon).
* After each head produces its own suggested update for a word's vector, these changes are summed together and added back to the original embedding. This creates a single, highly nuanced "contextual" vector that combines all the insights from every head [21:29].

When applying this to ESM-2, multi-head attention is what allows the model to see a protein from multiple angles at once. One head might be noticing local secondary structures (alpha helices), while another head is identifying distant 3D contacts (disulfide bonds). By having multiple heads, the model doesn't have to choose between looking "locally" or "globally"—it does both.