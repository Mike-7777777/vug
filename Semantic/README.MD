Name in DB = Real Name

---

Race - Hobbits 霍比特

FRODO = Frodo Baggins

SAM = Samwise Gamgee "Sam"

PIPPIN = Peregrin Took "Pippin"

MERRY = Meriadoc Brandybuck "Merry"

BILBO = Bilbo Baggins

GOLLUM = Gollum(Smeagol)

---

Race - Maiar 玛雅

GANDALF = Gandalf/Mithrandir

SARUMAN = Saruman

---

RACE -ELF 精灵

LEGOLAS = Legolas Greenleaf

ARWEN = Arwen

GALADRIEL VOICE OVER / GALADRIEL = Galadriel

ELROND = Elrond

---

Dwarves 矮人

Gimli = Gimli

---

Ents 树人

TREEBEARD = Treebeard

---

Man 人

ARAGORN = Aragorn

BOROMIR = Boromir

FARAMIR = FARAMIR

Denethor

THEODEN = Théoden

EOMER = Éomer

EOWYN = Éowyn

---



in Lsn

```
FRODO: 0.07448395061728395
SAM: -0.10780736842105262
GANDALF: -0.020832786885245903
ARAGORN: 0.0026746031746031707
GOLLUM: 0.026374698795180732
SMEAGOL: 0.09622333333333334
PIPPIN: 0.17629113924050632
MERRY: 0.14185217391304344
ARWEN: 0.0005785714285714441
```



lifespan

AVG. lifespan in their race



Hobbit lifespan - around 100 (need source)

Elf 精灵 - not dead



So they are all like immortal



台词量排序

FRODO

SAM

GANDALF

ARAGORN

PIPPIN

MERRY

GOLLUM

GIMLI

THEODEN













NOTEBOOK outlines



最后：角色的用词情绪值与他们目前年龄的对比（无视时间，因为都发生在三年内）





把每个人说最多的两三个词 写出来



## 参考：

Lsn.ipynb

1. 预处理（其实只是删除了“ ,”）
2. 按字数重新排序（说话最多的人物在第一位）
3. 按作品分类（将每个角色说的话按三个电影分开）
4. 做了一个fancy的bokeh图（真的很没有意义）
5. Word Cloud图
6. Sentiment analysis - 求了平均值



```
 res_dic = [vader.polarity_scores(text) for text in charlist[char]]
 res_neg = np.mean([res_dic[i]['neg'] for i in range(len(res_dic))])
 res_pos = np.mean([res_dic[i]['pos'] for i in range(len(res_dic))])
 res_com = np.mean([res_dic[i]['compound'] for i in range(len(res_dic))])
 
cite
Hutto, C.J. & Gilbert, E.E. (2014). VADER: A Parsimonious Rule-based Model for Sentiment Analysis of Social Media Text. Eighth International Conference on Weblogs and Social Media (ICWSM-14). Ann Arbor, MI, June 2014.
```





https://www.kaggle.com/code/mroczekj/school-project

#### pre-processing

todo Remove NA rows or column



第一部分（处理dialog列）：

删除所有标点符号

去除多余的空白处

将所有单词转化为小写

去除停顿词

去除数字



第二部分（处理char列）：

把修饰词去掉了

删掉了char里面的

voiceover

voice

mouth

eye

```
修改了一部分也许是typo的东西

argorn -> aragorn
gan dalf -> gandalf
gatekeeper -> gatekeepr
smeagol - gollum
white wizard - gandalf
```

其他跟上面一样



还处理了characters.cvs，把所有含有NaN的都删了（结果就剩十几行了，不知道是不是这样的）

```
lotr_characters.dropna(inplace=True)
```

---

#### 感受分析 Sentiment analysis

```
def flatten(t):
	return [iten for sublist in t for item in sublits]
```

问题1 谁是最消极的角色

```
使用NRC 看起来没有NLTK好用阿
而且他是按单字估计的，数量多 = 更消极 这个其实应该按比例吧，当然他后续也提供了比例。
```

问题2：哪个种族是最愤怒的种族？

目标3：霍比特人更积极还是人？

---



第三篇参考

https://www.kaggle.com/code/tombowe/lord-of-the-rings-sentiment-outliers

