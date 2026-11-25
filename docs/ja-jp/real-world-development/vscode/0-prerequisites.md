---
title: "演習 0: 前提条件"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

VS Code の演習を始める前に、必要なものを準備します。Tailspin Toys リポジトリのコピーを作成し、作業用の [codespace][codespaces] を起動して、エディターで GitHub Copilot Chat が動作することを確認します。

## 演習用リポジトリを設定する

コードを書くためのリポジトリのコピーを、[テンプレート][template-repository]から作成します。新しいリポジトリには演習に必要なファイルがすべて含まれます。

1. 新しいブラウザーウィンドウで [Tailspin Toys テンプレート][tailspin-template]を開きます。
2. 演習用リポジトリのページで **Use this template** を選択し、次に **Create a new repository** を選択します。

   ![Use this template ボタン](../../../_images/ex0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの案内に従います。それ以外の場合は、GitHub Copilot を利用できる組織にリポジトリを作成します。

   ![リポジトリテンプレートの設定を入力](../../../_images/ex0-repository-settings.png)

4. 後で使用するため、作成したリポジトリのパス (**organization-or-user-name/repository-name**) をメモします。

> [!NOTE]
> **バックログの準備完了**
>
> テンプレートからリポジトリを作成すると、GitHub issue のバックログも自動で作成されます。ワークショップでこれらの issue を使うため、自分で登録する必要はありません。

## codespace を作成する

次に、codespace で演習を行います。

[GitHub Codespaces][codespaces] は、ブラウザーで直接コードの作成、実行、デバッグができるクラウド開発環境です。複数のプログラミング言語、拡張機能、ツールをサポートする IDE を提供します。

1. 作成したリポジトリに移動します。
2. 緑色の **Code** ボタンを選択します。

   ![Code ボタンを選択](../../../_images/ex0-code-button.png)

3. **Codespaces** タブを選び、**+** ボタンを選択して新しい codespace を作成します。

   ![新しい codespace を作成](../../../_images/ex0-create-codespace.png)

codespace の作成には数分かかることがありますが、すべてのサービスを手動でインストールするよりもはるかに早く完了します。待っている間に GitHub Copilot の他の機能を確認してもかまいません。

> [!CAUTION]
> 今後の演習で codespace に戻ります。今はブラウザーのタブで開いたままにしてください。

> [!NOTE]
> このワークショップは codespace またはローカルの [dev container][dev-containers] で実行できます。どちらも必要な前提条件を備えています。ローカルで実行する場合は、クローンしたリポジトリを VS Code で開き、表示されたら **Reopen in Container** を選択します。codespace と同じ dev container が構築されます。

[codespaces]: https://github.com/features/codespaces
[dev-containers]: https://code.visualstudio.com/docs/devcontainers/containers

## GitHub Copilot Chat とエージェントモードを使う

GitHub Copilot Chat のエージェントモードには、IDE に GitHub Copilot Chat 拡張機能が必要です。GitHub Codespace を使用している場合は、すでにインストールされているはずです。

> [!TIP]
> 拡張機能がインストールされていない場合は、[Visual Studio Code Marketplace からインストール][copilot-chat-extension]できます。または Visual Studio Code の Extensions ビューを開き、**GitHub Copilot Chat** を検索して **Install** を選択します。

インストール後、GitHub アカウントで認証が必要な場合があります。

1. codespace に戻ります。
2. エディターの右側に Copilot Chat が表示されていない場合は、codespace 上部にある **Copilot Chat** アイコンを選択します。
3. Copilot Chat ウィンドウに `Hello world` などのメッセージを入力し、<kbd>Enter</kbd> を押します。Copilot Chat が有効になります。
4. 認証されていない場合は GitHub アカウントへのサインインが求められます。画面の案内に従います。

   ![Copilot Chat の認証プロンプトの例](../../../_images/ex1-copilot-authentication.png)

5. 認証後、Copilot Chat ウィンドウが表示されます。

## まとめ

演習用リポジトリのコピーを作成し、コードを書くための codespace を準備しました。

## 次のステップ

次は Copilot にプロジェクトの規約を伝えます。[演習 1 - カスタム指示][next-lesson]に進みます。

## リソース

- [GitHub Codespaces の概要][codespaces]
- [テンプレートからリポジトリを作成する][template-repository]
- [Codespaces クイックスタート][codespaces-quickstart]

[tailspin-template]: https://github.com/github-samples/tailspin-toys
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[codespaces-quickstart]: https://docs.github.com/codespaces/getting-started/quickstart
[copilot-chat-extension]: https://marketplace.visualstudio.com/items?itemName=GitHub.copilot-chat
[next-lesson]: ../1-custom-instructions/
