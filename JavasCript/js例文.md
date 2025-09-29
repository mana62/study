# 文字を逆からに変換
```js
const str = "Hello, JavaScript!";
const reverseStr = str.split('').reverse().join('');
console.log(reverseStr);
```

# 配列の中の最小値と最大値
```js
const numbers2 = [45, 12, 78, 34, 89, 2, 56];
const bigNum = Math.max(...numbers2);
const smallNum = Math.min(...numbers2);
console.log(`最大値:${bigNum}`);
console.log(`最小値:${smallNum}`);
```

# 1から10まで表示して10で停止
```js
let count = 0;
const interval = setInterval(() => {
  count++;
  console.log(`${count}`);
  if (count === 10){
    clearInterval(interval);
    console.log('停止');
  }
},1000);
```

# 2秒後に文字を表示
```js
setTimeout(() => {
  console.log('JavaScriptの練習中です!');
}, 2000);
```

# 配列の合計を求める
```js
const numbers = [12, 34, 56, 78, 90];
const totalNumber = numbers.reduce((acc, num) => acc + num, 0);
console.log(totalNumber);
```

# 配列の文字を順番に表示
```js
const messages = ["こんにちは！", "元気ですか？", "JavaScriptは楽しいですね！"];
for (let index = 0; index < messages.length; index++) {
  console.log(messages[index]);
}
```

# 配列を偶数と奇数に分ける
```js
const number = [13, 22, 35, 46, 57, 68, 79];
let even = [];
let odd = [];
number.forEach((index) => {
  if(index % 2 === 0) {
    even.push(index);
  } else {
    odd.push(index);
  }
});
console.log(even);
console.log(odd);
```

# カウントダウン（7→1）
```js
let seconds = 7;
const countDown = setInterval(() => {
  seconds--;
  if (seconds > 0) {
    console.log(seconds);
  } else {
    console.log('終了！');
    clearInterval(countDown);
  }
}, 1000);
```

# 順番にメッセージを表示（繰り返し）
```js
const messages = ["こんにちは！", "元気ですか？", "JavaScriptは楽しいですね！"];
let index = 0;
const interval1 = setInterval(() => {
  console.log(messages[index]);
  index++;
  if (index >= messages.length) {
    index = 0; // 最初に戻る
  }
}, 3000);
```

# ランダムにサイコロを振る
```js
let diceRoll = Math.floor(Math.random() * 6) + 1;
console.log(diceRoll); // 1〜6
```

# 重複を取り除いて配列にする
```js
const numbers = [1, 2, 3, 4, 3, 2, 1, 5, 6, 7, 7];
const clearNumber = [...new Set(numbers)];
console.log(clearNumber);
```

# getLongestWord関数
```js
const words = ["cat", "elephant", "dog", "giraffe", "fox"];
function getLongestWord(words) {
  return words.reduce((longest, current) => current.length > longest.length ? current : longest);
}
console.log(getLongestWord(words));
```

# upperCaseWords関数
```js
function upperCaseWords() {
  return words.filter(word => word.length >= 4).map(word => word.toUpperCase());
}
console.log(upperCaseWords());
```

# priceの合計値
```js
const products = [
  { name: "Apple", price: 100 },
  { name: "Orange", price: 150 },
  { name: "Banana", price: 200 }
];
const totalPrice = products.reduce((acc, item) => acc + item.price, 0);
console.log(totalPrice);
```

# フィボナッチ数列
```js
function fibonacci(n) {
  let firstNum = [0, 1];
  for (let i = 2; i < n; i++) {
    firstNum.push(firstNum[i - 1] + firstNum[i - 2]);
  }
  return firstNum;
}
console.log(fibonacci(10));
```

# エラーハンドリング（0で割る）
```js
function divide(a, b) {
  if (b === 0) {
    throw new Error('エラー: 0で割ることはできません');
  }
  return a / b;
}
try {
  console.log(divide(10, 2));
  console.log(divide(10, 0));
} catch (error) {
  console.error(error.message);
}
```

# 深いコピー
```js
const user = {
  name: "Taro",
  address: {
    city: "Tokyo",
    zip: "100-0001"
  }
};
const copyUser = JSON.parse(JSON.stringify(user));
console.log(copyUser);
```

# APIからデータ取得（fetch）
```js
fetch('https://jsonplaceholder.typicode.com/todos/1')
  .then(response => {
    if (!response.ok) throw new Error('Network response was not ok');
    return response.json();
  })
  .then(data => console.log(data))
  .catch(error => console.error('エラー:', error));
```

# アナグラム判定
```js
function isAnagram(str1, str2) {
  const normalize = str => str.toLowerCase().split('').sort().join('');
  return normalize(str1) === normalize(str2);
}
console.log(isAnagram("listen", "silent")); // true
console.log(isAnagram("hello", "world")); // false
```

# クラスの作成
```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  introduce() {
    console.log(`こんにちは、私は${this.name}です。${this.age}歳です。`);
  }
}
const person1 = new Person("太郎", 25);
person1.introduce();
```

# setTimeoutで段階的に表示
```js
setTimeout(() => {
  console.log('準備中...');
  setTimeout(() => {
    console.log('もうすぐ始まります！');
    setTimeout(() => {
      console.log('開始しました！');
    }, 2000);
  }, 2000);
}, 2000);
```

# 配列のスライス（部分取得）
```js
const arr = [1, 2, 3, 4, 5];
const part = arr.slice(0, 3);  // [1, 2, 3]
console.log(part);
```

