# MYSQLコマンド
```sql
-- mysqlのコンテナに入る
**docker exec -it mysql bash**
```

```sql
-- mysql へユーザー名でアクセス
**mysql -u ユーザ名 -p**

-- ルートでアクセスする場合
**mysql root ユーザ名 -p**

-- 両方ともパスワードを入力
```

**データベースの作成**

```sql
**CREATE DATABASE データベース名;**
```

**データベースの一覧の確認**

```sql
**SHOW databases;**
```

**使用するデータベースの選択**

```sql
**USE データベース名;**
```

**使用しているデータベースの確認**

```sql
**SELECT database();**
```

**テーブルの作成**

```sql
**CREATE TABLE テーブル名（カラム名 データ型 制約）;**
```

**複数カラムを指定する時**

```sql
**CREATE TABLE テーブル名 (

カラム名1 データ型 制約,

カラム名2 データ型 制約,

カラム名3 データ型 制約
);**
```

**一つのデータを挿入**

```sql
**INSERT INTO users (カラム) VALUES ('データ');**
```

**複数のデータの挿入**

```sql
**INSERT INTO テーブル名 (カラム名) VALUES (データ１), (データ２), (データ３)**
```

**テーブルの一覧を表示する**

```sql
**SHOW tables;**
```

**データの作成**

```sql
**INSERT INTO テーブル名 (カラム名) VALUES (値);**
```

**データの一覧**

```sql
**SELECT カラム名 FROM テーブル名;**
```

**新しいカラム（列）を追加**

```sql
**ALTER TABLE users ADD COLUMN age INT;**
```

**カラムを削除**

```sql
**ALTER TABLE users DROP COLUMN age;**
```

**テーブルの詳細を確認する方法**

```sql
**SHOW COLUMNS FROM テーブル名;**
```

**データの更新**

```sql
**UPDATE テーブル名 SET カラム名=値;**

または

**UPDATE テーブル名 SET カラム名=変更後の値 WHERE カラム名=変更する値;**
```

**データの削除**

```sql
**DELETE FROM テーブル名;

または

DELETE FROM テーブル名 WHERE カラム名=値;**
```

**テーブルの詳細を確認する方法**

```sql
**SHOW COLUMNS FROM テーブル名;**
```

**テーブルごと削除**

```sql
**DROP TABLE users;**
```

**データベースの削除**

```sql
**DROP DATABASE データベース名;**
```

**ユニークなデータを取得（重複排除）**

```sql
**SELECT DISTINCT name FROM users;**
```

**並び替え**

```sql
**SELECT * FROM users ORDER BY age DESC;**
```

- `ASC` → 昇順（小さい順）
- `DESC` → 降順（大きい順）

**条件に一致するデータを検索**

```sql
**SELECT * FROM users WHERE age >= 20 AND age <= 30;**
```

**グループ化と集計**

```sql
-- 年齢ごとの人数を数える
**SELECT age, COUNT(*) AS 人数 FROM users GROUP BY age;**

-- 特定の条件に絞る（HAVING句）
**SELECT age, COUNT(*) FROM users GROUP BY age HAVING COUNT(*) >= 2;**
```

## **権限を付けるSQLコマンド**

#### **① ユーザーの作成**

```sql
**CREATE USER 'ユーザー名'@'ホスト名' IDENTIFIED BY 'パスワード';**
```

例：
```sql
**CREATE USER 'testuser'@'localhost' IDENTIFIED BY 'password123';**
```

- **'localhost'** → 同じサーバーからのみアクセス可能
- **'%'** → どこからでもアクセス可能

#### **② 権限を付与（GRANT文）**

```sql
**GRANT 権限一覧 ON データベース名.テーブル名 TO 'ユーザー名'@'ホスト名';**
```

例：`testuser`にすべての権限を付与

```sql
**GRANT ALL PRIVILEGES ON mydb.* TO 'testuser'@'localhost';**
```

## よく使う権限一覧

| 権限             | 説明                               |
| ---------------- | ---------------------------------- |
| `ALL PRIVILEGES` | すべての権限                       |
| `SELECT`         | データの読み取り                   |
| `INSERT`         | データの挿入                       |
| `UPDATE`         | データの更新                       |
| `DELETE`         | データの削除                       |
| `CREATE`         | 新しいテーブルやデータベースの作成 |
| `DROP`           | テーブルやデータベースの削除       |

---

#### **③ 権限を確認する**

```sql
**SHOW GRANTS FOR 'ユーザー名'@'ホスト名';**
```

例：
```sql
**SHOW GRANTS FOR 'testuser'@'localhost';**
```

#### **④ 権限を取り消す（REVOKE文）**

```sql
**REVOKE 権限一覧 ON データベース名.テーブル名 FROM 'ユーザー名'@'ホスト名';**
```

例：
```sql
**REVOKE ALL PRIVILEGES ON mydb.* FROM 'testuser'@'localhost';**
```

#### **⑤ ユーザーを削除**

```sql
**DROP USER 'ユーザー名'@'ホスト名';**
```
例：
```sql
**DROP USER 'testuser'@'localhost';**
```

#### **⑥ 権限の変更を反映**

```sql
**FLUSH PRIVILEGES;**
```
→ **権限を設定・変更したら必ず実行**


