---
name: git-commit
description: 実際のdiffを確認したうえで、Chris Beamsの標準的なルール（命令形・50文字以内のsubject、必要な時だけbody）に沿った英語のコミットメッセージを作成し、確認を取ってからコミットする。pushは行わない（コミットまでがこのスキルの範囲）。ユーザーがコミットメッセージの作成を求めた時、「コミットして」と言った時、または変更がコミット可能な状態にある時に使う。会話の内容だけで推測せず、必ず実際のdiffを見て書く。プロジェクト固有のルール（AGENTS.mdなど）がある場合はそちらを優先する。
---

# git-commit

ベースはChris Beamsの「7つのルール」([cbea.ms/git-commit](https://cbea.ms/git-commit/))。ただしfooterは使わない。

このスキルはcommitまでを担当する。pushは行わない — pushはcommitより取り消しにくく、リモートや他の開発者・CIに影響する別種の操作なので、意図的に範囲外にしている。pushが必要な場合はユーザーに確認してから別途行うこと。

## 手順

1. **プロジェクト固有のルールを確認する**

   コミットメッセージの規約（`type(scope):`のようなConventional Commits形式にするか、bodyを常に付けるか付けないか、footerでIssueを閉じるかなど）はプロジェクトによって違う。まずAGENTS.md・CLAUDE.mdなど、リポジトリ内の規約ファイルを確認する。書かれていればそちらに従い、以降のステップはこのファイルのルールで上書きする。何も書かれていなければ、以下をデフォルトのルールとして使う。

2. **実際の変更を確認する**

   ```bash
   git status
   git diff
   ```

3. **明示的にstageする**

   ```bash
   git add <file1> <file2> ...
   ```

   `git add -A` は使わない。この変更に関係するファイルだけを選んで、無関係な変更が紛れ込まないようにする。

4. **1コミット=1論理変更。** diffに無関係な変更が複数混ざっている場合は、まとめて1つのメッセージにせず、ファイルを分けてそれぞれコミットする。

5. **subjectを書く（デフォルトルール）**

   - 命令形（英語）: "Add", "Fix", "Update" など（"Added", "Adds" ではない）
   - 大文字始まり
   - 末尾にピリオドを付けない
   - 50文字以内
   - typeプレフィックス（`feat:`など）は、プロジェクトの規約で指定されていない限り付けない

6. **bodyは必要な時だけ（デフォルトルール）**

   変更が自明でない場合（なぜその変更が必要だったか、背景を知らないと分からない場合）は、subjectと空行を挟んでbodyを書く。72文字で折り返し、「何を」「なぜ」を書く（「どうやって」はdiffが語るので書かない）。typoの修正や1行だけの自明な変更ならbodyは省略する。

7. **footerは付けない（デフォルトルール）**

   `Co-Authored-By`、`Closes #123`、`BREAKING CHANGE`などは、プロジェクトの規約で指定されていない限り書かない。

8. **確認してからコミット**

   メッセージをユーザーに見せる。OKが出たら:

   ```bash
   git commit -m "Subject here"
   ```

   bodyがある場合:

   ```bash
   git commit -m "Subject here" -m "Body here"
   ```

9. **pushはしない**

   コミットが終わったらそこで完了。リモートへの反映が必要な場合は、その旨をユーザーに伝え、別途明示的な指示を待つ。

## 例（デフォルトルールに従う場合）

**body不要な例:**
- `Fix BM25 tokenization`
- `Remove unused embedding cache logic`

**bodyが必要な例:**
```
Switch retriever default to dense embeddings

BM25 was missing semantically similar but lexically
different queries. Dense retrieval handles this better
for the current dataset.
```
