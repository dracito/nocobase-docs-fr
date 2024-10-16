# 承認ノード

承認ワークフローでは、専用の「承認」ノードを使用して、承認者が発起された承認を処理するための操作ロジック（承認、拒否、または返却）を構成する必要があります。「承認」ノードは承認プロセスでのみ使用可能です。

:::info{title=ヒント}
通常の「手動処理」ノードとの違い：通常の「手動処理」ノードは、より一般的なシナリオに対応しており、さまざまなタイプのワークフローにおける手動入力データや手動意思決定プロセスの継続などに使用されます。一方、「承認ノード」は承認プロセス専用の特化した処理ノードであり、他のワークフローでは使用できません。
:::

## ノードの作成

フロー内のプラス（“+”）ボタンをクリックして「承認」ノードを追加し、その後、通過モードのいずれかを選択して承認ノードを作成します：

![承認ノード_作成](https://static-docs.nocobase.com/f15d61208a3918d005cd2031fc9b6ce7.png)

## 通過モード

通過モードには2種類があります：

1. **直通モード**：通常、比較的単純なプロセスに使用され、承認ノードの通過可否はプロセスの終了を決定します。未通過の場合はプロセスを直接終了します。

    ![承認ノード_通過モード_直通モード](https://static-docs.nocobase.com/a9d446a186f61c546607cf1c2534b287.png)

2. **分岐モード**：通常、より複雑なデータロジックに使用され、承認ノードがいかなる結果を出した後も、その結果の分岐内で他のノードを実行し続けることができます。

    ![承認ノード_通過モード_分岐モード](https://static-docs.nocobase.com/57dc6a8907f3bb02fb28c354c241e4e5.png)

    その際、ノードが「返却」操作を設定している場合にのみ「返却」分岐が生成され、返却分岐の実行が完了した後は現在のプロセスを強制的に終了します。

    このノードが「通過」された後、通過分岐を実行するだけでなく、後続のプロセスも引き続き実行されます。「拒否」操作の後もデフォルトで後続のプロセスを続行可能で、ノード内で実行分岐を設定することでプロセスを終了することも可能です。

:::info{title=ヒント}
通過モードはノード作成後に変更できません。
:::

## 承認者

承認者は、このノードの承認行為を担当するユーザーの集合であり、一人または複数のユーザーを指定できます。選択肢のソースは、ユーザーリストから選択する静的値か、変数によって指定される動的値のいずれかです。

![承認ノード_承認者](https://static-docs.nocobase.com/29c64297d577b9ca9457b1d7ac62287d.png)

変数を選択する際は、コンテキストとノード結果内のユーザーデータの主キーまたは外部キーのみを選択できます。選択した変数が実行中に配列（多対多の関係）である場合、配列内の各ユーザーは全体の承認者集合に統合されます。

## 協議モード

最終実行時に承認者が一人だけの場合（複数の変数の重複を除いた場合）、選択した協議モードに関わらず、そのユーザーだけが承認操作を実行し、結果もそのユーザーによって決定されます。

承認者集合に複数のユーザーがいる場合、異なる協議モードを選択することは異なる処理方法を意味します：

1. **いずれか承認**：いずれか一人が承認すればノードが通過し、全員が拒否した場合のみノードが拒否されます。
2. **会議承認**：全員が承認しなければノードが通過せず、いずれか一人が拒否すればノードが拒否されます。
3. **投票**：設定された割合を超える人数が承認しなければノードが通過せず、そうでなければノードが拒否されます。

戻し操作については、どのモードでも承認者集合にユーザーが戻しとして処理した場合、ノードは直接プロセスを終了します。

## 処理順序

同様に、承認者集合に複数のユーザーがいる場合、異なる処理順序を選択することは異なる処理方法を意味します：

1. **並行**：全ての承認者は任意の順序で処理でき、先後の処理は関係ありません。
2. **順次**：承認者は承認者集合の順序に従って処理し、前の承認者が提出した後でなければ次の承認者は処理できません。

「順次」処理に設定されているかどうかに関わらず、実際の処理の先後に基づいて生じた結果も上記の「協議モード」のルールに従い、対応する条件を満たすとそのノードは実行を完了します。

## 拒否分岐後のワークフローの終了

「通過モード」が「分岐モード」に設定されている場合、拒否分岐の終了後にワークフローを終了することを選択できます。これにチェックを入れると、拒否分岐の末尾に「✗」が表示され、その分岐の終了後は後続のノードが続かないことを示します：

![承認ノード_拒否後退出](https://static-docs.nocobase.com/1e740df93c128fb6fe54bf85a740e683.png)

## 承認者インターフェースの設定

承認者インターフェースの設定は、承認者が承認ワークフローをこのノードで実行する際の操作インターフェースを提供します。設定ボタンをクリックしてポップアップを開きます：

![承認ノード_インターフェース設定_ポップアップ](https://static-docs.nocobase.com/2c321ae164b436f1c572305ff27cc9dd.png)

設定ポップアップでは、承認提出の詳細、操作バー、カスタムメッセージなどのブロックを追加できます：

![承認ノード_インターフェース設定_ブロック追加](https://static-docs.nocobase.com/9f8f11926e935ad8f8fbeec368edebfe.png)

承認内容の詳細ブロックは、発起人が提出したデータブロックであり、通常のデータブロックに似ています。任意でデータテーブルのフィールドコンポーネントを追加し、承認者が確認する必要がある内容を整理するために自由に配置できます。

![承認ノード_インターフェース設定_詳細ブロック](https://static-docs.nocobase.com/1140ec13caeea1b364d12e057720a29c.png)

操作バーには、このノードがサポートする操作ボタンを追加できます。例えば、「承認」、「拒否」、「返却」などがあります。

![承認ノード_インターフェース設定_操作バー](https://static-docs.nocobase.com/1bb090ed123f62ba8a524a3e9e7da328.png)

さらに、操作バーには承認者に記入を求める関連フィールドを追加することもできます。例えば、「コメント」フィールドです。

:::warning{title=重要}
特定の操作ボタンを有効または無効にした場合、操作インターフェース設定のポップアップを閉じた後に、このノードの設定を保存する必要があります。そうしないと、その操作ボタンの変更は反映されません。
:::
