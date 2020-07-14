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
