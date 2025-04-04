# 変数

## 概要
変数は、現在のコンテキスト内の特定の値を識別するためのトークンのセットであり、設定ブロックのデータ範囲、フィールドのデフォルト値、連動ルール、ワークフローなどのシナリオで使用されます。

## 現在サポートされている変数

### 現在のユーザー
現在ログインしているユーザーのデータを示します。

![20240416154950](https://static-docs.nocobase.com/20240416154950.png)

### 現在の役割
現在ログインしているユーザーの役割識別子（役割名）を示します。

![20240416155100](https://static-docs.nocobase.com/20240416155100.png)

### 現在のフォーム
現在のフォームの値で、フォームブロック専用です。使用シナリオには以下が含まれます：

- 現在のフォームの連動ルール
- フォームフィールドのデフォルト値（新規データ追加時のみ有効）
- 関係フィールドのデータ範囲設定
- 提出操作のフィールド値設定

#### 現在のフォームの連動ルール
![20240416170732_rec_](https://static-docs.nocobase.com/20240416170732_rec_.gif)

#### フォームフィールドのデフォルト値（新規データ追加時のみ有効）
![20240416171129_rec_](https://static-docs.nocobase.com/20240416171129_rec_.gif)

#### 関係フィールドのデータ範囲設定
関係間の連動を処理するために使用されます。例えば：
![20240416171743_rec_](https://static-docs.nocobase.com/20240416171743_rec_.gif)

#### 提出操作のフィールド値設定
![20240416171215_rec_](https://static-docs.nocobase.com/20240416171215_rec_.gif)

### 現在のオブジェクト
現在は、フォームブロックのサブフォームおよびサブテーブルのフィールド設定のみに使用され、各項目の値を示します：

- サブフィールドのデフォルト値
- サブ関係フィールドのデータ範囲

#### サブフィールドのデフォルト値
![20240416172933_rec_](https://static-docs.nocobase.com/20240416172933_rec_.gif)

#### サブ関係フィールドのデータ範囲
![20240416173043_rec_](https://static-docs.nocobase.com/20240416173043_rec_.gif)

### 上级对象

与「当前对象」类似，表示当前对象的父级对象。在 NocoBase v1.3.34-beta 及以上版本中支持。

### 現在のレコード
レコードとは、データテーブルの行を指し、各行は一つのレコードを表します。表示系ブロックの**行操作の連動ルール**には「現在のレコード」変数が含まれています。

#### 行操作の連動ルール
![20240416174813_rec_](https://static-docs.nocobase.com/20240416174813_rec_.gif)

### 現在のポップアップレコード
ポップアップ操作は、NocoBaseのインターフェース設定において非常に重要な役割を果たします。

- 行操作のポップアップ：各ポップアップには「現在のポップアップレコード」変数があり、現在の行レコードを示します。
- 関係フィールドのポップアップ：各ポップアップには「現在のポップアップレコード」変数があり、現在クリックされた関係レコードを示します。

ポップアップ内のブロックは「現在のポップアップレコード」変数を使用できます。関連する使用シーンは以下の通りです：

- ブロックのデータ範囲の設定
- 関係フィールドのデータ範囲の設定
- フィールドのデフォルト値の設定（新規データのフォーム）
- 操作の連動ルールの設定
- フォーム送信操作のフィールド値設定

#### ブロックのデータ範囲の設定
![20240416224307_rec_](https://static-docs.nocobase.com/20240416224307_rec_.gif)

#### 関係フィールドのデータ範囲の設定
![20240416224641_rec_](https://static-docs.nocobase.com/20240416224641_rec_.gif)

#### フィールドのデフォルト値の設定（新規データのフォーム）
![20240416223846_rec_](https://static-docs.nocobase.com/20240416223846_rec_.gif)

#### 操作の連動ルールの設定
![20240416223101_rec_](https://static-docs.nocobase.com/20240416223101_rec_.gif)

#### フォーム送信操作のフィールド値設定
![20240416224014_rec_](https://static-docs.nocobase.com/20240416224014_rec_.gif)

### テーブル選択レコード
現在のみ、表格区画の「Add record」操作のフォームフィールドのデフォルト値です。

#### 「Add record」操作のフォームフィールドのデフォルト値

### 親レコード（廃止）
関係区画内でのみ使用され、関係データのソースレコードを示します。

:::warning
「親レコード」は廃止されましたので、同等の「現在のポップアップレコード」を使用することをお勧めします。
:::

### 日付変数
関連変数は以下の通りです：

- 現在の時刻
- 昨日
- 今日
- 明日
- 先週
- 今週
- 来週
- 先月
- 今月
- 来月
- 先四半期
- 今四半期
- 来四半期
- 昨年
- 今年
- 来年
- 過去7日間
- 次の7日間
- 過去30日間
- 次の30日間
- 過去90日間
- 次の90日間

<br />

:::warning
現在の時刻（Current time）は時刻（文字列）ですが、それ以外のすべての日時変数は期間（配列）であり、現在の期間はデータ範囲でのみ使用可能で、フィールドのデフォルト値には使用できません。
:::

関連する使用シーンは以下の通りです：

- 区画データ範囲の日付フィールド条件設定
- 関係フィールドのデータ範囲の日付フィールド条件設定
- 操作連動ルールの日付フィールド条件設定
- 日付フィールドのデフォルト値設定

### URL クエリパラメータ
この変数は現在のページのURL中のクエリパラメータを示し、ページのURLにクエリ文字列が存在する場合のみ使用可能です。[リンク操作](/handbook/ui/actions/types/link)と合わせて使用することで、さらに便利になります。

![20240603200410](https://static-docs.nocobase.com/20240603200410.gif)

### API トークン
この変数の値は文字列で、NocoBase APIにアクセスするための認証情報です。ユーザーの識別に利用できます。

