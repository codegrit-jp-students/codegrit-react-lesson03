# Propsの解説

## Props

さて、ここまでのレッスンで既にPropsについてはある程度理解が深まってきたのではと思います。ここでは簡単に、Propsの特徴と、defaultPropsについて説明していきます。

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

Reactでは、基本的にPropsの受け渡しは親から子への一方通行となっています。これは、一方通行にすることでデータがどこから渡されているのかの流れをわかりやすくすることを目的にしています。

大きなコンポーネントになってくると、データの受け渡しが何層にも渡って行われることになり大変な場合があります。こうした場合には別の手法を使うことが出来るのですが、これについてはReduxコース以降で説明します。

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