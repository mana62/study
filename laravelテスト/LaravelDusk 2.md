# Laravel Dusk

## Laravel Duskとは
- クリック・入力・フォーム送信・認証・認可のテスト・画面遷移・非同期処理の待機などE２Eテストを行う <br>
- GitHub Actionsで自動化も可能
- 実行コマンド:`php artisan dusk`
- デメリットはテスト速度が遅い

---

## 対応ブラウザ
- Chrome（デフォルト）: ChromeDriverを使用
- Firefox: GeckoDriverを使用（設定が必要）
- Edge: EdgeDriverを使用（設定が必要）
- Safari: Safariの場合は追加設定が必要

---

## 使い分け
- PHPUnit: コントローラー、モデル、バリデーションなどのロジックテスト
- Dusk: ログイン画面、フォーム送信、JavaScriptの動作など、実際のユーザー操作

---

## 書き方の違い
- PHPUnit: $thisや$responseオブジェクトを使用
- Dusk: $browserオブジェクトを使って実際のブラウザ操作する点

---

## 違いのイメージ
| 比較項目     | PHPUnit                                  | Laravel Dusk                    |
| -------- | ---------------------------------------- | ------------------------------- |
| 主なテスト対象  | コードのロジック（関数・クラスなど）                       | 実際のブラウザ操作（UIの見た目・動作）            |
| 実行環境     | PHPのみ（サーバーサイド）                           | ChromeDriver（実ブラウザを自動操作）        |
| 基本クラス    | `Tests\TestCase`                         | `Tests\DuskTestCase`            |
| 例）ページ確認  | ✗ できない（HTML描画を見ない）                       | ✅ `$browser->assertSee('ログイン')` |
| 例）数値チェック | ✅ `$this->assertEquals(100, $user->age)` | ✗（通常は使わない）                      |


---

## セットアップ・インストール
```bash
## インストール・セットアップ
- インストール: `composer require --dev laravel/dusk`
- セットアップ: `php artisan dusk:install`
- テストファイル生成: `php artisan dusk:make LoginTest`
- ファイル配置場所: `tests/Browser/`
```

---

## よく使われる$browserのアサーション一覧
**ページ・URL関連**
| メソッド                | 説明            | 使用例                                                |
| ------------------- | ------------- | -------------------------------------------------- |
| `assertPathIs()`    | 現在のパスが一致するか   | `$browser->assertPathIs('/dashboard')`             |
| `assertPathIsNot()` | 現在のパスが一致しないか  | `$browser->assertPathIsNot('/login')`              |
| `assertRouteIs()`   | 現在のルート名が一致するか | `$browser->assertRouteIs('home')`                  |
| `assertUrlIs()`     | 完全なURLが一致するか  | `$browser->assertUrlIs('http://example.com/page')` |
| `assertSchemeIs()`  | URLスキームが一致するか | `$browser->assertSchemeIs('https')`                |
| `assertHostIs()`    | ホスト名が一致するか    | `$browser->assertHostIs('example.com')`            |
| `assertPortIs()`    | ポート番号が一致するか   | `$browser->assertPortIs(8000)`                     |

**表示・可視性関連**
| メソッド                  | 説明                     | 使用例                                         |
| --------------------- | ---------------------- | ------------------------------------------- |
| `assertSee()`         | テキストが表示されているか          | `$browser->assertSee('ようこそ')`               |
| `assertDontSee()`     | テキストが表示されていないか         | `$browser->assertDontSee('エラー')`            |
| `assertSeeIn()`       | 特定の要素内にテキストがあるか        | `$browser->assertSeeIn('.alert', '成功')`     |
| `assertDontSeeIn()`   | 特定の要素内にテキストがないか        | `$browser->assertDontSeeIn('.error', 'OK')` |
| `assertSeeLink()`     | リンクテキストが表示されているか       | `$browser->assertSeeLink('ログアウト')`          |
| `assertDontSeeLink()` | リンクテキストが表示されていないか      | `$browser->assertDontSeeLink('管理画面')`       |
| `assertVisible()`     | 要素が可視状態か               | `$browser->assertVisible('#modal')`         |
| `assertPresent()`     | 要素がDOM上に存在するか（非表示でもOK） | `$browser->assertPresent('#hidden-div')`    |
| `assertMissing()`     | 要素がDOM上に存在しないか         | `$browser->assertMissing('.deleted-item')`  |

