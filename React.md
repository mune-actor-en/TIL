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



---
