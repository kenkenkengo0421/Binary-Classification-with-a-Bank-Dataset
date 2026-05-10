# column
<br>

## １）標準

```

id       :int64	    各行を識別する一意の番号（学習には不要）。
------------------
1) age      :int64	    顧客の年齢。 
2) job      :object	職業（admin, blue-collar, technician など）。　
3) marital  :object	既婚・未婚などの婚姻状況。　
4) education:object	学歴（primary, secondary, tertiary など）
                    ※primary: 初等教育（小学校など）
                     secondary: 中等教育（中学校・高校など）
                     tertiary: 高等教育（大学・大学院など）
                     unknown: 不明
5) default  :object	債務不履行があるかどうか（yes / no）　過去にローンやクレジットカードの支払いが滞ったことがあるか
6) balance  :int64	    年間の平均残高。
7) housing  :object	住宅ローンがあるかどうか（yes / no）。
8) loan     :object	個人ローンがあるかどうか（yes / no）
9) contact  :object	連絡手段（cellular（スマホ）, telephone （固定）など）。
10) day      :int64	    最終接触日の「日」。
11) month    :object	最終接触日の「月」。
12) duration :int64	    最後の電話営業通話時間（秒）。※予測時には注意が必要
13) campaign :int64     このキャンペーン（勧誘）期間中の接触回数。
14) pdays    :int64	    前回のキャンペーン（勧誘）接触から経過した日数（-1は未接触）。
15) previous :int64	    このキャンペーン（勧誘）以前の接触回数。
16) poutcome :object	前回のキャンペーン（勧誘）の成果（success(契約してくれた), failure(断られた) など）。
------------------
y:int64	            目的変数。 定期預金を申し込んだかどうか（1: はい、0: いいえ）。

```

<br>

## ２）追加分

> ### 17) total_loan_count：int64　housingとloanの「yes」の合計数（負債の重さ）。<br>
0:借りていない<br>
1:個人ローン or 住宅ローンどちらかを借りている<br>
2:両方とも借りている<br>
```py
df["total_loan_count"] = (df['housing'] == 'yes').astype(int) + (df['loan'] == 'yes').astype(int) 

total_loan_count
1    384545
0    299595
2     65860
Name: count, dtype: int64
```
---


> ### 18) balance_per_age：float64　年齢に対する残高の比率<br>
1歳あたりいくら貯金しているか? 20歳の100万円と60歳の100万円を、モデルが同じ「100万円」として扱わないようにする<br>
1:「35.1（上位25%）」より高ければ<br>
0:それ以外<br>

```py
df['balance_per_age'] = (df['balance'] / df['age'] > 35.1).astype(int)

#四分位
count    750000.000000
mean          0.250255
std           0.433160
min           0.000000
25%           0.000000
50%           0.000000
75%           1.000000
max           1.000000
Name: balance_per_age, dtype: float64


balance_per_age
0    562309
1    187691
Name: count, dtype: int64

```
---

>### 19) is_new_prospect：int64　previousが0かつpdaysが-1の完全新規フラグ。

本当の意味での新規客 previous == 
<br>
0：今回のキャンペーンより前に、一度も接触がない。<br>
1：過去に接触した記録（日数）が全く存在しない<br>

```py
df['is_new_prospect'] = ((df['previous'] == 0) & (df['pdays'] == -1)).astype(int)

is_new_prospect
1    672417
0     77583
Name: count, dtype: int64

```

---


>### 20) day_month_interaction：int64　月と日の組み合わせによるボーナス・決算期特定<br>
四半期末かつ20日以降


```py
df["day_month_interaction"] = ((df['month'].isin(['mar', 'jun', 'sep', 'dec'])) & (df['day'] >= 20)).astype(int)

day_month_interaction
0    729495
1     20505
Name: count, dtype: int64
```
---


### 21) pdays_is_contacted： 過去に一度でも接触したことがあるか<br>
過去に一度でも接触したことがあるかないか<br>
1:接触済み<br>
0:未接触<br>

```py
df['pdays_is_contacted'] = (df['pdays'] != -1).astype(int)

pdays_is_contacted
0    672434
1     77566
Name: count, dtype: int64
```
---


### 22) balance_is_negative： 残高がマイナス（借金状態）かどうか<br>
残高がマイナスかどうか<br>
1:マイナス<br>
0:マイナスじゃない<br>

```py
df['balance_is_negative'] = (df['balance'] < 0).astype(int)

balance_is_negative
0    645355
1    104645
Name: count, dtype: int64
```
---
### 23) job_category_quality： 安定収入層（management, technician, admin.）判定

安定収入層かどうか？<br>
1: jobが<br>
'management'：（経営・管理職）,<br>
'technician'：（技術職）,<br>
'admin.'：（事務職）<br>
の場合<br>
0: それ以外<br>
```py
df['job_category_quality'] = df['job'].isin(['management', 'technician', 'admin.']).astype(int)

job_category_quality
1    395140
0    354860
Name: count, dtype: int64
```
---

### 24) is_peak_campaign_month： キャンペーン集中月（5, 6, 7, 8月など）判定<br>
キャンペーン集中月（5,6,7,8月）か<br>
1：はい<br>
0：いいえ<br>

```py
df['is_peak_campaign_month'] = df['month'].isin(['may', 'jun', 'jul', 'aug']).astype(int)

is_peak_campaign_month
1    561587
0    188413
Name: count, dtype: int64
```
---

### 25) age_bin_senior： 60歳以上の高齢層（退職金期待層）判定
60歳以上の高齢層か<br>
1：はい<br>
0：いいえ<br>
```py
df['age_bin_senior'] = (df['age'] >= 60).astype(int)

age_bin_senior
0    726431
1     23569
Name: count, dtype: int64
```
---