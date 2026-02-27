# Laravelのアーキテクチャ

---

## 主なアーキテクチャと種類

| 名称                  | 構成層                                                                      | 特徴・用途                                                      |
| ------------------- | ------------------------------------------------------------------------ | ---------------------------------------------------------- |
| **① MVC**           | Model, View, Controller                                                  | Laravelの基本構造。小～中規模開発に最適。ServiceやRepositoryを追加しなくても動く      |
| **② 3層アーキテクチャ**     | Controller（Presentation）<br>Service（Business）<br>Repository（Data Access） | MVCを整理した形。ビジネスロジックとDB操作を分ける構造。中規模〜大規模でよく使われる              |
| **③ 4層アーキテクチャ**     | Controller → Service → Repository → Model                                | Laravelでの実践的構成。RepositoryでEloquentをカプセル化して保守性を高める         |
| **④ DDD（ドメイン駆動設計）** | Application, Domain, Infrastructure, Presentation                        | ドメイン（業務知識）を中心に設計。超大規模や複雑な業務システムで採用                        |
| **⑤ クリーンアーキテクチャ**   | Entities, Use Cases, Interface Adapters, Frameworks                      | DDDをさらに抽象化。「依存方向を内側に向ける」＝どの層もビジネスロジックに依存。テスト容易・再利用性◎       |
| **⑥ MVVM / MVP**    | Model, View, ViewModel / Presenter                                       | フロントエンドやモバイルアプリで使われる構造（Vue.js, Reactなど）。Laravelにはあまり登場しない |

---

## Laravel でよく使われる層構成

| 開発規模                  | 採用される構成                      | 説明                            |
| --------------------- | ---------------------------- | ----------------------------- |
| 小規模（個人開発・簡単なCRUD）     | **MVCだけ**                    | Controllerに直接Modelを呼び出す。最小構成。 |
| 中規模（中小企業アプリ）          | **Service層追加（MVC+Service）**  | ロジックを分離して保守性UP               |
| 大規模（複数ドメイン・長期運用）      | **Service + Repository（4層）** | データアクセスをカプセル化。DB変更にも強い       |
| 超大規模（業務システム・マイクロサービス） | **DDD / クリーンアーキテクチャ**        | ビジネスルールを中心に設計。複数チームで開発可能     |

---

```bash
MVC（最小構成）
┌────────┬────────┬────────┐
│ Controller │  View  │  Model  │
└────────┴────────┴────────┘

3層
Controller → Service → Repository

4層
Controller → Service → Repository → Model

DDD / クリーンアーキテクチャ（概念的）
Presentation → Application → Domain → Infrastructure
```

---

## まとめ

| 分類                                    | 書く場所                   | 内容の種類                   | 具体例                                                         |
| ------------------------------------- | ---------------------- | ----------------------- | ----------------------------------------------------------- |
| **データ構造・属性の変換**                       | **Model（モデル）**         | カラム定義・アクセサ・ミューテタ・リレーション | `$fillable`, `getNameAttribute()`, `setPasswordAttribute()` |
| **データの取得・検索（where, orderBy, joinなど）** | **Repository（リポジトリ）**  | DBクエリや集計処理              | `User::where('is_active', true)->get()`                     |
| **ビジネス処理（複数の操作の組み合わせ）**               | **Service（サービス）**      | 割引・合計金額計算・複数リポジトリ呼び出し   | `$total = $orderRepository->sumItems();`                    |
| **表示やリクエスト処理**                        | **Controller（コントローラ）** | リクエスト受付・レスポンス返却         | `$users = $userService->getActiveUsers();`                  |



| 処理の種類              | 書く場所              |
| ------------------ | ----------------- |
| データの形・変換・リレーション    | 🟢 **Model**      |
| データの取得・検索・保存       | 🔵 **Repository** |
| ビジネスロジック・複数リポジトリ連携 | 🟣 **Service**    |
| リクエスト受け取り・レスポンス返却  | 🟠 **Controller** |
