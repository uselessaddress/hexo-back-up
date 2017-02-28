---
title: 文本学习迷你项目
toc: true
date: 2017-02-26 20:00:00
tags:
- 转载
- 机器学习
categories: 精华转载
---
# 项目简介
本节课开始，你使用大量监督式分类算法，根据作者来识别邮件。 在这些项目中，我们为你做了预处理，将输入邮件转换到 TfIdf 中，这样你就能向算法提供这些邮件了。 现在，你将自行完成预处理工作，以便你能从原始数据直接得到经过处理的特征。

你将得到两个文本文件：一个包含来自 Sara 的所有邮件，一个包含 Chris 的邮件。 你还将访问 parseOutText() 函数，该函数接受作为参数的已读邮件，并且返回包含邮件中所有（被词干化的）单词的字符串。

<!--more-->

# 项目准备
```
# 下载语料库
import nltk
nltk.download()

# 停止词
from nltk.corpus import stopwords
sw = stopwords.words('english')

print len(sw)

# 词干化
from nltk.stem.snowball import SnowballStemmer
stemmer = SnowballStemmer('english')
stemmer.stem('responsiveness')
stemmer.stem('responsivity')
stemmer.stem('unresponsive')

```

# parseOutText()
你将从热身习题开始了解 parseOutText()。 前往工具目录并运行 parse_out_email_text.py，该程序包含 parseOutText() 和一封测试邮件，用以运行此函数。

parseOutText() 获得被打开的邮件，然后仅返回文字部分，除去了可能出现在邮件开头的元数据，接下来就是邮件正文了。 我们现在已经设置好了这一脚本，这样它就能将邮件正文打印到屏幕上，你运行 parseOutText() 时会得到怎样的正文？
答：Hi Everyone  If you can read this message youre properly using parseOutText  Please proceed to the next part of the project

在 parseOutText() 中，添加以下注释： 
```
# words = text_string 
```

增强 parseOutText() ，这样返回的字符串就有了所有因使用 SnowballStemmer（使用 nltk 包，可在 http://www.nltk.org/howto/stem.html 找到我发现有用的一些示例）而获得的词干化单词。

重新运行 parse_out_email_text.py，该程序将使用你更新的 parseOutText() 函数。你现在的输出是什么？
答：hi everyon if you can read this messag your proper use parseouttext pleas proceed to the next part of the project

提示：你需要将字符串分解成单个单词，词干化每个单词，然后再将所有单词重新组合成一个字符串。

# 清除“签名文字”
在 text_learning/vectorize_text.py 中，你将迭代所有来自 Chris 和 Sara 的邮件。 将每封已读邮件提供给 parseOutText() 并返回词干化的文本字符串。然后做以下两件事：

- 删除签名文字（“sara”、“shackleton”、“chris”、“germani”——如果你知道为什么是“germani”而不是“germany”，你将获得加分）
- 向 word_data 添加更新的文本字符串——如果邮件来自 Sara，向 from_data 添加 0（零），如果是 Chris 写的邮件，则添加 1。

完成此步骤后，你应该有两个列表：一个包含了每封邮件被词干化的正文，第二个应该包含用来编码（通过 0 或 1）谁是邮件作者的标签。

对所有邮件运行程序需要花一些时间（5 分钟或更长时间），所以我们添加了一个 temp_counter，将第 200 封之后的邮件切割掉。 当然，一切就绪后，你会希望对整个数据集运行程序。

在以下方框中，放入你得到的 word_data[152] 字符串。
答：tjonesnsf stephani and sam need nymex calendar

# 进行 TfIdf
使用 sklearn TfIdf 转换将 word_data 转换为 tf-idf 矩阵。删除英文停止词。

你可以使用 get_feature_names() 访问单词和特征数字之间的映射，该函数返回一个包含词汇表所有单词的列表。有多少不同的单词？
答：

你 TfId 中的单词编号 34597 是什么？
答：stephen

需要说明的是，如果问题是“单词编号 100 是什么”，我们肯定会查找对应 vocab_list[100] 的单词。有时候，零索引数组谈论起来非常不好理解。


# 源码分享
https://github.com/voidking/ud120-projects

# 书签
机器学习入门
https://cn.udacity.com/course/intro-to-machine-learning--ud120

