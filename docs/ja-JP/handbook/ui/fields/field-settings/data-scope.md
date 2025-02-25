# データ範囲の設定

## 概要

リレーションフィールドのデータ範囲設定は、ブロックのデータ範囲設定に類似しており、リレーションデータに対してデフォルトのフィルタ条件を設定します。

## 使用方法

![20240422153711](https://static-docs.nocobase.com/20240422153711.png)

### 静的値

例：販売中の商品だけを関連付けることができます。

![20240422155953](https://static-docs.nocobase.com/20240422155953.png)

### 変数値

例：生産日が先月以前の商品だけを関連付けることができます。

![20240422163640](https://static-docs.nocobase.com/20240422163640.png)

変数の詳細については、[変数](/handbook/ui/variables)をご参照ください。

### リレーションフィールドの連動

リレーションフィールド間は、データ範囲を設定することで連動を実現します。

例：注文表には多対多のリレーションフィールド「商品」と多対一のリレーションフィールド「顧客」があり、商品表には多対多のリレーションフィールド「顧客」が存在します。注文表のブロック内では、選択可能な商品は現在のフォームで選択された顧客に関連する商品となります。

![20240422154145](https://static-docs.nocobase.com/20240422154145.png)

<video width="100%" height="440" controls>
      <source src="https://static-docs.nocobase.com/20240422155351.mp4" type="video/mp4">
</video>

