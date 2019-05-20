# defaultPropsとタイプチェック

## defaultProps

defaultPropsを設定することで**Propsが親要素から渡されなかった場合のデフォルトの値を設定**することができます。

例えばですが、モダルを利用したログインフォームと、利用しない場合とでフォームの内容を変えたいとしましょう。この際に例えばモダルを利用しない場合の値をdefaultPropsに設定しておいて、モダルを利用する場合には個別で値を渡すというようなことができます。

```js
import React, { Component } from 'react'

class LoginForm extends Component { 
  render() {
    return (
      ...省略
    );
  }
}

LoginForm.defaultProps = {
  withModal: false,
  modalWidth: '350px',
  modalHeight: '450px'
}
```

また上記のように外に出してdefaultPropsを設定する以外に、スタティックメソッドを利用して以下のように書くこともできます。

```js
class LoginForm extends Component { 
  static defaultProps = {
    withModal: false,
    modalWidth: '350px',
    modalHeight: '450px'
  }

  render() {
    return (
      ...省略
    );
  }
}
```

## Propsのタイプチェックを行う

さて、Propsを渡した時に時折起こることとして、Propsに思っていた通りの値が渡っていないことや、Propsに渡すべき値が何なのか分からないというようなことが起こりえます。こうした状況を避けるために`prop-types`というライブラリを利用してpropsのタイプチェックを行うことができます。また、後から見た時にそのコンポーネントがどういうPropsを受け取るのかがひと目でわかるため開発効率の向上にも役立ちます。

`prop-types`ライブラリは元々はReactに含まれていたのですが、Facebookの開発している別のタイプチェッキングライブラリ"Flow"との使い分けがより行いやすいように分離されたという経緯があります。そのため、`prop-types`ライブラリを別でインストールする必要があります。

まずはyarnを利用してライブラリをインストールしましょう。

```bash
$ yarn add -D prop-types
```

準備はこれで完了です。では先程のdefaultPropsに加えてPropsのタイプチェックを設定してみましょう。タイプチェックを行うにはpropTypesというオブジェクトを設定します。

```js
import React, { Component } from 'react'
import PropTypes from 'prop-types'

class LoginForm extends Component { 
  render() {
    return (
      ...省略
    );
  }
}

LoginForm.defaultProps = {
  withModal: false,
  modalWidth: '350px',
  modalHeight: '450px'
}

LoginForm.propTypes = {
  withModal: PropTypes.bool,
  modalWidth: PropsTypes.number,
  modalHeight: PropsTypes.number
}
```

propTypesはdefaultPropsと同様にstaticメソッドを利用して書くこともできます。

## PropTypesでチェックできる値の種類

PropTypesを利用してチェックできる値は主に以下のようなものがあります。これ以外にチェックしたいものがある場合は公式ドキュメントを都度見るのが良いでしょう。

- 配列: PropTypes.array
- Boolean: PropTypes.bool,
- ファンクション: PropTypes.func,
- 数値: PropTypes.number,
- オブジェクト: PropTypes.object,
- 文字列: PropTypes.string,
- シンボル: PropTypes.symbol,
- React要素: PropTypes.element

### 複数の値が入りうる場合

例えば、Propsに複数のタイプが入りうる際には`oneOfType`を利用します。例えば、文字列と数字列のいずれかを受け付ける場合は以下のようにします。

PropTypes.oneOfType: {
  PropTypes.string,
  PropTypes.number,
}

### 配列内の要素の値を指定したい場合

配列内の要素に決まった値を入れたい場合は`arrayOf`を利用します。例えば以下は配列内の要素で数値のみを取りたい場合の例です。

PropTypes.arrayOf(PropsTypes.number)

### 決まった値のいずれかをとってほしい場合

例えば、Propsには数値の1,2,3のみしか与えられないようにしたいような場合、`oneOf`を利用することができます。

PropTypes.oneOf([1, 2, 3])

### Propsが必ず必要にする

指定したPropsが必ず渡されることを期待する場合は`isRequired`をtype名の後につけます。

例: 数値が必ず必要な場合

PropsTypes.number.isRequired

### Objectの中身を細かく指定する

Propsで渡すObjectの中身を定義したい場合は`shape`を利用します。例えば、ログインフォームでメールアドレスとパスワードが必要な場合以下のようにします。

LoginForm.propTypes = {
  inputs: PropTypes.shape({
    email: PropTypes.string.isRequired,
    password: PropTypes.string.isReqied
  }).isRequired
}