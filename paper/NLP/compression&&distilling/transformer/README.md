### Distilling Task-Specific Knowledge from BERT into Simple Neural Networks

![Distilling Task-Specific Knowledge from BERT into Simple Neural Networks](/Users/chenhao/fchome/Libraries/paper/compression&&distilling/transformer/images/Distilling Task-Specific Knowledge from BERT into Simple Neural Networks.png)

1. 模型结构：
   1. teacher model -原始的bert()
   2. student model-单层的LSTM(上述图分别对应的是single sentence任务和pair sentence任务的模型结构)
2. 作者还点到了，为了强调简单模型的效果，不使用attention这些trick
3. 损失仅仅用了target loss和最后一层的distill loss
4. CV中通常将图片扭曲折叠来扩增数据集，该论文的方法提出了一种扩增NLP数据方法（说的是数据集太小了，不好train）
   1. maks掉原句中的部分词
   2. 使用同性的词替换
   3. 从一个完整的句子中，随机的samle n-gram词作为一个句子

------

### extreme language model compression with optimal subwords and shared projections

![extreme language model compression with optimal subwords and shared projections](/Users/chenhao/fchome/Libraries/paper/compression&&distilling/transformer/images/extreme language model compression with optimal subwords and shared projections.png)

1. 模型结构如上图所示
2. teacher和student不同点
   1. 使用了不同分词方法的vocabulary词表（teacher 有30552个， student只有4928个）
   2. 训练teacher的时候，两中分词方法都用，也就是两个词表都用，在pre-trained最后mask任务中，使用不同的softmax,主要取决于该被mask的词在哪个词表汇中

3. 如题目所说，shared projection（映射）主要是利用超参数U和V将student和teacher的中间层输出强行拉到同一个可计算的空间，然后利用mean square error loss计算两个中间层的相似度。公式如下

   ![extreme language model compression with optimal subwords and shared projections2](/Users/chenhao/fchome/Libraries/paper/compression&&distilling/transformer/images/extreme language model compression with optimal subwords and shared projections2.png)

   上述公式(1)是将teacher 空间强行拉到student空间

   公式(2)是将student空间强行拉到teacher空间

4. 最终的损失也是两个， target loss + 中间层的损失

------

### Improving Multi-Task Deep Neural Networks via Knowledge Distillation for Natural Language Understanding(多任务蒸馏)

![Improving Multi-Task Deep Neural Networks via Knowledge Distillation for Natural Language Understanding](/Users/chenhao/fchome/Libraries/paper/compression&&distilling/transformer/images/Improving Multi-Task Deep Neural Networks via Knowledge Distillation for Natural Language Understanding.png)

1. 大体结构如上图所示
2. 没有太大的创新，是将多任务模型压缩到同一个模型上
3. 多个teacher, 一个学生，而且多个teacher是多个任务的。

------

### MINILM: Deep Self-Attention Distillation for Task-Agnostic Compression of Pre-Trained Transformers

![MINILM- Deep Self-Attention Distillation for Task-Agnostic Compression of Pre-Trained Transformers](/Users/chenhao/fchome/Libraries/paper/compression&&distilling/transformer/images/MINILM- Deep Self-Attention Distillation for Task-Agnostic Compression of Pre-Trained Transformers.png)

1. 基本框架如图所示
2. deep self-attention distillation,主要是说在transformer系列的teacher和student模型中，将self-attention中的query矩阵和key矩阵做点积，然后强行拉到一个空间，距离越近越好，同理，value没有和它做点积的，就自己和自己做点积，然后也是拉到同一个空间。
3. 论文说了，考虑到模型的灵活性，没有采取layer to layer的学习方式(每层都被学习)。
4. **论文介绍了一种 Teacher Assisant 的方式训练模型，其实就是阶段性训练的方法。eg: teacher 模型(layers: L, hidden size: d) ， student模型(layers: M, hidden size:s), 其中 M 远小于L, s也远小于d。可以先train一个中间模型(layers: L, hidden size:s )然后再此基础上，然后再train模型，将L缩小变为M**

------