**フォーム・入力関連**
| メソッド                       | 説明                   | 使用例                                                                  |
| -------------------------- | -------------------- | -------------------------------------------------------------------- |
| `assertInputValue()`       | input要素の値が一致するか      | `$browser->assertInputValue('email', 'test@example.com')`            |
| `assertInputValueIsNot()`  | input要素の値が一致しないか     | `$browser->assertInputValueIsNot('password', '')`                    |
| `assertChecked()`          | チェックボックスがチェック済みか     | `$browser->assertChecked('agree')`                                   |
| `assertNotChecked()`       | チェックボックスが未チェックか      | `$browser->assertNotChecked('newsletter')`                           |
| `assertRadioSelected()`    | ラジオボタンが選択されているか      | `$browser->assertRadioSelected('gender', 'male')`                    |
| `assertRadioNotSelected()` | ラジオボタンが選択されていないか     | `$browser->assertRadioNotSelected('gender', 'other')`                |
| `assertSelected()`         | selectで選択されているか      | `$browser->assertSelected('country', 'japan')`                       |
| `assertNotSelected()`      | selectで選択されていないか     | `$browser->assertNotSelected('country', 'usa')`                      |
| `assertSelectHasOptions()` | selectに指定のoptionがあるか | `$browser->assertSelectHasOptions('status', ['active', 'inactive'])` |
| `assertSelectHasOption()`  | selectに1つのoptionがあるか | `$browser->assertSelectHasOption('role', 'admin')`                   |


**要素の状態関連**
| メソッド                     | 説明           | 使用例                                              |
| ------------------------ | ------------ | ------------------------------------------------ |
| `assertEnabled()`        | 要素が有効状態か     | `$browser->assertEnabled('submit-button')`       |
| `assertDisabled()`       | 要素が無効状態か     | `$browser->assertDisabled('submit-button')`      |
| `assertButtonEnabled()`  | ボタンが有効状態か    | `$browser->assertButtonEnabled('保存')`            |
| `assertButtonDisabled()` | ボタンが無効状態か    | `$browser->assertButtonDisabled('削除')`           |
| `assertFocused()`        | 要素にフォーカスがあるか | `$browser->assertFocused('input[name="email"]')` |
| `assertNotFocused()`     | 要素にフォーカスがないか | `$browser->assertNotFocused('#password')`        |

**属性・クラス関連**
| メソッド                    | 説明              | 使用例                                                          |
| ----------------------- | --------------- | ------------------------------------------------------------ |
| `assertAttribute()`     | 要素の属性値が一致するか    | `$browser->assertAttribute('.btn', 'disabled', 'true')`      |
| `assertDataAttribute()` | data属性の値が一致するか  | `$browser->assertDataAttribute('#item', 'id', '123')`        |
| `assertAriaAttribute()` | aria属性の値が一致するか  | `$browser->assertAriaAttribute('.modal', 'hidden', 'false')` |
| `assertHasClass()`      | 要素が指定のクラスを持つか   | `$browser->assertHasClass('.alert', 'alert-success')`        |
| `assertMissingClass()`  | 要素が指定のクラスを持たないか | `$browser->assertMissingClass('.alert', 'alert-error')`      |


**認証・Cookie関連**
| メソッド                      | 説明                    | 使用例                                          |
| ------------------------- | --------------------- | -------------------------------------------- |
| `assertAuthenticated()`   | ユーザーが認証済みか            | `$browser->assertAuthenticated()`            |
| `assertGuest()`           | ユーザーが未認証か             | `$browser->assertGuest()`                    |
| `assertAuthenticatedAs()` | 特定のユーザーで認証済みか         | `$browser->assertAuthenticatedAs($user)`     |
| `assertCookie()`          | Cookieが存在するか          | `$browser->assertCookie('session_token')`    |
| `assertPlainCookie()`     | 暗号化されていないCookieが存在するか | `$browser->assertPlainCookie('preference')`  |
| `assertCookieMissing()`   | Cookieが存在しないか         | `$browser->assertCookieMissing('temp_data')` |

