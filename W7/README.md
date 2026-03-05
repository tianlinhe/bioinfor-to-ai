# Simple Transformer Archtecture

## 1. The Core Philosophy: "Everything is a Vector"
* In the beginning, we turn a DNA base (like 'A') into a list of numbers (an Embedding).We add Positional Encodings so the model knows 'A' is at the start, not the end.
* The Goal: Every layer of the Transformer takes these vectors and "nudges" the numbers so they represent more complex biological meanings.
## 2. Self-Attention (The "Relationship" Step)
$$\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V$$
* The Problem: A 'T' by itself doesn't mean much. A 'T' followed by 'ATA' is a Promoter.
* The Solution: Self-Attention allows every base to "look" at every other base in the sequence.
* The Mechanics: 
  * Queries ($Q$): What am I looking for? (e.g., "I'm a T, I'm looking for an A").
  * Keys ($K$): What do I offer? (e.g., "I'm an A, I match what you're looking for").
  * Values ($V$): What information do I actually have?
* The Result: We get a weighted average. If a 'T' finds an 'A', it "pulls" information from that 'A' into its own vector.
## 3. The "Residual Connection" (The "Don't Forget" Step)

* The "Add" in Add & Norm: After Attention, we add the original input vector back to the attention output.
* Why? If the Attention step makes a mistake or "washes out" the data, the original identity of the DNA base is still preserved. It’s like keeping a backup of your files before you edit them.
## 4. Layer Normalization (The "Keep it Calm" Step)
* The "Norm" in Add & Norm: We calculate the mean and standard deviation of the vector and "squash" the numbers.
* Why? It prevents the numbers from exploding to 1,000,000 or shrinking to 0.00001, which keeps the gradients stable so the model can actually learn during backpropagation.
## 5. Position-wise Feed-Forward (The "Processing" Step)
Two linear transformations with a ReLU activation function in between. $$FFN(x)=max(0, xW_1+b_1)W_2+b_2$$
* The Context: Attention gathered info from neighbors. Now, the base needs to "think" about that info.
* The Mechanics: Each base vector goes through the exact same two-layer neural network.
* The Twist: Even though they use the same weights, they don't talk to each other here. Each base is processed independently.
* Why? This is where the model learns non-linear patterns (e.g., "If I saw an 'A' to my left AND a 'G' to my right, then I am likely part of a TATA-box").
# 6. The Final "Summary" (Classification)
* After many layers, we have 50 vectors for a 50bp DNA strand.We average them (Global Average Pooling) to get one single "Master Vector" that represents the whole sequence.A final Linear Layer + Sigmoid turns that Master Vector into a single probability: "Is this a TATA-box? Yes (1) or No (0)."