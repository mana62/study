# watch モード（自動コンパイル）
＝ **コードを保存するたびに自動で JS に変換**してくれるモード

```bash
tsc index.ts --watch
```
⚫︎または短く
```bash
tsc index.ts -w
```

⚫︎止めたいときは
```bash
Ctrl + C
```

## 複数ファイルを一気にコンパイルする方法
⚫︎ファイルを並べるだけ
```bash
tsc index.ts index1.ts
```
＝ スペース区切りで OK