# React
## React 基礎知識
### Functional Component
- 関数定義
- `state`やライフサイクルメソッドを持たない
- JSXを`return`する
```jsx
// src/Hello.jsx
import React from 'react';

const Hello = (props) => {
    return (
        <div>
            <h2>こんにちは！{props.name}さん！</h2>
        </div>
    );
};
```
### Class Component
- クラス定義
- `React.Component`を継承する
- `state`やライフサイクルを持つ
- `props`には`this`が必要
- `render`メソッド内でJSXを`return`する
```jsx
// src/Hello.jsx
import React from 'react';

class Hello extends React.Component {
  constructor(props){
    super(props); 
  }
  render(){
    return (
        <div>
            <h2>こんにちは！{this.props.name}さん！</h2>
        </div>
    );
  }
};
```

### `props`で子コンポーネントに値を渡す
```jsx
// 親コンポーネント
import React from 'react';
import Thanks from './Thanks';

const Hello = () => {
    return (
        <div>
            <Thanks name={"サスケ"} />
        </div>
    );
};

// 子コンポーネント
import React from 'react';

const Thanks = (props) =>{
    return (
      <div>
          <h2>{props.name}、ありがとうだってばよ！</h2>
      </div>
    );
};

export default Thanks;
```

---
### Fragment
- `<div>`が増えてしまうのを避けるときに使える機能
- 複数コンポーネントの記述が可能
- `<> </>` or `<React.Fragment></React.Fragment>`で囲む
```jsx
class Hello extends React.Component{
  constructor(props) {
    super(props);
  }
  render() {
    return (
      // 複数のコンポーネントをラップできる
      <React.Component>
        <Hello title={"こんにちは！"} />
        <Hello title={"元気？"} />
        <Hello title={"久しぶり！"} />
      </React.Component>

      // こちらでもOK！
      <>
        <Hello title={"こんにちは！"} />
        <Hello title={"元気？"} />
        <Hello title={"久しぶり！"} />
      </>
    )
  }
}
```

---
### state
- 「ローカルステート」と呼ばれるもの
- オブジェクト型で記述するs
  - Reduxのstateは「グローバルステート」
- `props`として別コンポーネントに渡すことが可能
- `render`内では**値を変更してはいけない**
  - 無限ループになってしまう
- 値を変更する場合は`setState()`で変更する
- `state`を変更すると再レンダーされる
  - ページをリロードせずに表示を切り替えられる
- `Class Component`でのみstateを扱える
  - `Functional Component`では扱えない（Hooksを利用すれば可能）
#### stateの設定方法
```jsx
// オブジェクト型で記述する
class Blog extends React.Component{
  constructor(props) {
    super(props);
    this.state = {
      isPublished: false
    }
  }
}
export default Blog;
```
#### stateの取得方法
- 「同コンポーネント内」であれば、`this.state.変数名`で取得可能
- 子コンポーネントとして参照する場合は`props`として渡す
```jsx
// src/Blog.jsx
  render() {
    return (
      <>
        <Article 
            title={"Reactの使い方"} 
            isPublished={this.state.isPublished}
            // 関数型で渡さないと実行されて無限ループになってしまう）
            toggle={() => this.togglePublished()} />
      </>
    )
  }
```
#### stateの変更方法
- `setState()`を使用する
- `setState()`内に記述されたstateのみ変更する
- 基本的に関数でラップする
```jsx
// src/Blog.jsx
// 公開状態を反転させる関数
togglePublished = () => {
  this.setState({
    isPublished: !this.state.isPublished
  }) 
}
```
---
### ライフサイクル
内容が変更された場合やデータベースとのAPI通信など、関数の外にある処理
#### Mounting
コンポーネントを配置する
- `constructor`
  - 初期化
- `render`
  - 仮想DOMを描画する（JSXを返す）
- `componentDidMount()`
  - render()後に一度だけ呼ばれる関数
  - API通信など
#### Updating
コンポーネントを更新する
- `render`
  - 仮想DOMを再描画する
- `componentUpdate()`
  - 再render()後に呼ばれる関数
  - イベント発火時
#### Unmounting
コンポーネントを破棄する（無駄なイベントが解放されて負荷が軽減される）
- `componentWillUnmount()`
  - コンポーネントが破棄される直前に呼ばれる関数

---
### 名前付きexport
- クラスはexportできない
- 複数の関数をexportできる
- 記述は２通りある
```jsx
// export function 関数名(){}
export function Foo() {
  return(
    <div>Foo</div>
  )
}
// export const 関数名 = () => {}
export const Bar = () => {
  return(
    <div>Bar</div>
  )
}
```

### 名前なし(default)export
- クラスもexportできる
- 1ファイル1export
- ES6で推奨
- アロー関数は定義後にexportする
```jsx
// モジュール
// src/Foo.jsx
export default function Foo() {
  return(
    <div>Foo</div>
  )
};

// モジュール
// src/Bar.jsx
const Bar = () => {
  return(
    <div>Bar</div>
  )
};
export default Bar

// クラス
// src/Hoge.jsx
export default class Hoge extends React.Component{
  render(){
    return(
      <div>Hoge</div>
    )
  }
};
```
### モジュール全体をimport
- モジュール全体をimport
- 「名前なし(default)export」したモジュールをimport
```jsx
// src/Blog.jsx
import React from 'react';
import Article from './Article';

// Article.jsx
const Article = (props) => {
  return(
    <div>Article</div>
  )
};
export default Article
```
### 関数ごとのimport
- {}内にimportしたい関数名を記述する
```jsx
// src/Hoge.jsx
import { Foo, Bar } from './FooBar';

// src/FooBar.jsx
export function Foo() {
  return(
    <div>Foo</div>
  )
}

export const Bar = () => {
  return(
    <div>Bar</div>
  )
}
```
### 別名import
- 別名（エイリアス）をつけてimportできる
- モジュール全体：`* as 別名`
- モジュールの一部：`{モジュール名 as 新しいモジュール名}`
```jsx
// src/Hoge.jsx
import * as AnotherArticle from './Article';
import { Foo as AnotherFoo } from './FooBar'
```
---
## React Hooks
- Class Componentの機能をFunctional Componentでも使えるようになる
  - クラス（`this）`を使わないため、コードがシンプルになる
  - Functional Componentでstateを管理できる
