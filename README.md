# Binary Classification with a Bank Dataset
(銀行データセットを用いた二値分類)

># 概要

### 目標
目標は、顧客が銀行の定期預金に加入するかどうかを予測すること

### 評価
提出されたデータは、予測値と観測された目標値との間の ROC曲線下面積（AUC）を用いて評価されます。

[機械学習における評価プロセス（ROC-AUC）のまとめ](https://github.com/kenkenkengo0421/Binary-Classification-with-a-Bank-Dataset/blob/main/memo.md)

### 提出
テストセットの各項目についてid、バイナリターゲットの確率を予測する必要がありますy。ファイルにはヘッダーが含まれ、以下の形式である必要があります。

```csv
id,y
750000,0.5
750001,0.5
750002,0.5
etc.
```

### データ項目
過去の定期預金キャンペーンデータ<br>



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

[train.csv  kaggle公式分析ページ](https://www.kaggle.com/competitions/sch2-reg-2026-d4-2/data?select=train.csv)


[特徴量一覧(columns)](https://github.com/kenkenkengo0421/Binary-Classification-with-a-Bank-Dataset/blob/main/columns.md)



<br>
<br>
<br>

># 環境構築(windows)

Binary-Classification-with-a-Bank-Dataset/で実行 

zip解凍
```PowerShell
 Expand-Archive -Path .\data.zip
```

<br>

解凍後の中身移動
```PowerShell
mv .\data\data\* .\data\
```

<br>

いらんフォルダ消す
```PowerShell
rm -r -fo .\data\data
```

<br>

venv構築
```PowerShell
py -m venv .venv
```
<br>

venv有効化
```PowerShell
.\.venv\Scripts\Activate.ps1
```
<br>

必要pipinstall
```PowerShell
pip install -r requirements.txt
```

<br>

nb起動
```PowerShell
jupyter lab
```

<br>

># 記録

```

V0.0
roc-auc :0.9610978699150123　1-16全部乗せ

----

V0.1
roc-auc :0.7164416803000848  1,2,4,13,16のみ
kaggleスコア :0.71797

----

V0.2
roc-auc :0.7515724085199316  5,6,7,8のみ
kaggleスコア :0.74812

----

V0.3
roc-auc :  0.841403093714189 12 durationのみ削除
kaggleスコア :0.84026

----

V0.4
roc-auc :  0.8418812223868644 1-16, 12削除
kaggleスコア :0.84043

追加column

17) total_loan_count：int64　housingとloanの「yes」の合計数（負債の重さ）
18) balance_per_age：float64　年齢に対する残高の比率
19) is_new_prospect：int64　previousが0かつpdaysが-1の完全新規フラグ。
20) day_month_interaction：int64　月と日の組み合わせによるボーナス・決算期特定

----

V0.5
roc-auc :  0.8421719373317439  1-16, 12削除,
kaggleスコア :0.84065

17) total_loan_count：int64　housingとloanの「yes」の合計数（負債の重さ）。
18) balance_per_age：float64　年齢に対する残高の比率
19) is_new_prospect：int64　previousが0かつpdaysが-1の完全新規フラグ。
20) day_month_interaction：int64　月と日の組み合わせによるボーナス・決算期特定

追加column

21) pdays_is_contacted： 過去に一度でも接触したことがあるか
22) balance_is_negative： 残高がマイナス（借金状態）かどうか
23) job_category_quality： 安定収入層（management, technician, admin.）判定
24) is_peak_campaign_month： キャンペーン集中月（5, 6, 7, 8月など）判定
25) age_bin_senior： 60歳以上の高齢層（退職金期待層）判定

----

V0.6
roc-auc :  0.8524207108450856  1-16, 12削除,
kaggleスコア :0.85180

17) total_loan_count：int64　housingとloanの「yes」の合計数（負債の重さ）。
18) balance_per_age：float64　年齢に対する残高の比率
19) is_new_prospect：int64　previousが0かつpdaysが-1の完全新規フラグ。
20) day_month_interaction：int64　月と日の組み合わせによるボーナス・決算期特定
21) pdays_is_contacted： 過去に一度でも接触したことがあるか
22) balance_is_negative： 残高がマイナス（借金状態）かどうか
23) job_category_quality： 安定収入層（management, technician, admin.）判定
24) is_peak_campaign_month： キャンペーン集中月（5, 6, 7, 8月など）判定
25) age_bin_senior： 60歳以上の高齢層（退職金期待層）判定

追加column

26) is_student: 職業が学生であるかどうか
27) is_telephone_contact: 連絡手段が固定電話であるかどうか（特定の生活様式や年齢層の絞り込み）
28) poutcome_success_flag: 前回のキャンペーン成果が「成功」であったか
29) poutcome_failure_flag: 前回のキャンペーン成果が「失敗」であったか
30) age_bin_young: 25歳未満の若年層であるか

LightGBMの学習回数を100 => 500に変更

----

```