# 「Pythonによるデータ分析入門」 Wes McKinney 著 瀬戸山雅人、小林儀匡 訳 オライリージャパン

## Pandas,Numpy,Jupyterを使ったデータ処理

2023-10-23 ~ 11-17  

## chap1. はじめに

## cha2. Pythonの基礎、IpythonとJupyter Notebook

IPython  
終了は、``` exit() ```

Jupyter Notebook  
``` jupyter notebook ``` ..起動  
・タブ補完機能  
・イントロスペクション ..?  

## chap3. Python組み込みのデータ構造と関数、ファイルの扱い

シーケンス型オブジェクト

- タプル
- リスト
- ディクショナリ
- セット
- 組み込みのシーケンス関数
  - enumerate関数
  - sorted関数
  - zip関数
  - reversed関数
- リスト、セット、ディクショナリの内包表記

関数

- 無名(ラムダ)関数
- ジェネレータ
- エラーと例外の処理

ファイルとオペレーティングシステム

## chap4. NumPyの基礎：配列とベクトル演算

Numpyの特徴  
巨大なデータ配列を効率的に扱うことができる

numpy.arangeはPythonの組み込みのrange関数のndarray版  

データ型を明示的に型変換(キャスト)するには、NumPyのastypeメソッドを使う

ndarrayでは、要素ごとの処理のためにわざわざforループを書く必要はない  
この機能は、**ベクトル演算**と呼ばれる

ブールインデックス参照

numpy.randomモジュールは、Python標準のrandomモジュールの機能を補完

ndarrayのファイル入出力

## chap5. pandas入門

```python
import numpy as np  
import pandas as pd  
```

SeriesとDataFrame

```python
from pandas import Series, DataFrame
```

## chap6. データの読み込み、書き出しとファイル形式

### テキスト形式のデータの読み書き

```df = pd.read_csv('examples/ex1.csv')```

テキスト形式でのデータの書き出し

```data.to_csv('examples/out.csv')```

バイナリ形式でデータを書きだす（シリアライズする）1つの簡単な方法は、Python組み込みのpickleモジュールを用いること  
pandasオブジェクトはすべて、データをpickle形式でディスクに書き出すto_pickleメソッドを持っている  

### Web APIを用いたデータの取得

```resp = requests.get(url)```

```resp.raise_for_status() # HTTPのエラーレスポンスチェック```

## chap7. データのクリーニングと前処理

欠損値を取り除く  
```dropna()```  

データフレームオブジェクトの場合、dropnaメソッドは欠損値を1つでも含む行をすべて削除する  

欠損値を穴埋めする  
```fillna()```  

重複の除去  
```data.drop_duplicated()```  

離散化とビニング  
```pd.cut()```  
```pd.qcut()```  

## chap8. データラングリング：連結、結合、変形

階層の順序変更やソート  
```frame.swaplevel('key1', 'key2')```  
```frame.sort_index(level=1)```  

階層ごとの要約統計量  
```frame.groupby(level='key2').sum()```  

データフレームの結合  

- merge
- join
- concat
- combine_first

変形とピボット操作  

stack ...データ内の各列を行へとピボット（回転）させる  
unstack ...各行を列へと回転させる  

pivot
melt

## chap9. プロットと可視化

```import matplotlib.pyplot as plt```  
```import seaborn as sns```

```python
fig, ax = plt.subplots()
ax.plot(np.random.standard_normal(1000).cumsum());
ticks = ax.set_xticks([0, 250, 500, 750, 1000])
labels = ax.set_xticklabels(['one', 'two', 'three', 'four', 'five'], rotation=30, fontsize=8)
ax.set_xlabel('Stages')
ax.set_title('My first matplotlib plot')
```

凡例の追加  
label = ''  
.legend()

タイトルの追加  
.set_title('')

プロットのファイルへの保存  

```python
fig.savefig('fig.pdf')
fig.savefig('fig.png', dpi=400)
```

棒グラフ  
data.plot.bar()  
data.plot.barh()  
積み上げ  
(stacked=True)  

順番通りに並び替え  
order = ["Thur", "Fri", "Sat", "Sun"]  
(order=order)  

ヒストグラムと密度プロット  
tips['tip_pct'].plot.hist(bins=50)  
tips['tip_pct'].plot.density()  

散布図  
sns.regplot()  
sns.pairplot()  

ファセットグリッドとカテゴリ型データ  
sns.catplot()  

## chap10. データの集約とグループ操作

```python
grouped = df['data1'].groupby(df['key1'])
grouped
```  

.stack()  
.unstack()

```py
grouped = df.groupby('key1')
grouped['data1'].nsmallest(2)
```

自分自身で定義した集約関数を使う  

```py
def peak_to_peak(arr):
    return arr.max() - arr.min()
grouped.agg(peak_to_peak)
```

```py
grouped.describe()
```

欠損値を平均で埋める  

```py
s.fillna(s.mean())
```

.transform()

.apply()

.pivot_table()

pd.crosstab(data[''], data[''], margins=True)

## chap11. 時系列データ

from datetime import datetime  
from datetime import timedelta  

strptime は文字列を datetime オブジェクトに変換し、strftime は datetime オブジェクトを文字列に変換する

さまざまな種類の日付表現をパースできるメソッド  
pandas.to_datetime()

日付範囲の生成  
index = pd.date_range('2012-04-12', '2012-06-01')

タイムゾーンを扱う  
import pytz

タイムゾーンのローカライゼーションと変換  

ts_utc = ts.tz_localize('UTC')  
ts_utc.tz_convert('America/New_York')

再サンプリングと頻度変換  
ts.resample('M').mean()  

四本値(始値(o)、高値(h)、安値(l)、終値(c))の再サンプリング  
ts.resample('5min').ohlc()  

移動窓関数  
close_px['AAPL'].plot()  
close_px['AAPL'].rolling(250).mean().plot()

指数加重関数  
ewma30 = aapl_px.ewm(span=30).mean()

2つの時系列に対する移動窓関数  
corr = returns['AAPL'].rolling(125, min_periods=100).corr(spx_rets)
corr.plot()

## chap12. Pythonにおけるモデリングライブラリ入門

Patsy  
statsmodels  
scikit-learn  

pandasと分析ライブラリとの接点は、通常はNumPy配列  
データフレームをNumPy配列に変換するには、to_numpyメソッドを使う

## chap13. データ分析の実例

## 付録A NumPy:応用編

平坦化(flatten)とレイベリング(ravel)  
arr.ravel()  

配列を連結するためのヘルパー関数：r_とc_  
np.r_[arr1, arr2]  
np.c_[arr1, arr2]  

## 付録B IPythonシステム： 上級編

マジックコマンドについて  
マジックコマンドは、パーセント記号％が冒頭につく  
例えば、Pythonの処理時間を確認するのに、%timeitマジック関数が使える  

マジックコマンドのオプションは?を使って確認できる  

%runコマンド  
%run script.py  

%load script.py  
