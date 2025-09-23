# Notes for AI-LLM knowledge base

> refer to [plan for 2025](https://docs.google.com/spreadsheets/d/1iUaLCp-V2caDFwJS4PCY1MrNhlYZv3rqJJjHtvXeUUk/edit?gid=0#gid=0)


#### The key differences between BPE, WordPiece and Unigram tokenization algorithms

**BPE** - Byte-Pair Encoding Tokenization

- BPE is a subword tokenization algorithm that starts with a small vocabulary and learns merge rules.  
- BPE tokenizers learn merge rules by merging the pair of tokens that is the most frequent.  
- BPE tokenizes words into subwords by splitting them into characters and then applying the merge rules.  

**WordPiece**

- WordPiece is a subword tokenization algorithm that starts with a small vocabulary and learns merge rules.  
- A WordPiece tokenizer learns a merge rule by merging the pair of tokens that maximizes a score that privileges frequent pairs with less frequent individual parts.  
- WordPiece tokenizes words into subwords by finding the longest subword starting from the beginning that is in the vocabulary, then repeating the process for the rest of the text.  

**Unigram**

- Unigram is a subword tokenization algorithm that starts with a big vocabulary and progressively removes tokens from it.
- Unigram adapts its vocabulary by minimizing a loss computed over the whole corpus.
- Unigram tokenizes words into subwords by finding the most likely segmentation into tokens, according to the model.


  
