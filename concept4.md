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