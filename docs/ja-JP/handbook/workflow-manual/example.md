## 記事審査

一般ユーザーが提出した記事は、管理者の承認を受けた後に公開状態に更新されます。承認が拒否された場合、記事は草稿状態（非公開）を維持します。このプロセスは、手動ノードの更新フォームを使用して実現できます。

「新規記事の追加」をトリガーとするワークフローを作成し、手動ノードを追加します：

<figure>
  <img alt="手動ノード_例_記事審査_フロー編成" src="https://github.com/nocobase/nocobase/assets/525658/2021bf42-f372-4f69-9c84-5a786c061e0e" width="360" />
</figure>

手動ノードでは、責任者を管理者に設定し、トリガーデータに基づいて新規記事の詳細を表示するブロックを追加します：

<figure>
  <img alt="手動ノード_例_記事審査_ノード設定_詳細ブロック" src="https://github.com/nocobase/nocobase/assets/525658/c61d0aac-23cb-4387-b60e-ce3cc7bf1c24" width="680" />
</figure>

設定画面では、更新データフォームに基づくブロックを追加し、記事テーブルを選択して管理者が承認するかどうかを決定します。承認後は、後の設定に従って対応する記事が更新されます。フォームを追加すると、デフォルトで「プロセスを続ける」ボタンが表示され、これをクリックすると承認されたと見なされます。また、「プロセスを終了する」ボタンを追加し、承認が拒否された場合に使用します：

<figure>
  <img alt="手動ノード_例_記事審査_ノード設定_フォームと操作" src="https://github.com/nocobase/nocobase/assets/525658/4baaf41e-3d81-4ee8-a038-29db05e0d99f" width="673" />
</figure>

プロセスを続ける際には、記事の状態を更新する必要があります。これには二つの設定方法があります。一つは、フォーム内に記事の状態フィールドを直接表示し、操作担当者が選択できるようにする方法です。この方法は、フィードバックなどを積極的にフォームに記入する必要がある場合に適しています。

<figure>
  <img alt="手動ノード_例_記事レビュー_ノード設定_フォームフィールド" src="https://github.com/nocobase/nocobase/assets/525658/82ea4e0e-25fc-4921-841e-e1a2782a87d1" width="668" />
</figure>

操作を簡略化するために、「プロセスを続行」ボタンにフォームの値を設定し、「状態」フィールドに「公開済み」という値を追加します。これにより、オペレーターがボタンをクリックすると、記事が公開済みの状態に更新されます。

<figure>
  <img alt="手動ノード_例_記事レビュー_ノード設定_フォームの値" src="https://github.com/nocobase/nocobase/assets/525658/0340bd9f-8323-4e4f-bc5a-8f81be3d6736" width="711" />
</figure>

次に、フォームブロックの右上隅にある設定メニューから、更新するデータのフィルタ条件を選択します。ここでは「記事」テーブルを選び、フィルタ条件を「ID が トリガー変数 / トリガーデータ / ID に等しい」とします。

<figure>
  <img alt="手動ノード_例_記事レビュー_ノード設定_フォーム条件" src="https://github.com/nocobase/nocobase/assets/525658/da004055-0262-49ae-88dd-3844f3c92e28" width="1020" />
</figure>

最後に、各ブロックのタイトルや関連ボタンのテキスト、フォームフィールドのヒントテキストを変更して、インターフェースをより親しみやすくします。

<figure>
  <img alt="手動ノード_例_記事レビュー_ノード設定_最終フォーム" src="https://github.com/nocobase/nocobase/assets/525658/21db5f6b-690b-49c1-8259-4f7b8874620d" width="678" />
</figure>

設定パネルを閉じ、送信ボタンをクリックしてノード設定を保存すると、ワークフローが設定完了となります。このワークフローを有効にすると、新しい記事を追加する際に自動的にトリガーされ、管理者は待機タスクリストからこのワークフローの処理が必要であることを確認でき、クリックして待機タスクの詳細を確認できます。

<figure>
  <img alt="手動ノード_例_記事レビュー_タスク一覧" src="https://github.com/nocobase/nocobase/assets/525658/4e1748cd-6a07-4045-82e5-286826607826" width="1363" />
</figure>

<figure>
  <img alt="手動ノード_例_記事レビュー_タスク詳細" src="https://github.com/nocobase/nocobase/assets/525658/65f01fb1-8cb0-45f8-ac21-ec8ab59be449" width="680" />
</figure>

管理者は記事の詳細に基づいて、記事が公開可能かどうかを判断します。公開可能な場合は「承認」ボタンをクリックすると、記事は公開状態に更新されます。公開できない場合は「拒否」ボタンをクリックすると、記事は下書き状態のままとなります。

## 休暇承認

従業員が休暇を取得する必要がある場合、上司の承認を得て初めて有効となり、該当する従業員の休暇データが消化されます。承認されたか拒否されたかにかかわらず、リクエストノードを通じてSMSインターフェースを呼び出し、従業員に関連する通知SMSが送信されます（詳細は[HTTPリクエスト](#_HTTP_リクエスト)のセクションを参照）。このシナリオでは、手動ノード内のカスタムフォームを使用して実現できます。

「新規休暇申請」をトリガーとするワークフローを作成し、手動ノードを追加します。これは以前の文章レビューのプロセスと似ていますが、ここでの責任者は上司です。トリガーデータに基づくブロックを追加して新規休暇申請の詳細を表示し、さらに上司が承認するかどうかを決定するためのカスタムフォームに基づくブロックを追加します。カスタムフォームには、承認の有無を示すフィールドと、拒否理由のフィールドを追加します。

<figure>
  <img alt="手動ノード_例_休暇承認_ノード設定" src="https://github.com/nocobase/nocobase/assets/525658/ef3bc7b8-56e8-4a4e-826e-ffd0b547d1b6" width="675" />
</figure>

記事レビューのプロセスとは異なり、上司の承認結果に基づいて後続のプロセスを進める必要があるため、ここでは「プロセスを続行」ボタンのみを設定し、提出に使用します。「プロセスを終了」ボタンは使用しません。

手動ノードの後に条件判断ノードを追加することで、上司が休暇申請を承認したかどうかを判断できます。承認された場合の分岐には、休暇の取り消しデータ処理を追加し、その後にリクエストノードを配置して、従業員にSMS通知を送信します。これにより、以下の完全なプロセスが得られます：

<figure>
  <img alt="手動ノード_例_休暇承認_プロセス編成" src="https://github.com/nocobase/nocobase/assets/525658/733f68da-e44f-4172-9772-a287ac2724f2" width="593" />
</figure>

条件判断ノードの条件設定は、「手動ノード / カスタムフォームデータ / 承認フィールドの値が‘承認’であるかどうか」となります：

<figure>
  <img alt="手動ノード_例_休暇承認_条件判断" src="https://github.com/nocobase/nocobase/assets/525658/57b972f0-a3ce-4f33-8d40-4272ad205c20" width="678" />
</figure>

リクエストノードで使用するデータは、手動ノード内の対応するフォーム変数を利用して、承認と拒否のSMS内容を区別できます。これにより、全体のプロセス設定が完了し、ワークフローを開始すると、従業員が休暇申請フォームを提出した後、上司は保留中のタスクで承認処理を行うことができ、操作は基本的に記事レビューのプロセスと類似しています。

