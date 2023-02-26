# Natural language APIに関して  
Googleが学習、管理している自然言語処理用のAPI。  
学習不要で使用可能。

# 料金
![image](https://user-images.githubusercontent.com/54636129/221351846-296a3732-229e-4e1c-a9b7-e55c5d8ffba1.png)  
  
おそらく対象になるのはエンティティ分析と感情分析、コンテンツ分類か。  
料金は5000件以降は1000件ごとに$1のコストがかかる。  

# 機能  
- sentiment
  - positive / negativeを判定。  
- entitiy detection
  - 文章中に登場するentity(話題？)を抽出し、それに対してタグをつける。
- category
  - 何に対する文なのかを判断。現状日本語非対応。

さっと試すだけなら[こちら](https://cloud.google.com/natural-language?hl=ja)から実践可能。
# 実験  
google play storeから手動で取ってきたストアレビューを用いて検証  
  
# 結果


# 参考
[プロダクト概要](https://cloud.google.com/natural-language/docs/basics?hl=ja)  
[クライアントライブラリ](https://cloud.google.com/natural-language/docs/reference/libraries)
[やってみたブログ](https://oc-technote.com/python/google-cloud-natural-language-extract-japanese-address/)
