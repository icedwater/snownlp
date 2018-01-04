# SnowNLP: Simplified Chinese Text Processing

SnowNLP is a Python library which makes processing Chinese character text more convenient. It was inspired by [TextBlob](https://github.com/sloria/TextBlob). As most of the Natural Language Processing tools are targeted at English, I (@isnowfy) wrote a library for Chinese character text. Furthermore, unlike TextBlob, SnowNLP does not use NLTK. Rather, I use my own calculation methods here, and provide some trained corpora. Do note that this library works only on Unicode characters, so please make sure your text has been decoded into unicode first.

    from snownlp import SnowNLP

    s = SnowNLP(u'这个东西真心很赞')

    s.words         # [u'这个', u'东西', u'真心',
                    #  u'很', u'赞']

    s.tags          # [(u'这个', u'r'), (u'东西', u'n'),
                    #  (u'真心', u'd'), (u'很', u'd'),
                    #  (u'赞', u'Vg')]

    s.sentiments    # 0.9769663402895832 positive的概率

    s.pinyin        # [u'zhe', u'ge', u'dong', u'xi',
                    #  u'zhen', u'xin', u'hen', u'zan']

    s = SnowNLP(u'「繁體字」「繁體中文」的叫法在臺灣亦很常見。')

    s.han           # u'「繁体字」「繁体中文」的叫法
                    # 在台湾亦很常见。'

    text = u'''
    自然语言处理是计算机科学领域与人工智能领域中的一个重要方向。
    它研究能实现人与计算机之间用自然语言进行有效通信的各种理论和方法。
    自然语言处理是一门融语言学、计算机科学、数学于一体的科学。
    因此，这一领域的研究将涉及自然语言，即人们日常使用的语言，
    所以它与语言学的研究有着密切的联系，但又有重要的区别。
    自然语言处理并不是一般地研究自然语言，
    而在于研制能有效地实现自然语言通信的计算机系统，
    特别是其中的软件系统。因而它是计算机科学的一部分。
    '''

    s = SnowNLP(text)

    s.keywords(3)	# [u'语言', u'自然', u'计算机']

    s.summary(3)	# [u'因而它是计算机科学的一部分',
    			    #  u'自然语言处理是一门融语言学、计算机科学、
    			    #    数学于一体的科学',
		        	#  u'自然语言处理是计算机科学领域与人工智能
		        	#	 领域中的一个重要方向']
    s.sentences

    s = SnowNLP([[u'这篇', u'文章'],
                 [u'那篇', u'论文'],
                 [u'这个']])
    s.tf
    s.idf
    s.sim([u'文章'])# [0.3756070762985226, 0, 0]

## Features

* Chinese Word Segmentation ([Character-Based Generative Model][cgm])
* Part-of-Speech Tagging ([TnT][TnT] 3-gram HMM)
* Sentiment Analysis (Currently based on purchase reviews, may not work well on other data for now.)
* Text Classification (Naive Bayes)
* Character-to-Pinyin conversion (Trie-based)
* Traditional-to-Simplified conversion (Trie-based)
* Keyword generation ([TextRank][rank])
* Summarization ([TextRank][rank])
* tf, idf
* Tokenization (separation into sentences)
* Text similarity measures ([BM25][BM25])
* Supports Python3 (Thanks to [erning][erning])

## Get It now

    $ pip install snownlp

## About data training

I have provided some training data and methods for segmentation, part-of-speech tagging, and sentiment analysis.

Using segmentation as an example:

In the `snownlp/seg` folder

    from snownlp import seg
    seg.train('data.txt')
    seg.save('seg.marshal')
    # from snownlp import tag
    # tag.train('199801.txt')
    # tag.save('tag.marshal')
    # from snownlp import sentiment
    # sentiment.train('neg.txt', 'pos.txt')
    # sentiment.save('sentiment.marshal')

The data has been saved as `seg.marshal`, but you may edit `data_path` in `snownlp/seg/__init__.py` to point to the newly-trained data.

## License

MIT licensed.

[cgm]: http://aclweb.org/anthology//Y/Y09/Y09-2047.pdf
[TnT]: http://aclweb.org/anthology//A/A00/A00-1031.pdf
[rank]: http://acl.ldc.upenn.edu/acl2004/emnlp/pdf/Mihalcea.pdf
[BM25]: http://en.wikipedia.org/wiki/Okapi_BM25
[erning]: https://github.com/erning
