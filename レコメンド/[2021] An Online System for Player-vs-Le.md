# [2021] An Online System for Player-vs-Level Matchmaking in Human Computation Games  
これまで提案されてきた、skill chain / rating arrayといった手法に独自要素のイプシロン・グリーディーを加え、  
実験を行なった。  
人はamazonのサービスを使って雇える。  
https://aws.amazon.com/jp/premiumsupport/knowledge-center/mechanical-turk-use-cases/


# ${\epsilon}greedy$  
オンラインのモードでは  
- リリースされたばかりの問題の難易度は不正確である。  
- 難易度が高すぎる問題は、上級者ばかりが挑戦するため、評価が歪んでしまう。  
といったような問題がある。  
これを解消するためにイプシロン・グリーディなマッチングを行う。  
![image](https://user-images.githubusercontent.com/54636129/199463975-ae1ff637-b942-4256-9280-6eb6d7ce3e73.png)  
  
# rating arraysのテスト  
各段階に対して、単独のratingをつけるのではなく、arrayタイプのratingをつけることにより、  
多面的でダイナミックなマッチングを行える。  
思想はvector matchingに近そう。  
  
# 結果  
![image](https://user-images.githubusercontent.com/54636129/199468440-da88c6ea-f3f1-45b2-b341-2cbcf9bd39bf.png)  
イプシロングリーディの導入により、プレイヤーkpiは下がったもの、レーティングのエラーは小さくなった。  
また、完全ランダムなときに比べると、今回用いたモデルの方がプレイ時間が、解けた問題の数、間違えた問題の数の面で改善が見られる。  
arrayを使った場合と使わない場合で比べるとプレイ時間、クリア数にわずかな改善が見られる。  
  
# 考察  
この論文から得られるものは少なかったが、DDA分野の読むべき論文へのアクセスが得られた。  