# 配列の共通部分を取得（filter & includes）
```js
const arr1 = [1, 2, 3, 4, 5];
const arr2 = [3, 4, 5, 6, 7];
const common = arr1.filter((val) => arr2.includes(val));
console.log(common); // [3, 4, 5]
```

# Promiseで3秒後に完了
```js
function waitThreeSeconds() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve("処理完了");
    }, 3000);
  });
}
waitThreeSeconds().then(result => {
  console.log(result);
});
```

# fetch + タイムアウト処理（Promise.race）
```js
function fetchWithTimeout(url, timeout = 500) {
  return Promise.race([
    fetch(url),
    new Promise((_, reject) => setTimeout(() => reject(new Error('タイムアウト')), timeout))
  ]);
}
```

# someで10未満の要素があるか判定
```js
const nums = [5, 8, 12, 20];
const someNums = nums.some((num) => num < 10);
console.log(someNums); // true
```

# everyで全要素が10以上か判定
```js
const nums = [10, 20, 30, 40];
const checkNum = nums.every(num => num >= 10);
console.log(checkNum); // true
```

# カタカナ変換（ひらがな→カタカナ）
```js
const names = ["たろう", "はなこ", "じろう"];
const toKatakana = (str) => {
  return str.replace(/[\u3041-\u3096]/g, ch =>
    String.fromCharCode(ch.charCodeAt(0) + 0x60)
  );
};
const katakanaNames = names.map(toKatakana);
console.log(katakanaNames); // ["タロウ", "ハナコ", "ジロウ"]
```

# 配列拡張（前後に要素追加）
```js
const nums = [1, 2, 3];
const extendedNums = [0, ...nums, 4];
console.log(extendedNums); // [0, 1, 2, 3, 4]
```

# 配列の平均値を求める
```js
function calculateAverage(arr) {
  const sum = arr.reduce((acc, num) => acc + num ,0);
  return sum / arr.length;
}
console.log(calculateAverage([10, 20, 30, 40, 50])); // 30
```

# 最初の文字だけ大文字にする
```js
const str = "hello world";
const capitalized = str.charAt(0).toUpperCase() + str.slice(1);
console.log(capitalized); // Hello world
```

# 背景色をランダムに変える（DOM操作）
```js
const button = document.getElementById('button');
const colors = ['red', 'blue', 'green', 'yellow', 'purple'];
button.addEventListener('click', () => {
  const randomColor = colors[Math.floor(Math.random() * colors.length)];
  document.body.style.backgroundColor = randomColor;
});
```

# 非同期処理を順に実行
```js
async function step() {
  await step1();
  await step2();
  await step3();
  console.log("完了");
}
function step1() {
  return new Promise(resolve => setTimeout(() => { console.log("step1"); resolve(); }, 1000));
}
function step2() {
  return new Promise(resolve => setTimeout(() => { console.log("step2"); resolve(); }, 1000));
}
function step3() {
  return new Promise(resolve => setTimeout(() => { console.log("step3"); resolve(); }, 1000));
}
step();
```

# 最後の文字だけ出力する
```js
const str = "こんにちは";
const lastChar = str.slice(-1); // → "は"
```

# 親要素.append(key, value)：指定した親要素の最後に小要素やテキストを挿入する

```js
const formData = new FormData(); // ①作る
formData.append("name", "taro"); // ②追加する
fetch("/submit", { method: "POST", body: formData }); // ③送る
```

# 45日後を表示
```js
const today = new Date(); // 今日の日付
today.setDate(today.getDate() + 45); // 45日後に更新

// YYYY-MM-DD 形式に整形
const yyyy = today.getFullYear();
const mm = String(today.getMonth() + 1).padStart(2, '0'); // 月は0始まり
const dd = String(today.getDate()).padStart(2, '0');

const formatted = `${yyyy}-${mm}-${dd}`;
console.log(formatted); // 例: "2025-10-23"
```

# urlに含めると問題になる特殊文字（例：&, =, ?, /, 空白など）を正規化する
```js
const name = "山田=太郎";
const encoded = encodeURIComponent(name);
console.log(encoded); // → %E5%B1%B1%E7%94%B0%3D%E5%A4%AA%E9%83%8E

// 下記のような表示になる👇

https://example.com?name=%E5%B1%B1%E7%94%B0%3D%E5%A4%AA%E9%83%8E
```

# 削除ボタンを押したときに、そのボタンが属してるブロックを消したい
```html
<div class="item">
  <span>商品A</span>
  <button class="remove">削除</button>
</div>
```

```js
document.querySelectorAll('.remove').forEach(btn => {
  btn.addEventListener('click', function() {
    this.closest('.item').remove();
  });
});
// this はボタン。そこから一番近い .item(親要素) を探して削除
```

# 入力欄の中で、親の .form-group にエラー表示したい
```js
const input = document.querySelector('.email');
const group = input.closest('.form-group');
group.classList.add('has-error');
// 「この input が属してるフォームのまとまり」に対して処理したいとき
```

# クリックした要素が、どのセクションに属してるか知りたい
```js
element.closest('.section')
// 「どの質問ブロックの中でクリックされたか」を判定したいときに使える



// [closest の役割]
// ・削除ボタン → 親を消す	this.closest('.item').remove()
// ・入力欄 → 親にエラー表示	input.closest('.form-group')
// ・クリック位置 → セクション判定	element.closest('.section')
```