**ソースコード関連**
| メソッド                    | 説明                    | 使用例                                                    |
| ----------------------- | --------------------- | ------------------------------------------------------ |
| `assertSourceHas()`     | ページソースに文字列が含まれるか      | `$browser->assertSourceHas('<meta name="csrf-token"')` |
| `assertSourceMissing()` | ページソースに文字列が含まれないか     | `$browser->assertSourceMissing('debug-info')`          |
| `assertScript()`        | JavaScriptの実行結果が一致するか | `$browser->assertScript('window.App.version', '1.0')`  |

**特殊なアサーション**
| メソッド                        | 説明                   | 使用例                                                             |
| --------------------------- | -------------------- | --------------------------------------------------------------- |
| `assertTitle()`             | ページタイトルが一致するか        | `$browser->assertTitle('ホーム - MyApp')`                          |
| `assertTitleContains()`     | ページタイトルに文字列が含まれるか    | `$browser->assertTitleContains('MyApp')`                        |
| `assertDialogOpened()`      | ダイアログ（alert等）が開いているか | `$browser->assertDialogOpened('本当に削除しますか？')`                    |
| `assertVue()`               | Vueコンポーネントのプロパティ値を確認 | `$browser->assertVue('user.name', 'John', '@user-component')`   |
| `assertVueIsNot()`          | Vueプロパティが特定の値でないか    | `$browser->assertVueIsNot('loading', true, '@component')`       |
| `assertVueContains()`       | Vue配列プロパティに値が含まれるか   | `$browser->assertVueContains('items', 'apple', '@list')`        |
| `assertVueDoesNotContain()` | Vue配列に値が含まれないか       | `$browser->assertVueDoesNotContain('items', 'banana', '@list')` |


**操作系**
| メソッド | 説明 | 使用例 |
|---------|------|--------|
| `visit()` | ページに移動 | `$browser->visit('/home')` |
| `click()` | 要素をクリック | `$browser->click('.button')` |
| `type()` | テキスト入力 | `$browser->type('email', 'test@example.com')` |
| `select()` | selectで選択 | `$browser->select('country', 'japan')` |
| `check()` | チェックボックスをON | `$browser->check('agree')` |
| `uncheck()` | チェックボックスをOFF | `$browser->uncheck('newsletter')` |
| `press()` | ボタンを押す | `$browser->press('送信')` |
| `waitFor()` | 要素が表示されるまで待つ | `$browser->waitFor('.modal')` |
| `pause()` | 指定時間停止(ms) | `$browser->pause(1000)` |


**非同期処理**
| メソッド | 説明 |
|---------|------|
| `waitFor('.selector')` | 要素が表示されるまで待つ |
| `waitUntilMissing('.selector')` | 要素が消えるまで待つ |
| `waitForText('テキスト')` | テキストが表示されるまで待つ |
| `waitForLocation('/path')` | 特定のURLに遷移するまで待つ |
| `waitForRoute('route.name')` | 特定のルートに遷移するまで待つ |

---

## よく使う実践例
```php
$this->browse(function ($browser) {
    $browser->visit('/login')
            ->assertSee('ログイン')                    // テキスト確認
            ->type('email', 'test@example.com')       // 入力
            ->type('password', 'password')
            ->press('ログイン')                        // ボタンクリック
            ->assertPathIs('/dashboard')               // URL確認
            ->assertAuthenticated()                    // 認証確認
            ->assertSeeIn('.welcome', 'ようこそ')      // 特定要素内確認
            ->assertVisible('.user-menu');             // 要素の可視性確認
});
```

---

## トラブルシューティン(よくあるエラーと対処法)
- **Chrome/ChromeDriverのバージョン不一致**: `php artisan dusk:chrome-driver --detect`で解決
- **要素が見つからない**: `waitFor()`を使って要素の読み込みを待つ
- **テストが遅い**: 並列実行 `php artisan dusk --parallel`
- **スクリーンショット**: 失敗時は自動的に`tests/Browser/screenshots/`に保存される

---

## 補足
- ヘッドレスモード: `DuskTestCase.php`で設定可能（CI/CD環境で便利）
- 複数ブラウザ: `$this->browse(function ($first, $second) {})`で同時操作可能
- ページオブジェクト: `php artisan dusk:page LoginPage`で再利用可能なページクラスを作成