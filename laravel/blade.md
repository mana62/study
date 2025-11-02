## @stack: コンテンツの「受け皿」
- **「ここに何か追加するかもしれないから、場所を空けておいてね」**という合図
- たとえば、ウェブページのヘッダー部分に「追加のJavaScriptを後で入れるためのスペース」を確保しておくイメージ

```bash
# resources/views/layouts/app.blade.phpなど

...
    @stack('scripts')
</body>
```

---

## @push: コンテンツを「入れる」
- **「@stackで、空けておいた場所に、この内容を入れてね」**という具体的な指示
- 複数の場所で@pushを使うと、その内容がすべて@stackで空けたスペースに集められて表示される

```bash
@push('scripts')
    <script src="my-script.js"></script>
    # このように書くと、my-script.jsが自動的にヘッダー部分に追加される
@endpush
```
---

## @once: 1回だけ「入れる」
- **「この内容は、もし複数回指示されても、絶対に1回だけしか表示しないでね」**という特別な指示
- 複数のファイルで同じJavaScriptライブラリを読み込む必要がある場合でも、@onceを使えば、そのライブラリがページに重複して読み込まれるのを防げる

```bash
@once
    <script src="jquery.js"></script>
    # コードをどのファイルに書いても、jquery.jsはページ全体で一度だけしか読み込まれない
@endonce
```
---

## @can: 認可チェック
- このユーザーがこの操作をしていいか？を判定して、表示を切り替えるために使う

```bash
@can('update', $post)
    <a href="{{ route('posts.edit', $post) }}">編集する</a>
@endcan
# ログイン中のユーザーが $post を 更新できる権限を持っていれば、リンクが表示される
```

### どこで権限を定義するのか
- 通常は Policyクラスで定義する

```bash
public function update(User $user, Post $post)
{
    return $user->id === $post->user_id;
}
```
→ つまり、@can('update', $post) はこの update() メソッドを呼び出して判定してる

---

## まとめ
- @stack：コンテンツを入れる場所を用意する
- @push：その用意した場所にコンテンツを追加する
- @once：同じ内容を一度だけ表示させる
- @can： 認可チェック

---

# $loopについて
| 式                       | 意味・使い方                   | 使用例                      |
| ------------------------ | ------------------------------ | --------------------------- |
| `{{ $loop->iteration }}` | ループ回数を1から返す（1,2,3） | `@foreach($items as $item)` |
