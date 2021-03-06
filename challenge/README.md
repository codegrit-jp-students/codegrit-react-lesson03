# チャレンジ3

## 目的

- コンテナーコンポーネントとステートレスコンポーネントを使い分ける。
- PropTypesやDefaultPropsを利用する。
- Themeを設定する

## チャレンジの取り組み方

1. スターターファイルのリポジトリを、フォークして自分のGitHubアカウントにリポジトリを作りましょう。
2. マイルストーンごとに要件に合うようにファイルを編集していきます。
3. 分からない部分があれば、テキストを復習して、再度チャレンジしてみましょう。
4. 再チャレンジしてしばらく考えても分からない場合はチャットでメンターに質問しましょう。
5. 完了したら、GitHubリポジトリをメンターにシェアしてください。(質問時に既にシェアしている場合は、レビューを直接依頼しましょう。)
6. メンターから課題レビューが届きます。
7. ビデオチャットの際は、分からない点を更に突っ込んで聞いたり、より良い書き方を聞いてみましょう。

## 概要

このチャレンジでは、チャレンジ2で作成したインスタカードに、PropTypesとdefaultPropsを追加してより各コンポーネントの内容をわかりやすくします。また加えてライトモードと、ダークモードの2つのモードを設定していきます。

## 完成イメージ

![react_ch03_image](https://firebasestorage.googleapis.com/v0/b/codegrit-images.appspot.com/o/codegrit-react%2FLesson03%2Fchallenge%2Fcodegrit-react-ch03-video.gif?alt=media&token=103353dd-2660-4783-9d86-c29c44fa7aa9)

## スターターファイル

- [codegrit-jp-students/react-ch03-starter](https://github.com/codegrit-jp-students/codegrit-react-ch03-starter)

上記のURL先のリポジトリをフォークして、コードを書き進めて行きましょう。

## マイルストーン１

### 要件

1. ThemeSwither.jsに適切なpropsを渡して、テーマを変更出来るようにしましょう。

2. `className="insta-card"`の部分を変数を利用してクラス名がテーマによって、テーマが"light"なら"insta-card"、"dark"なら"insta-card insta-card-dark"というクラスが入るようにします。

3. `theme`propsをうまく子コンポーネントに渡していき、うまく完成イメージと同じものを再現しましょう。

## マイルストーン2

### 要件

`prop-type`パッケージを利用して、defaultPropsとpropTypesを設定します。

1. `prop-types`パッケージを利用して、各コンポーネントに対して必要ならdefaultPropsとpropTypesを設定しましょう。

2. `ThemeSwithcher`コンポーネントや`MainIcons`コンポーネントでは、条件によって画像やクラスを切り替える方法(の内の一つ)を利用しています。よく確認して、今後自分でも同じことができるようになりましょう。

## 評価

課題の後、以下の２つについてメンターにフィードバックをお願いします。

1. 要件のカバー度: 1.全く出来なかった 2.ほとんど出来なかった 3. 半分ほどは出来た 4.8割ほどは出来た 5. 全部出来た
2. 難易度: 1. とても難しかった 2. 難しかった 3. ちょうど良かった 4. 簡単だった 5. とても簡単だった
