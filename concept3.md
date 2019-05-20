# Stateの初期値とsetStateファンクション

## State

さて、ここまでは親要素から渡されるデータPropsのみにフォーカスしてきました。ReactではPropsに加えて、そのコンポーネント自体が保持するデータとしてStateを利用することができます。

## Stateの初期値を設定する

stateはclassを利用したComponentの定義内で以下のようにして指定できます。

```js
import React, { Component } from 'react'

class LoginForm extends Component {
  constructor() {
    super()
    this.state = {
      email: '',
      password: ''
    }
  }
  render() {
    const { email, password } = this.state;
    return (
      ...省略
    );
  }
}

export default LoginForm;
```

上記のように、stateに保存された値は`this.state`で呼び出すことができます。

また、Stateは以下のように設定することもできます。

```js
import React, { Component } from 'react'

class SignupForm extends Component {
  state = {
    email: '',
    password: '',
    termAgreement: false
  }
  render() {
    const {
      email,
      password,
      termAgreement,
    } = this.state;
    return (
      ...省略
    );
  }
}

export default SignupForm;
```

## setStateファンクションでStateをアップデートする

Propsと異なりStateは書き込みを行うことができます。この際には必ずstateを直接書き換えずに、setStateファンクションを利用します。

setStateは通常以下のようにして呼び出します。

```js
this.setState({
  プロパティ名: 値
})
```

ただしsetStateファンクション内で、stateやpropsを使う場合には、注意点が一つあります。複数のsetStateが同時に呼び出されたり、Propsがアップデートされている途中などの場合、stateの値が上手くアップデートされないことがあります。そのため、こうした場合には以下のようにファンクションを利用して書きます。

```js
this.setState((state, props) => {
  プロパティ名: 値
})
```

引数のpropsは利用しない場合は省略が可能です。例えば以下の例をご覧ください。

```js
import React, { Component } from 'react'

class SignupForm extends Component {
  state = {
    email: '',
    password: '',
    termAgreement: false
  }

  // 規約に同意するボタンがクリックされた場合に呼び出されるファンクション
  onClickAgreeBtn = (e) => {
    e.preventDefault()
    this.setState((state) => ({
      termAgreement: !state.termAgreement // 現在のステートのtermAgreementのBoolean値を反転させる。
    }))
  }

  render() {
    const {
      email,
      password,
      termAgreement,
    } = this.state;

    return (
      ...省略
    );
  }
}

export default SignupForm;
```