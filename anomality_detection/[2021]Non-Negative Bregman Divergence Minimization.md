## [2021]Non-Negative Bregman Divergence Minimization for Deep Direct Density Ratio Estimation
Masahiro Kato 1 Takeshi Teshima 2
(Cyberagentの人たち)

Density ratio estimationに対してflexibleなモデルを使うと


## 問題設定
- $p_{nu}(X)$, $p_{de}(X)$が与えられたとき、$r(x) = \frac{p_{nu}(x)}{p_{de}(x)}$の値を$X^{nu}$, $X^{de}$のサンプルから求めること
が目標。

- DRM-BD minimization
	- let $f : (b_r, B_r) → R$
		- これが学習モデルになる。
	- $X_{nu} \sim p_{nu}(X)$, $X_{de} \sim p_{de}(X)$
	- $BD_{f}(r) = \hat{E}_{de}[\partial f(r(X_{i}))r(X_{i}) - f(r(X_{i}))] - \hat{E}_{nu}[\partial f(r(X_{j}))]$
		- これをそれぞれのサンプルに対して最小化することにより、密度推定を行う。

## train-loss hackingの問題
density ratio estimationのタスクにflexibleなモデルを使うとoverfittingしやすいことが知られている。
- DRM-BD minimizationの場合は二項目の$\hat{E}_{nu}[\partial f(r(X_{j}))]$の値は、
$X_{nu}$か存在しない値の部分でサージ的にモデルの出力を大きくすることにより
無限に小さくすることが可能なので、こういった極端な方向に学習が進んでしまう。
（overfitしてしまう）
 ![17b8afcc012a420d82335f425e059ad9](https://user-images.githubusercontent.com/54636129/172040947-67083a4b-6e6a-4356-98fd-10c95b65bb45.png)

## train-loss hackingを解決するNon-negative BD
いくつかの仮定をおくことで、この問題を回避することが可能。
- 仮定1
	- rは上から抑えられている。
		- 上限値を$\bar{R}$として、ハイパーパラメータCを以下を満たすように置く。
			- $0 < C < \frac{1}{\bar{R}}$
- 仮定2
	- $\partial f(t) = C(\partial f(t)t - f(t)) + \hat{f}(t)$ で決まる$\hat{f}$も上から抑えられている。

この時、

![cef06b60eaf1af665bcc1e7cba82140c](https://user-images.githubusercontent.com/54636129/172040986-7665ec62-103c-4723-87f3-815b53d3f303.png)
![74848f1a7ff6dcb7ade9b2313ea0185a](https://user-images.githubusercontent.com/54636129/172040998-edc626ed-e4c8-4f7c-95ef-0cb1b547249c.png)

を使うことにより、過学習を抑えた学習が可能。

## これから読むもの
- Density ratio matching under the bregman divergence: A unified framework of density ratio estimation.
	- DRM-BD minimizationを提案した論文

