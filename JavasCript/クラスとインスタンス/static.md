# クラスのstatic
＝staticは基本classのみに使われる
✅static → クラス自体に属する（プロトタイプじゃない）
✅通常のメソッド → プロトタイプに属する（効率良く共有）
✅constructorのプロパティ → 各インスタンスに属する

```js
// 例
class Pokemon {
  constructor(name, hp) {
    this.name = name;  // 各ポケモンで違う(ここはフィールド)
    this.hp = hp;      // 各ポケモンで違う(ここはフィールド)
  }

  // 通常のメソッド - 各ポケモンで使える
  attack() {
    console.log(`${this.name}の攻撃！`);
  }

  // staticプロパティ - 全ポケモン共通
  static maxLevel = 100;
  static totalCreated = 0;

  // staticメソッド - 全ポケモン共通の処理
  static getMaxLevel() {
    return Pokemon.maxLevel;
  }
}

const pikachu = new Pokemon("ピカチュウ", 35);
const zenigame = new Pokemon("ゼニガメ", 44);

// 各ポケモンで違う
console.log(pikachu.name);   // "ピカチュウ"
console.log(zenigame.name);  // "ゼニガメ"

// 全ポケモン共通
console.log(Pokemon.maxLevel);  // 100（どのポケモンも最大レベルは100）
```

🟠 プロトタイプのメソッドにstaticをつけた場合
→ static メソッドは「クラス（コンストラクタ関数）自身のプロパティ」になる（インスタンスではなくクラスに属する）

⚠️フィールドに、staticを書かずに通常通り書いた場合
→ インスタンスが呼ばれるごとに作成される（個別）

⭕️フィールドに、staticを用いて書いた場合
→ クラス定義時に1回だけ作られ、全インスタンスで共有（メモリ効率化）

＝共通で使用するもののみ、staticで読んだ方が効率が良さそう