- 完全後方互換
### `useState()`
- 「ステートフック」と呼ばれる
- クラスコンポーネントの`this.state`と`this.setState()`の代替機能
- 複数のstateを扱う場合、stateごとに宣言する
```jsx
// useStateの使い方

// 1. useState関数をimportする
import React, { setState } from 'react';

// 2. 宣言する
const [isPublished, togglePublished] = setState(false);
   // ↑state変数名  // ↑state変更関数名     // ↑stateの初期値

// 3. JSX内で使う
<input type="checkbox" checked={isPublished} id="check" onClick={() => togglePublished(!isPublished)} />
```
---
### `useEffect()`
- ライフサイクルメソッドの代替機能
- Functinal Componentでライフサイクルを使える
- 時間の流れではなく、機能ごとにコードをまとめられる
#### 仕組み
- render()ごとに処理が実行される
- 代替機能メソッド
  - `componentDidMount()`
  - `componentDidUpdate()`
  - `componentWillUnmount()`
### useEffectのパターン
#### 1. renderする度に実行
- 基本形
- `useEffect()`内にCallBack関数を書く
- CallBackはrenderする度に呼ばれる
- returnする関数はUnmount時に呼ばれる（クリーンアップ関数）
```jsx
useEffect(() => {
    // CallBack関数
  console.log('Render!');
  return () => {
    // クリーンアップ関数
    console.log('Unmounting');
  }
})
```
#### 2. Mount時にのみに実行
- 第2引数の配列内の値を前回のrenderと今回のrenderと比較する
  - 変更があればCallBack関数を実行する
- 第2引数に空の配列を渡すと、初回（Mount時）のみ実行される
```jsx
useEffect(() => {
  console.log(' Reander!');
}, [])
```
#### 3. Mount&Unmount時のみ実行
- 1と2の複合型
- 通常のCallBack関数はMount時（1回のみ）実行される
  - 2回目以降は配列が空のため、Updatingは実行されない
- Unmount時はreturn内のクリーンアップ関数が実行される
```jsx
useEffect(() => {
  console.log('Render!');
  return() => { 
    console.log('Unmounting');
  }
}, [])
```
#### 4. 特定のrender時に実行
- Mount時に実行される
- limitの値が変わった時に実行される
  - limitの値が`true`から`false`に変更した時に実行
```jsx
const [limit, release] = useState(true);

useEffect(() => {
  console.log('Render!');
}, [limit])
```
---
### Reactのmapメソッド
- mapメソッドは「CallBack関数」
- Reactでmapメソッドを使用する場合は`key`を設定する
  - `key`はどの要素が追加・変更・削除されたのか識別するため、それぞれの項目に`key`を与える
- 参考URL：[リストとkey](https://ja.reactjs.org/docs/lists-and-keys.html)
```jsx
// 一意に特定できるような文字列を key として選ぶのが最良の方法
// リスト全体を変更するのではなく、「変更のあった箇所だけ更新する」というReactの思想に基づいた設計を推奨している
const numbers = [1, 2, 3, 4, 5, 6];
const ListNumbers = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
 );

const todoList = todos.map((todo) =>
  <li key={todo.id}>
    {todo.content}
  </li>
);

// IDがない場合はインデックスを使用する
const todoLIst = todos.map((todo) =>
  <li key={index}>
    {todo.text}
  </li>
);

```
---
### return内の条件分岐
- `{}`内はjavaScriptになる
- 最初に条件式を記述する
- `()`内に真 or 偽 の時に返すJSXを記述する
#### `if` のみの場合
```jsx
// 「&&」
{(list.length === 0) && (
  <Loading />
)}

```
#### `if else` の場合
```jsx
// 「?」と「:」
{isQuestion ? (
  <Avatar alt="icon" src={Harinezumi} />
) : (
  <Avatar alt="icon" src={Human} />
)}
```
---
### コールバック関数における`bind()`
- renderされる度に毎回コールバック関数が生成されるのでパフォーマンスが落ちてしまう
  - 上記の事象を防ぐために`bind`を用いる必要がある
```jsx
constructor(prpps){
  super(props);
  this.slectAnswer = this.selectAnswer.bind(this);
}

selectAnswer = (selectAnswer, nextQuestionId) => {
  // 処理を記述する
}

render() {
  return(
    <AnswerList 
      anwers={this.state.answers}
      select={this.selectAnswer}
    />
  )
}
```
---
### Material-UI
#### Hook API
- 基本は`Hook APIを推奨`
  - 公式のサンプルコードが主にHook APIを使用ため
```jsx
export default function Hook() {
  const classes = useStyles();
  return <Button className={classes.root}>Hook</Button>;
}
```

#### Styled components API
- コンポーネントのシンタックスに直接スタイルを適用する
  - 名前とスタイルを紐付ける（Buttonコンポーネントに直接スタイルを当てる）
```jsx
export default function StyledComponents() {
  return <MyButton>Styled Components</MyButton>;
}
```

#### Higher-order component API
- スタイルを当てたコンポーネントを返すコンポーネント
```jsx
function HigherOrderComponent(props) {
  const { classes } = props;
  return <Button className={classes.root}>Higher-order component</Button>;
}
```
---




---
