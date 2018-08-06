# distill-attention

*Preliminary draft for journal submission to [Distill](distill.pub)*

## Attention (working title)

### Attention in Humans

- Similar to Olah's LSTM [blogpost](http://colah.github.io/posts/2015-08-Understanding-LSTMs/), arguing that humans do not process input sequentially.
- Processing input sequentially might not be optimal

*Golden ratio and attention in paintings and images; nature's warnings from colors*

### Drawbacks of Pre-Attention Networks

- Compressing a variable sequence into a fixed-length vector (see Figure 2 from Bahdanau et. al ([2015](https://arxiv.org/pdf/1409.0473.pdf))
- Unnecessarily imposing long-term dependencies

*Reference to seq2seq paper that uses reversed inputs; interactive demo for number of operations in seq2seq translation*

### A Mathematical Function for Attention

- Introduce concepts of Query, Key, Value
- Introduce dot-product attention and other variants

### Regular Attention and Self-Attention

- Show intuitive uses of regular and self-attention in different examples
- Translation; SAGAN

### Implementing Attention

- Examples of Attention mechanisms
- Show several different examples with the aim of allowing reader to implement Attention is their own domain
- Transformer; Decoder-Only Transformer; Image Captioning; SAGAN

### Advantages of Attention

- Shortening long-term dependencies
- Interpretability (see Figure 3 from Bahdanau et. al ([2015](https://arxiv.org/pdf/1409.0473.pdf)), Appendix from Vaswani et. al ([2017](https://arxiv.org/abs/1706.03762)) and attention used in image-captioning)

### Disadvantages of Attention

- Positional invariance

***Fun Examples!***

- [Translation](https://github.com/greentfrapp/attention-primer/tree/master/5_translation)
- MNIST captioning

