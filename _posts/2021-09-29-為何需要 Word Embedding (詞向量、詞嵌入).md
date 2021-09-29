---
title: 為何需要 Word Embedding (詞向量、詞嵌入)
author: Yu-Lun Chiang
date: 2021-09-29 14:00:00 +0800
categories: [AI, Natural-Language-Processing, Word-Embedding]
tags: [ai, nlp, word-embedding]
math: true
image:
  src: https://images.unsplash.com/photo-1620969427101-7a2bb6d83273?ixlib=rb-1.2.1&q=85&fm=jpg&crop=entropy&cs=srgb
  width: 850
  height: 585
---

不寫出來，就沒印象，凡讀過必忘。

有能力檢索自己腦袋中的記憶寶殿，在正確的時間拾起正確的記憶，那是我嚮往的能力。

---
## 前言

至今，自然語言處理中最重要的基石：Word Embedding，其起初的想法與後續的發展皆圍繞在一位語言學家 John Rupert Firth 在 1957 年曾經說過的一句名言：

> You shall know a word by the company it keeps. (Firth, J. R. 1957)

<span style="color:green">**藉由上下文判斷該詞的意義**</span>，看似再稀鬆平常不過了，甚至時常在讀英文時，許多讀書技巧也是圍繞著這個道理。但一件事物的本質與意義是由旁物推敲而定，總覺得不那麼紮實可靠。不過，每每用網路上的劍橋辭典查詢英文單字時，總是出現更多英文單字來解釋之，或許，這樣的現象最能解釋 John Rupert Firth 的這句名言吧！

我們深知任何語言不過是一連串符號象徵，其背後意義在人腦中如何運算無從得知。企圖用電腦運算來處理語言、理解語言，便必須使用數值來表示這些語言符號，這便是 Word Embedding 的起源。

那麼，現在問題變成，<span style="color:green">**該用什麼數值來表示字詞**</span>，便是 Word Embedding 最需要探討的議題了。

題外話，我認為為何深度學習能在電腦視覺與聲音辨識中，相較於自然語言處理，更能快速取得成功的其中一項因素，因為電腦視覺與聲音辨識的資料本身即是數值，而自然語言的資料是符號，需要額外轉換為適當的數值，發展成本與阻礙更多。


---
## One Hot Representation

當時，人們想到一種簡單的方式來進行轉換：One Hot Representation。

假設擁有一部字典，裡頭只有五個字：「蘋果」、「漂亮」、「台灣」、「日本」、「香蕉」。

每個字詞代表為一種維度。
- 蘋果 = [1, 0, 0, 0, 0]
- 漂亮 = [0, 1, 0, 0, 0]
- 台灣 = [0, 0, 1, 0, 0]
- 日本 = [0, 0, 0, 1, 0]
- 香蕉 = [0, 0, 0, 0, 1]

我們會發現，每個字詞都是<span style="color:green">**正交 (Orthogonal)**</span>，換句話說，在這五維空間中，這五個字詞彼此之間的<span style="color:green">**距離是相同的**</span>。

停下來想一想，這樣合理嗎？舉一個例子來瞧瞧：

- <span style="color:red">台灣</span>出產<span style="color:blue">香蕉</span>，總是外銷<span style="color:red">日本</span>。
- <span style="color:red">日本</span>種植<span style="color:blue">蘋果</span>，總是外銷<span style="color:red">台灣</span>。

在這兩句話當中，可以發現，<span style="color:red">台灣</span>與<span style="color:red">日本</span>、<span style="color:blue">香蕉</span>與<span style="color:blue">蘋果</span>，分別是兩組意義相似的概念。在空間中，直覺上彼此距離應該要彼此相近、與其他字詞的距離應該要相遠才是，而用 One Hot Representation 表示的字詞，彼此距離卻是相同的，這是 One Hot Representation 的缺點：「<span style="color:green">**語義鴻溝**</span>」。

再來，字典中不單單只有五個字，當裡頭含有五萬字詞時，若使用 One Hot Representation，代表每個字詞須用五萬個維度來表示，且其中 49999 個維度是零、1 個維度是一，計算相當不方便且無效率呢！這是 One Hot Representation 的第二個缺點，「<span style="color:green">**維度災難**</span>」。


---
## 稍作片刻、駐足思考

”You shall know a word by the company it keeps.” 一個字詞的意義來自於它身旁的上下文，背後延伸的概念是「<span style="color:green">**擁有相似上下文的字詞，詞義應相似；擁有相異上下文的字詞，詞義應相異**</span>」。

套用空間概念來思考：「擁有相似上下文的字詞，轉換成 Word Embedding 之後，在空間中的距離應相近；反之，距離應相遠」。那麼，適當的 Word Embedding 應該滿足上述條件，亦是 Hinton 所提及的 Distribution Hypothesis 的全貌。


---
## Distribution Representation

1986 年，深度學習之父 Hinton，發表一篇論文《Distribution Representations》，提到將每個神經元(維度)表示為一個概念時，叫做 Local Representation，One Hot Representation 是 Local Representation 的一種；另外，提出 <span style="color:green">**Distribution Representation**</span> 的概念，<span style="color:green">**每個神經元(維度)可以表示多組概念，每個概念可被多個神經元(維度)所表示**</span。

Distribution Representation 一次解決了「語義鴻溝」與「維度災難」兩個問題。其一，這些神經元能透過模型訓練，使其符合 Distribution Hypothesis (擁有相似上下文的字詞，空間中彼此距離相近)；其二，維度(神經元個數)不必因字典大小而無限擴展，相較於 One Hot Representaion 來說，是較低維的向量(少於字典中的字詞數)。

自此之後，自然語言處理領域中， 「Word Embedding」、「詞嵌入」、「詞向量」，幾乎都由 Distribution Representation 表示，只是獲得此向量的方式不一，百家齊放。

---
## 結語

Word Embedding，是銜接人類符號與電腦運算的重要橋樑，是深度學習在自然語言處理領域中不可或缺的基石。

透過簡單的文章，不只分享自己整理好的知識，也不斷更新自己腦袋中的知識，分享即幫助自己。

