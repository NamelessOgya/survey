## [2020]Learning from positive and unlabeled data: a survey  
PU(positive learning)に関してのサーベイ  
https://link.springer.com/content/pdf/10.1007/s10994-020-05877-5.pdf  
 興味のある部分のみ抜粋。
 
## PUとは　　
確実にpositiveであるサンプルとunlabeledのサンプルが与えられた状態で
学習を行うタスクのこと。  
半教師学習は学習時にpositive/negative両者のデータが与えられており、
この点において問題設定が異なる。  
  
 ##  assumpution   
 assumptionによって用いるべき解法が異なる。
 
 ### positiveデータの割り振りに関するassumption
 
- **selected completely as random**  
positiveデータがpositiveデータに含まれるかunlabeledに含まれるかはランダムに決まること  
  
- **Selected at random**  
共変量の値によって、どちらに含まれるかが確率的に決まること  

### データのassumetion  
- **■separability**  
positive/negativeがある閾値を境にくっきりとわかれること  
  
- **■smoothness**  
確率分布が唐突に変化しないこと  
共変量が似たインスタンスは似た確率をとる。
>According to the smoothness assumption, examples that are close to each other are more
likely to have the same label.

## learning methods
### two-step technique  
separability と smoothnessが満たされている場合適用対象となる。  
- **step1**  
unlabeledサンプルから代表的なpositive / negativeサンプルを分ける  
- **step2**  
semi-supervisedな学習を行う。

### Biased learning  
SCARが満たされている場合に適応となる。
unlabeledサンプルに含まれるデータはすべてNegativeと考えて学習を行う。  
その際にpositiveとわかっているものを外した時に重い罰則を科すなどして、positiveサンプルをよく判別できるようにする。

## 感想
- positive / negative両方のサンプルがある場合にはsemi-supervidedな手法が適応となるようだった。
two-step techniqueの部分にも必要なので、どちらにせよ学ぶ必要あり。
手法の細かい部分はsemi-supervisedな手法を学習してから読んでみる。



