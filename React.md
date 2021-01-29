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
      <React.Fragment>
        <Hello title={"こんにちは！"} />
        <Hello title={"元気？"} />
        <Hello title={"久しぶり！"} />
      </React.Fragment>

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
- `componentDidMount`と同じ挙動をする
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

### useCallback
## Custom Hooks
Custom Hooksとは、Hooksで用意されている関数（useStateやuseEffectなど）を組み合わせて、独自の関数を作成することを意味する。
```tsx
// customHooks.tsx

import { ChangeEvent, Dispatch, SetStateAction, useCallback } from 'react';

type UseChangeEvent<T> = {
  (update: Dispatch<SetStateAction<T>>): (event: ChangeEvent<HTMLInputElement>) => void
}

// 関数名の先頭に「use」を付けると、HooksがCustom Hooksだと認識してくれる
// ※本来ならば「customHooks.tsx」はReactのコンポーネントファイルではないため、「useCallback」は使用できないが、Custom Hooks内（「use」がついたもの）であれば使用可能となる。
export const useStringChangeEvent: UseChangeEvent<string> = (update) => {
  return useCallback(
    (event: ChangeEvent<HTMLInputElement>) => {
      update(event.target.value);
    }, [update]
  );

```


---
## Reactのmapメソッド
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
## React + Typescript
### 参考URL
- [Create React App Adding TypeScript](https://create-react-app.dev/docs/adding-typescript/)
- [React+TypeScript Cheatsheets](https://github.com/typescript-cheatsheets/react-typescript-cheatsheet#reacttypescript-cheatsheets)
- [ReactをTypeScriptで書く1: TypeScript編](https://www.dkrk-blog.net/javascript/react_ts01#type-alias%E5%9E%8B%E3%82%A8%E3%82%A4%E3%83%AA%E3%82%A2%E3%82%B9)
- [TypeScriptの型入門](https://qiita.com/uhyo/items/e2fdef2d3236b9bfe74a)
- [TypeScriptのInterfaceとTypeの比較](https://qiita.com/tkrkt/items/d01b96363e58a7df830e)

---
### 導入
#### React + Typescriptの環境を構築
`npx create-react-app my-app --template typescript`<br>
または<br>
`yarn create react-app my-app --template typescript`
#### Typescriptに必要な「Create React App」の資源をインストール
`npm install --save typescript @types/node @types/react @types/react-dom @types/jest`<br>
または<br>
`yarn add typescript @types/node @types/react @types/react-dom @types/jest`
#### `index.ts`をリネーム
`src/index.ts`を`src/index.tsx`に名前を変更し、サーバーを再起動する

---
### Typescriptの基本
#### 型定義
明示的に型を指定するため、変数名の後に`:型名`をつける（Type Annotation）
```tsx
// planetには文字列のみ代入できる
let planet: string = 'earth';
```
#### プリミティブ型
- string
- number
- boolean
- null
- undefined
- symbol
- bigint
#### リテラル型
- リテラル型とは、「プリミティブ型を細分化」したもの
  - 文字列のリテラル型：【例】`'planet'`
  - 数値のリテラル型：【例】`5` 
  - 真偽値のリテラル型：【例】`true`
```tsx
// 'earth'は'earth'という文字列しか許されない型になっている
const planet: 'earth' = 'earth';
const galaxy: 'blackHole' = 'earth';    //当然'blackHole'は'earth'という文字列ではないため、エラーになる
```
文字列のリテラル型は`string`型に属しているので、`string`型として扱うことができる。（他の型も同じ）
```jsx
const planet: 'earth' = 'earth';
const galaxy: string = 'earth';    // 'earth'はstring型に含まれるので型エラーにならない
```
#### リテラル型と型推論
下記のサンプルは`const`で変数宣言している、つまり再代入できないのでずっと`'earth'`が代入していることを保証している。<br>
変数`planet`は`'earth'`が代入されているので、`'earth'`型となる。
```tsx
const planet = 'earth';    // planetは'earth'型を持つ
const galaxy: 'blackHole' = planet;    // 'earth'型は'blackHole'型ではないためエラーとなる
```
`const`ではなく`let`を使用する場合は変数が置き換わる可能性があるため、リテラル型で限定するのではなく「プリミティブ型」で定義する。
```tsx
let planet = 'earth';     // planetはstring型で推論される
const galaxy: string = planet;
const comet = 'cometHalley' = planet;    // planetはstring型で型推論されるが、'cometHalley'型を持つcometには代入できない
```
#### any型
なんでも代入できる。しかし、型定義のメリットを活かせないので極力使用しない。
```tsx
// planetにはなんでも代入できる
let planet: any = 'earth';
```
#### union型
なんらかの型が代入される、または代入してほしい場合に使用する。<br>
`|`（パイプ）でつなげて記述する。
```tsx
// planetにはstringかundefinedを代入できる
let planet: string | undefined = 'earth';
```
上記のように`undefined`が代入される可能性がある場合に、`undefined`では使用できない`length`などを実行すると、エラーになってしまう。<br>
そこで`tsconfig.json`ファイルに`"strictPropertyInitialization": true`を設定すると、あらかじめエラーを知らせてくれる。
```tsx
let planet: string | undefined;
planet.length;    // 「undefinedの可能性があります」と警告してくれる
```
コンパイルエラーを防ぐためには、型定義に合った適切な値を代入することでコンパイルが通るようになる。
```tsx
let planet: string | undefined;
planet = 'earth';    // 適切な値を代入する
planet.length;
```
#### 文字列リテラル
特定の文字列だけ代入してほしい場合に使用する。
```tsx
// planetには「earth」か「jupiter」か「sun」が代入できる
let planet: 'earth' | 'jupiter' | 'sun' = 'earth';
```
#### 関数
「引数」と「返り値」の型を指定できる。<br>
返り値がない場合は、`void`を指定する。
```tsx
// アロー関数で記述
// number（数値）で型定義
// 返り値なしなので「void」
const addCount = (num: number): void =>{
  console.log(num + 1);
}
```
#### 配列
```tsx
const planet: string[] = ['earth', 'moon', 'sun',];
```
#### オブジェクト
```tsx
const planet: {
  name: string;
  size: number;
} = {
  name: 'earth',
  size: 6371
}
```

#### ジェネリック型
型を後から実際の型を定義できる。（型の再利用が可能）<br>
```tsx
type UseChangeEvent<T> = {
// 「T」の部分を「ジェネリック型パラメーター」という
  (update: Dispatch<SetStateAction<T>>): (event: ChangeEvent<HTMLInputElement>) => void
}

// string型の場合
export const useStringChangeEvent: UseChangeEvent<string> = (update) => {
}

// number型の場合
export const useNumberChangeEvent: UseChangeEvent<number>= (update) => {
}
```

---
#### InterfaceとTypeの違い
- Interface
基本的にInterfaceを使うといい。要素をあとで拡張したい場合に追加できるため

|項目|Interface|Type|
|:--|:--|:--|
|使いどころ|クラスやオブジェクトを定義|型もしくは型の組み合わせに別名をつける|
|継承|○|○<br>※交差型（複数の型を全て満たす型）で可能|
|同じ要素の宣言|マージされる|エラーになる|
|交差型・共用体型・タプル型|×|○|


---
