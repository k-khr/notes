# additive smoothing

$d$ 個のカテゴリ変数を考える。
それぞれのカテゴリ $i \in [d]$ が $\{ n_i \}_{i=1}^d$ 回ずつ生起した [^occurrence] とする。
このとき、カテゴリ $i$ が生起する経験分布は $p_{i, \text{emp}} := n_i / N$ と表わせる。
ただし、$N := \sum_{i=1}^d n_i$ である。

一方 $N$ や $n_i$ が小さいとき、この経験分布 $p_{i, \text{emp}}$ が外れ値となってしまう恐れがある。
そのため、経験分布の情報を事前分布を用いて "均す" ことを考える。

最も簡単な例として、無情報事前分布を離散一様分布 $p_{i, \text{prior}} := 1/d$ として取ることを考えよう。
全カテゴリ $i \in [d]$ において、それぞれ擬似的に $\alpha$ 回の生起が行われたとすると、擬似的なカテゴリ $i$ が生起する確率は以下のように表せる。
$$
\hat{\theta}_i = \frac{n_i + \alpha}{N + \alpha d} \tag{1}
$$
ここで、均した生起回数を $\hat{n}_i = N \hat{\theta}_i$ とするのが additive smoothing である。
各カテゴリについて擬似的な生起を足して (additive) 離散的な $n_i$ を smoothing することから、この名前がついている (ハズ)。
擬似的な生起回数 $\alpha$ は pseudocount と呼ばれ、smooting を行う際に調節可能なパラメータである。
これを大きくすると、より一様分布に近づく。


## Bayesian perspective

簡単な例として $N$ 回の独立なベルヌーイ試行を考えよう。
すなわち $d=2$ で、$n_1$ 回成功し、$n_2 = N - n_1$ 回失敗したとする。
このとき、二項分布 $\text{Binomial}(p)$ のパラメータ $p$ を推定したい。
パラメータ $p$ の事前分布が $\Beta(\alpha, \beta)$ であると仮定すると、事後分布は $\Beta(\alpha + n_1, \beta + n_2)$ となり、その期待値は以下のように表せる。
$$
\mathbb{E}\left[ p \mid \text{occurences} \right] = \frac{\alpha + n_1}{(\alpha + n_1) + (\beta + n_2)} = \frac{n_1 + \alpha}{N + \alpha (1 + \beta / \alpha)}
$$
ここで、$d=2$ ゆえ $\beta = \alpha$ とおくと式 $(1)$ と一致する。
もしこのベルヌーイ試行の成功確率に何も情報がないのであれば、無情報事前分布として $p$ の事前分布は $[0 ,1]$ を台とする一様分布と取るのが良く、その場合は $\alpha = \beta = 1$ となる。
また $\alpha = \beta$ のとき、事前分布は対称となる。

これを一般化して、カテゴリ分布のパラメータ $p_i$ の事前分布はパラメータ $\bm{\alpha} = [ \alpha_1, \alpha_2, \ldots, \alpha_d ]$ をもつディリクレ分布 $\text{Dirichlet}(\bm{\alpha})$ として表せる。
カテゴリ $i$ の生起確率 $p_i$ の事後分布は $\alpha = \sum_{i \in [d]} \alpha_i$ を用いて以下のように表せる。
$$
\mathbb{E}\left[ p_i \mid \text{occurences} \right]
= \frac{n_i + \alpha_i}{N + \alpha_i (1 + (\sum_{i \in [d]} \alpha_i) / \alpha_i)}
$$
ここですべての $i \in [d]$ に対し $\alpha_i = \alpha$ とおく、すなわち事前分布を対称なディリクレ分布とおくと、式 $(1)$ と一致する。



[^occurrence]: 生起は occurrence の訳語 (のつもり)。

