---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

Copilot cloud agent の演習を始める前に、必要な環境を準備します。Tailspin Toys リポジトリのコピーを作成し、[codespace][codespaces] を起動します。この環境で指示ファイルを編集し、cloud agent が作成した内容をレビューします。

## 演習用リポジトリを設定する

コードを作成するためのリポジトリのコピーを用意するため、[テンプレート][template-repository]から新しいインスタンスを作成します。新しいリポジトリには演習に必要なファイルがすべて含まれ、各演習で使用します。

1. 新しいブラウザー ウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. 演習用リポジトリのページで **Use this template** を選択し、次に **Create a new repository** を選択して自分用のコピーを作成します。

    ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot にアクセスできる組織に新しいリポジトリを作成します。

    ![リポジトリ テンプレートの設定](../../../_images/ex0-repository-settings.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) をメモします。演習中にこのパスを参照します。

## codespace を作成する

続いて、codespace で演習を進めます。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードの作成、実行、デバッグができるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールに対応した、フル機能の統合開発環境 (IDE) を提供します。

1. 作成したリポジトリを開きます。
2. **Code** ボタンを選択します。

    ![Code ボタン](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選択し、**+** ボタンを選択して新しい codespace を作成します。

    ![新しい codespace を作成](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかりますが、すべてのサービスを手動でインストールするよりもはるかに速く準備できます。待っている間に GitHub Copilot のほかの機能を確認してもかまいません。次にそれらを取り上げます。

> [!CAUTION]
> 次の演習で codespace に戻ります。今はブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの[開発コンテナー][dev-containers]で実行するように設計されています。どちらの環境にも、スムーズに進めるために必要な前提条件がインストールされています。ローカルで進める場合は、クローンしたリポジトリを VS Code で開き、メッセージが表示されたら **Reopen in Container** を選択します。VS Code が codespace と同じ開発コンテナーを構築します。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## まとめ

演習用リポジトリのコピーを作成し、codespace の作成を開始しました。この環境で Copilot cloud agent と一緒に作業します。

## 次のステップ

cloud agent が従うカスタム指示を追加します。[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces を使い始める][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[next-lesson]: ../1-custom-instructions/
