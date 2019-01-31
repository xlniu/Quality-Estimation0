# Quality-Estimation
机器翻译子任务-翻译质量评价<br>

##简介：
翻译质量评价（Quality Estimation,QE）是机器翻译领域中的一个子任务，大致可分为 Sentence-level QE，Word-level QE，Phrase-level QE，详情可参考WMT(workshop machine translation)比赛官网 http://www.statmt.org/wmt17/quality-estimation-task.html 。本项目针对 Sentence-level QE，使用 bert生成翻译句对中单词的 context embedding，然后将其输入到 bi-lstm 中，使用最后一个隐层节点的输出计算翻译质量评分。由于 wmt18-qe 的测试集标签没有公布，本项目仅在 wmt17-qe 数据集上进行实验。

##实验需要的包：
tensorflow >= 1.11.0<br>
keras(在2.2.4下测试通过，其他版本应该也是可以的，请自行尝试)<br>
matplotlib<br>

##实验步骤：
1、准备数据，下载17年wmt sentence level的数据，并将数据改写成标准形式：'src ||| mt',示例：'Who was Jim Henson ? ||| Jim Henson was a puppeteer';
数据在比赛官网可以下载到，代码见convert_data.ipynb;<br>
2、使用bert提取特征(context embedding)，代码见extract_features.py(WordPiece tokenize在这个代码中实现);<br>
这里用到的预训练模型是：BERT-Base, Multilingual Cased (New, recommended): 104 languages, 12-layer, 768-hidden, 12-heads, 110M parameters，可以到这里下载：https://github.com/google-research/bert ;<br>
3、将提取到的特征转换为后续bi-lstm需要的输入格式,代码见：convert_features.ipynb;<br>
4、使用bi-lstm训练翻译质量评价模型，下面举一个例子说明bi-lstm的输入、输出，具体的细节见代码：qe_model.ipynb(keras);<br>
输入：'Who was Jim Henson ? [SEP] Jim Henson was a puppeteer [SEP]'(输入为每个单词的context embedding);<br>
输出：翻译质量评分score;<br>

##实验结果：