```R
#### Clean up data a bit
lotr <- rename(lotr, dialogue = dialog, character = char) %>%
  select(character, dialogue, movie) %>%
  mutate(character = str_trim(character, side = "both")) %>%
  mutate(character = str_replace_all(character, "[()]","")) %>%
  mutate(character = str_replace_all(character, "[[:digit:]]+", "")) %>%
  mutate(character = str_replace_all(character, "ARGORN","ARAGORN")) %>%
  mutate(character = str_replace_all(character, "ARWEN VOICEOVER","ARWEN")) %>%
  mutate(character = str_replace_all(character, "BILBO VOICEOVER","BILBO")) %>%
  mutate(character = str_replace_all(character, "ELROND VOICEOVER","ELROND")) %>%
  mutate(character = str_replace_all(character, "EYE OF SAURON","SAURON")) %>%
  mutate(character = str_replace_all(character, "FRODO VOICE OVER","FRODO")) %>%
  mutate(character = str_replace_all(character, "FRODO VOICE","FRODO")) %>%
  mutate(character = str_replace_all(character, "GALADRIEL VOICE OVER","GALADRIEL")) %>%
  mutate(character = str_replace_all(character, "GALADRIEL VOICEOVER","GALADRIEL")) %>%
  mutate(character = str_replace_all(character, "GALADRIEL VOICE-OVER","GALADRIEL")) %>%
  mutate(character = str_replace_all(character, "GAN DALF","GANDALF")) %>%
  mutate(character = str_replace_all(character, "GANDALF VOICE OVER","GANDALF")) %>%
  mutate(character = str_replace_all(character, "GANDALF VOICEOVER","GANDALF")) %>%
  mutate(character = str_replace_all(character, "GATEKEEPR","GATEKEEPER")) %>%
  mutate(character = str_replace_all(character, "GENERAL SHOUT","GENERAL")) %>%
  mutate(character = str_replace_all(character, "MERRY and PIPPIN","MERRY")) %>%
  mutate(character = str_replace_all(character, "MERRY AND PIPPIN","PIPPIN")) %>%
  mutate(character = str_replace_all(character, "ORCS","ORC")) %>%
  mutate(character = str_replace_all(character, "SAM VOICEOVER","SAM")) %>%
  mutate(character = str_replace_all(character, "SARUMAN VOICE OVER","SARUMAN")) %>%
  mutate(character = str_replace_all(character, "SARUMAN VOICEOVER","SARUMAN")) %>%
  mutate(character = str_replace_all(character, "SAURON VOICE","SAURON")) %>%
  mutate(character = str_replace_all(character, "SMEAGOL","GOLLUM")) %>%
  mutate(character = str_replace_all(character, "SOLDIERS IN MINAS TIRITH","SOLDIER")) %>%
  mutate(character = str_replace_all(character, "SOLDIERS ON GATE","SOLDIER")) %>%
  mutate(character = str_replace_all(character, "SOLDIERS","SOLDIER")) %>%
  mutate(character = str_replace_all(character, "STRIDER","ARAGORN")) %>%
  mutate(character = str_replace_all(character, "URUK-HAI","URUK HAI")) %>%
  mutate(character = str_replace_all(character, "WHITE WIZARD","GANDALF"))
```

---

第四篇

https://www.kaggle.com/code/xvivancos/analyzing-the-lord-of-the-rings-data/report

```
数据清理
removePunctuation(). Remove all punctuation marks 删除所有标点符号
stripWhitespace(). Remove excess whitespace 删除多余的空白处
tolower(). Make all characters lowercase 删除多余的空白处
removeWords(). Remove some common English stop words (“I”, “she’ll”, “the”, etc.)删除一些常见的英语停顿词
removeNumbers(). Remove numbers删除数字
```



图

1 台词数量

三部曲中对话最多的角色是什么？- 相对占比 -绝对数量

按照电影分类 - 三个电影总台词比例的饼图



2 情感分析 [使用的[方法](https://www.cs.uic.edu/~liub/FBS/sentiment-analysis.html) 以及NRC]

二元归类 -pos -neg 有默认方法可以用来制作可视化云图

NRC的多元归类 - 可以找到三部曲中各种情绪的排序（总的、分电影的）

​	积极、消极、愤怒、期待、厌恶、恐惧、喜悦、悲伤、惊讶和信任等

​	我们可以通过这种可视化的方式发现一些奇怪的东西。双塔》是有最多对话的电影（正如我们在前面的情节中看到的），但不是有更多相关情感的电影。也许这是因为《双塔》是一部有更多动作的电影。我们还可以用这个词库来计算每一种情绪的最频繁词汇（NRC可以有默认方法进行单词数量排序，这比nltk方便，但其实也很容易实现）。

​	NRC除了可以对不同心情的高频词汇展示，NRC还能对每个角色使用的高频词汇展示。



3 第三种尝试 - [AFINN](http://www2.imm.dtu.dk/pubdb/pubs/6010-full.html)情感词典

分数在-5到5之间，负分表示负面情绪



总结

最爱说话的人物
每部电影中最常出现的词
双词的网络
每个角色最常出现的词
使用统计学tf-idf的每个角色的最相关的词
使用bing、nrc和AFINN词典进行的情感分析
一些可视化的人物信息（性别、境界、种族等）。





我刚看过你的代码，基本上你做完了全部内容，而且比我写的更容易理解。但我们的结论有些不同，我估计是因为我们清理数据的方法不一样，我觉得我的清理能让我们获得多一点数据，你可以检查一下。

所以数据清理是一个可以优化的地方。另外，代码中的“sentimentA()”函数可以被我的代码中的“sentimentC()”函数替代。请注意，我不知道这会不会带来更好的结果，只是作为一个提议（它们的区别在于A获得的是每一句情感分析的平均值，而C获得的是每个单词情感分析的平均值）。