# React*TS

## ① 拡張子をtsではなく、tsxにする
- **TypeScriptファイル内でReactのJSX構文が使えるようになる**

## ② tsconfig.json設定を変更
React + TypeScript（.tsx）を使う場合は下記を変更

```json
{
  "compilerOptions": {
    "jsx": "react-jsx"
  }
}
```

```json
// おすすめの設定
{
  "compilerOptions": {
    "target": "ES2020",        // 生成するJSのバージョン。ES2020の文法まで使える。
    "module": "ESNext",        // import/export をそのまま出力（Vite/Webpack向け）。
    "strict": true,            // TypeScriptの厳格モードをすべてONにする（型安全が上がる）。
    "jsx": "react-jsx",        // React 17+ の新しいJSX変換方式を使う設定。
    "moduleResolution": "Node",// Node.js式のモジュール解決（node_modulesを探すなど）。
    "esModuleInterop": true    // CommonJSのライブラリをimportで扱いやすくする。
  }
}
```

## ③ propsに型を付ける方法
props の型は「オブジェクトの形」を定義するだけ

```tsx
// 基本系
type Props = {
  title: string
  count: number
}

const MyComponent = ({ title, count }: Props) => {
  return <h1>{title}：{count}</h1>
}
```
**ポイント**
- Props という型を作る
- 引数に Props を指定する
- これだけで props の型チェックが効く

### 子要素（children）を受け取る場合
children を使うコンポーネント

```tsx
type Props = {
  children: React.ReactNode
}

const Box = ({ children }: Props) => {
  return <div className="box">{children}</div>
}
```
## ReactNode と ReactElement の違い
### ReactNode（型）
- **React が描画できるすべて**
→ ほぼ何でも入る

例：
- 文字列 "hello"
- 数字 123
- null
- undefined
- JSX <div />
- 配列 [<A />, <B />]

👉 **つまり children の型はほぼ ReactNode 一択**

### ReactElement（型）
- **JSX で書かれた 1 つの要素**

```tsx
<div />
<MyComponent />
```
**ReactElement は 「JSX 1個」限定**
→ children の型には向かない
→ 返り値の型として使われることが多い


## ④ Hooksをタイプスクリプトで使う方法
### useState
- 自動推論される場合（これが基本）
```tsx
const [count, setCount] = useState(0)
// number と推論される
```
- 型を明示したい場合
```tsx
const [name, setName] = useState<string>("")
```

### null を扱う場合
```tsx
const [user, setUser] = useState<User | null>(null)
```

### useEffect
- 特に型は不要

```tsx
useEffect(() => {
  console.log("mounted")
}, [])
```

### useRef
- DOM を参照する場合

```tsx
const inputRef = useRef<HTMLInputElement>(null)

<input ref={inputRef} />
```
- 値を保持する場合

```tsx
const idRef = useRef<number>(0)
```

## ⑤ イベントハンドラとタイプスクリプトを一緒に使う方法
React のイベントは React 独自の型 を使う

### onChange（input）
```tsx
const handleChange = (e: React.ChangeEvent<HTMLInputElement>) => {
  console.log(e.target.value)
}
```

### onClick（button）
```tsx
const handleClick = (e: React.MouseEvent<HTMLButtonElement>) => {
  console.log("clicked")
}
```

### onSubmit（form）
```tsx
const handleSubmit = (e: React.FormEvent<HTMLFormElement>) => {
  e.preventDefault()
}
```

### ツール（Redux / React Router）
#### Redux Toolkit（RTK）
- 型はほぼ自動でつく
→ 実務では Redux Toolkit + useSelector/useDispatch の型付け が定番

```tsx
const count = useAppSelector((state) => state.counter.value)
const dispatch = useAppDispatch()
```

#### React Router
- パラメータの型付けが重要

```tsx
import { useParams } from "react-router-dom"

type Params = {
  id: string
}

const UserPage = () => {
  const { id } = useParams<Params>()
}
```

## まとめ

| 項目 | 一番覚えるべきこと |
|------|----------------------|
| **props** | `type Props = { ... }` を作って引数に付ける |
| **children** | `ReactNode` を使う |
| **ReactNode** | ほぼ何でも入る（children 向け） |
| **ReactElement** | JSX 1個（返り値向け） |
| **useState** | 推論に任せる。必要なら `<string>` のように型指定 |
| **useRef** | DOM → `<HTMLInputElement>`、値 → `<number>` |
| **イベント** | `React.ChangeEvent<HTMLInputElement>` など React 独自の型を使う |
