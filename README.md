# レッスン3. PropsとState

## Props

さて、ここまでのレッスンで既にPropsについてはある程度理解が深まってきたのではと思います。ここでは簡単に、Propsの利用上の注意点及び、defaultPropsについて説明していきます。

## Propsの特徴

### Propsは読み取り専用

Propsは**書き換えることができません**。そのため、Propsの値を変更したい場合には別の変数を利用する必要があります。

```js
import React from 'react'

const Index = ({ word }) => {
  // const word = 'test' -> エラー
  const difWord = word === '世界' ? 'world' : 'mundo'
  return <p>Hello, {difWord}</p>
}

return Index
```

### データの受け渡しは一方通行

Reactでは、基本的にPropsの受け渡しは親から子への一方通行となっています。これは、一方通行にすることでデータの管理を行いやすくすることを目的にしています。ただし、大きなアプリになると、このことによって子から子へと何層にも渡ってデータを渡すことになり


## コンポーネント間でのPropsの受け渡し

Propsはレッスン2でも見たとおり、以下のような形式で子コンポーネントへと渡すことが出来ます。

```js
return <ChildComponent
  propOne={"propOneに渡す内容"}
  propTwo={"propTwoに渡す内容"} />
```

あるいは、まとめて渡せます。

```js
const props = {
  propOne: "propOneに渡す内容",
  propTwo: "propTwoに渡す内容"
}
return <ChildComponent {...props} />
```

子コンポーネントでは渡されたpropsに対して以下のようにしてアクセスすることが出来ます。

```js
const { propOne, propTwo } = this.props;
console.log(propOne); // propOneに渡された内容
console.log(propTwo); // propTwoに渡された内容
```

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

## Stateとベスト・プラクティス

非常に便利なStateですが一点注意点があります。複数のコンポーネントがそれぞれstateを持つと管理が難しくなる問題です。Reduxというライブラリが作られた理由の一つがこのStateの管理を行いやすくするためです。

Reduxを使わないでこのStateの管理を行うために考え出されたベスト・プラクティスの一つが、コンテナーコンポーネントと、プレゼンテーションコンポーネントの分離です。

- コンテナーコンポーネント

コンテナーコンポーネントは、ステートを保持し、データの変更などを管理します。コンテナーは基本的には表示の部分は扱わないか最小限にします。

- プレゼンテーションコンポーネント

プレゼンテーションコンポーネントはそれ自体ではステートを持たず、Propsの受け取りとそれを利用したデータの表示のみを行います。

実際に例を見てみましょう。ここでは、非常に簡単なものですがアプリのテーマを管理するコンテナーと、テーマの変更フォームを表示するコンポーネント、テーマの実際の表示を行うコンポーネントの3つを作ってみます。

![theme-switcher-image.png](https://firebasestorage.googleapis.com/v0/b/codegrit-images.appspot.com/o/codegrit-react%2FLesson03%2Ftheme-switcher-image.png?alt=media&token=997e11f7-1f27-4981-98f4-e7a84efe3528)

サンプルコードはこちら

[codegrit-react-lesson03-sample-theme-switcher](https://github.com/codegrit-jp-students/codegrit-react-lesson03-sample-theme-switcher)


```js
import React, { Component } from 'react'
import PropTypes from 'prop-types'

class ThemeContainer extends Component {
  state = {
    theme: 'dark'
  }

  onSwitchTheme = (theme, e=null) => {
    if (e) e.preventDefault();
    this.setState({ theme })
  }

  render() {
    const { theme } = this.state;
    return (
      <main>
        <ThemeSwitcher 
          theme={theme}
          switchTheme={this.onSwitchTheme} />
        <ShowTheme 
          theme={theme}
        />
      </main>
    );
  }
}

const ThemeSwitcher = ({ theme, switchTheme }) => (
  <ul>
    <li>
      <a href onClick={(e) => switchTheme('normal', e)}>
        Normal
      </a>
    </li>
    <li>
      <a href onClick={(e) => switchTheme('light', e)}>
        Light
      </a>
    </li>
    <li>
      <a href onClick={(e) => switchTheme('dark', e)}>
        Dark
      </a>
    </li>
  </ul>
);

ThemeSwitcher.propTypes = {
  switchTheme: PropTypes.func.isRequired
}

const ShowTheme = ({ theme }) => {
  let bgStyle, fontStyle;
  switch (theme) {
    case 'normal':
      bgStyle = {
        backgroundColor: 'gray',
      }
      fontStyle = {
        color: 'black',
      }
      break;
    case 'dark':
      bgStyle = {
        backgroundColor: 'gray',
      }
      fontStyle = {
        color: 'black',
      }
      break;
    default:
      bgStyle = {
        backgroundColor: 'gray',
      }
      fontStyle = {
        color: 'black',
      }
      break;
  }
  return (
    <div className="wrapper" style={bgStyle}>
      <p style={fontStyle}>現在のテーマは{theme}です。</p>
    </div>
  );
}

ShowTheme.defaultProps = {
  theme: 'normal'
}

ShowTheme.propTypes = {
  theme: PropTypes.string
}
```

上のように、テーマの変更のファンクション自体やテーマのデータの保持は、ThemeContainerが保持しています。ShowThemeコンポーネントとThemeSwitcherコンポーネントは表示またはPropsで渡されたファンクションの呼び出しを行っています。

こうすることで、ShowTheme、ThemeSwitcherコンポーネントのテストが行いやすくなり、またThemeContainerのみを見ればどのようなステートとファンクションが利用される予定なのかがすぐに分かります。

また、コンテナーコンポーネントではclassを利用してコンポーネントを定義しているのに対して、プレゼンテーションコンポーネントではファンクションを利用してコンポーネントを定義していることにも注目しましょう。コンポーネントの定義はこのように役割によって使い分けることができます。

まだStateを学んだばかりなのですぐにはメリットが掴みづらいかと思いますが、今後のレッスンで徐々に使い分けを学んでいきましょう。

## 更に学ぼう

- [React公式 - コンポーネントと props](https://ja.reactjs.org/docs/components-and-props.html)
- [React公式 - stateとライフサイクル](https://ja.reactjs.org/docs/state-and-lifecycle.html)
- [React公式 - Reactの流儀](https://ja.reactjs.org/docs/thinking-in-react.html)