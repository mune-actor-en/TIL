# React
## 基礎知識
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
```jsx
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
コンポーネントを破棄する
- `componentWillUnmount()`
  - コンポーネントが破棄される直前に呼ばれる関数



---
