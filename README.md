Saliency For NMT
================

This is a collection of implementations of saliency methods for seq2seq-attn. There are five methods implemented here:
 - First-derivative sensitivity analysis
 - Input erasure (inspired by [Li, et al](https://arxiv.org/pdf/1612.08220.pdf))
 - An algorithm similar to SmoothGrad ([Smilkov, et al.](https://arxiv.org/pdf/1706.03825.pdf))
    - Differs the magnitude of the size and number of perturbations, and in input modality
 - An algorithm similar to LIME ([Ribiero, et al.](https://arxiv.org/pdf/1602.04938.pdf))
    - Differs in that it does continuous perturbations of the input vectors, it does not use any distance-based kernel for its cost function, and it uses cross-validated LASSO rather than the proposed k-LASSO.
    - Note: this algorithm is very slow and can take up to a couple minutes to run on a single sentence; I recommended that you disable it if you are not using it.
 - Layerwise Relevance Propagation (method taken exactly from [Arras, et al.](https://arxiv.org/pdf/1706.07206.pdf))

To see an example of usage, see `example.lua`, which takes arguments `-model` (a seq2seq-attn model file), `-dict` (a seq2seq-attn src.dict file), and `-description` (an optional file, which should be of the type generated by `describe.lua` from [seq2seq-comparison](https://github.com/dabbler0/seq2seq-comparison) and is used to normalize activations to mean-0 stdev-1). It then takes input in stdin, alternating sentences to run saliency on and a neuron to examine in the saliencies that were just generated.

Some examples of saliency in action from an English-Spanish translation network:
  - [a neuron that detects whether a token is inside parentheses](https://rawgithub.com/dabbler0/saliency/master/examples/paren-saliencies.html)
  - [a neuron that seems to detect whether a token is inside a negated clause](https://rawgithub.com/dabbler0/saliency/master/examples/negation-saliencies.html)

In these examples saliency was taken at the end of each table with respect to all of the prior tokens in the sentence. Each file contains 400 example sentence with saliency taken at a random token.

Some select examples from the negated clause neuron:

![Recognizing the word "not" activating](https://github.com/dabbler0/saliency/blob/master/examples/saliency-example-1.png?raw=true)
![Recognizing the word "excluding" activating](https://github.com/dabbler0/saliency/blob/master/examples/saliency-example-2.png?raw=true)
![Recognizing the word "as" and a comma stopping](https://github.com/dabbler0/saliency/blob/master/examples/saliency-example-3.png?raw=true)
