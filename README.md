# Binary Classification with a Bank Dataset
(銀行データセットを用いた二値分類)

>## 概要

### 目標
目標は、顧客が銀行の定期預金に加入するかどうかを予測すること

### 評価
提出されたデータは、予測値と観測された目標値との間の ROC曲線下面積（AUC）を用いて評価されます。


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

[train.csv](https://www.kaggle.com/competitions/sch2-reg-2026-d4-2/data?select=train.csv)

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
<br>
<br>

>## 環境構築(windows)
```
--Binary-Classification-with-a-Bank-Dataset/ -- 

data/
    |____sample_submission.csv 
    |____test.csv 
    |____train.csv

```

```PowerShell
py -m venv .venv
```
<br>

```PowerShell
.\.venv\Scripts\Activate.ps1
```
<br>

```PowerShell
pip install -r requirements.txt
```

<br>

```PowerShell
jupyter lab
```

<br>

>## 記録
>
V0.0 roc-auc :  0.9610978699150123　1-16全部乗せ<br>
V0.1 roc-auc :  0.7164416803000848  1,2,4,13,16のみ<br>