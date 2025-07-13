# MYSQLコマンド
---

## MySQLに入る

```sql
-- mysqlのコンテナに入る
docker exec -it mysql bash

-- mysql にユーザー名でアクセス
mysql -u ユーザ名 -p

-- root でアクセス
mysql -u root -p
```

---

## データベース操作

```sql
-- データベース作成
CREATE DATABASE データベース名;

-- 一覧表示
SHOW DATABASES;

-- 使用するDBを選択
USE データベース名;

-- 現在のDBを確認
SELECT database();

-- DB削除
DROP DATABASE データベース名;
```

---

## テーブル操作

```sql
-- テーブル作成（単一）
CREATE TABLE テーブル名 (カラム名 データ型 制約);

-- テーブル作成（複数）
CREATE TABLE テーブル名 (
  カラム1 データ型 制約,
  カラム2 データ型,
  カラム3 データ型
);

-- 一覧表示
SHOW TABLES;

-- テーブル詳細
SHOW COLUMNS FROM テーブル名;

-- テーブル削除
DROP TABLE テーブル名;

-- カラム追加
ALTER TABLE テーブル名 ADD COLUMN カラム名 データ型;

-- カラム削除
ALTER TABLE テーブル名 DROP COLUMN カラム名;
```

---

## データ操作

```sql
-- データ挿入（1件）
INSERT INTO テーブル名 (カラム) VALUES ('データ');

-- データ挿入（複数）
INSERT INTO テーブル名 (カラム名) VALUES
  ('データ1'),
  ('データ2'),
  ('データ3');

-- データ一覧
SELECT * FROM テーブル名;

-- 特定カラム取得
SELECT カラム名 FROM テーブル名;

-- データ更新
UPDATE テーブル名 SET カラム=値;
UPDATE テーブル名 SET カラム=新値 WHERE カラム=条件;

-- データ削除
DELETE FROM テーブル名;
DELETE FROM テーブル名 WHERE カラム=値;
```

---

## 検索・絞り込み・並べ替え

```sql
-- 条件指定
SELECT * FROM users WHERE age >= 20 AND age <= 30;

-- 並び替え
SELECT * FROM users ORDER BY age DESC;

-- 重複排除
SELECT DISTINCT name FROM users;

-- 件数制限
SELECT * FROM users LIMIT 10;

-- NULLチェック
SELECT * FROM members WHERE email IS NULL;
SELECT * FROM users WHERE name IS NOT NULL;

-- あいまい検索
SELECT * FROM users WHERE name LIKE '%田%';

-- IN句
SELECT * FROM users WHERE age IN (20, 25, 30);

-- BETWEEN句
SELECT * FROM users WHERE age BETWEEN 20 AND 30;
```

---

## 集計・グループ化・サブクエリ

```sql
-- 集計
SELECT COUNT(*) FROM users;

-- グループ化
SELECT age, COUNT(*) FROM users GROUP BY age;

-- HAVING句
SELECT age, COUNT(*) FROM users GROUP BY age HAVING COUNT(*) >= 2;

-- サブクエリ
SELECT * FROM products WHERE price >= (SELECT AVG(price) FROM products);
```

---

## テーブル結合（JOIN）

```sql
-- INNER JOIN
SELECT u.name, o.amount FROM users u INNER JOIN orders o ON u.id = o.user_id;

-- LEFT JOIN
SELECT u.name, o.amount FROM users u LEFT JOIN orders o ON u.id = o.user_id;

-- RIGHT JOIN
SELECT u.name, o.amount FROM users u RIGHT JOIN orders o ON u.id = o.user_id;

-- FULL OUTER JOIN（MySQL標準では未対応: UNIONで代用）
SELECT name, amount FROM users LEFT JOIN orders ON users.id = orders.user_id
UNION
SELECT name, amount FROM users RIGHT JOIN orders ON users.id = orders.user_id;
```

---

## ユーザーと権限

```sql
-- ユーザー作成
CREATE USER 'ユーザー名'@'ホスト名' IDENTIFIED BY 'パスワード';

-- 権限付与
GRANT ALL PRIVILEGES ON データベース名.* TO 'ユーザー名'@'ホスト名';

-- 権限確認
SHOW GRANTS FOR 'ユーザー名'@'ホスト名';

-- 権限取消
REVOKE ALL PRIVILEGES ON データベース名.* FROM 'ユーザー名'@'ホスト名';

-- ユーザー削除
DROP USER 'ユーザー名'@'ホスト名';

-- 権限反映
FLUSH PRIVILEGES;
```

---

## よく使われるSQL構文まとめ表

| 構文              | 説明                   | 例                                                                                         | 解説                   |
| ----------------- | ---------------------- | ------------------------------------------------------------------------------------------ | ---------------------- |
| `SELECT`          | データの取得           | `SELECT * FROM users;`                                                                     | 全データ取得           |
| `WHERE`           | 条件指定               | `SELECT * FROM users WHERE age > 20;`                                                      | 年齢20歳超のみ取得     |
| `INSERT INTO`     | データ追加             | `INSERT INTO users (name, age) VALUES ('けんた', 22);`                                     | 新規追加               |
| `UPDATE`          | データ更新             | `UPDATE users SET age = 30 WHERE id = 1;`                                                  | id=1の年齢を30に更新   |
| `DELETE`          | データ削除             | `DELETE FROM users WHERE id = 1;`                                                          | id=1の削除             |
| `ORDER BY`        | 並び替え               | `SELECT * FROM users ORDER BY age DESC;`                                                   | 降順で並び替え         |
| `LIMIT`           | 件数制限               | `SELECT * FROM users LIMIT 10;`                                                            | 上位10件取得           |
| `DISTINCT`        | 重複除去               | `SELECT DISTINCT department FROM employees;`                                               | 部署一覧取得           |
| `GROUP BY`        | グループ化             | `SELECT dept, COUNT(*) FROM emp GROUP BY dept;`                                            | 部署ごとに集計         |
| `HAVING`          | グループ集計の絞り込み | `SELECT dept, COUNT(*) FROM emp GROUP BY dept HAVING COUNT(*) > 5;`                        | 5人超だけ              |
| `JOIN`            | 結合全般               | `SELECT u.name, o.amount FROM users u JOIN orders o ON u.id = o.user_id;`                  | users と orders を結合 |
| `LEFT JOIN`       | 左外部結合             | 上記参照                                                                                   | NULLも表示             |
| `RIGHT JOIN`      | 右外部結合             | 上記参照                                                                                   | orders基準             |
| `INNER JOIN`      | 内部結合               | 上記参照                                                                                   | 共通データのみ         |
| `OUTER JOIN`      | 両外部結合             | 上記参照                                                                                   | 両方全部               |
| `LIKE`            | あいまい検索           | `SELECT * FROM users WHERE name LIKE '%田%';`                                              | 「田」含む名前         |
| `IN`              | 複数条件               | `SELECT * FROM users WHERE age IN (20,25,30);`                                             | 複数一致               |
| `BETWEEN`         | 範囲検索               | `SELECT * FROM users WHERE age BETWEEN 20 AND 30;`                                         | 20〜30                 |
| `UNION`           | 結合（複数SELECT）     | `SELECT name FROM a UNION SELECT name FROM b;`                                             | 重複除去して結合       |
| `EXISTS`          | 存在確認               | `SELECT * FROM users WHERE EXISTS (SELECT * FROM orders WHERE users.id = orders.user_id);` | 関連存在時取得         |
| `CASE`            | 条件分岐               | `SELECT name, CASE WHEN age>=20 THEN '成人' ELSE '未成年' END AS type FROM users;`         | 年齢分岐               |
| `ALTER TABLE`     | テーブル変更           | `ALTER TABLE users ADD phone VARCHAR(15);`                                                 | カラム追加             |
| `DROP TABLE`      | テーブル削除           | `DROP TABLE old_data;`                                                                     | 削除                   |
| `SELECT COUNT(*)` | 件数取得               | `SELECT COUNT(*) FROM users;`                                                              | 行数取得               |
| `subquery`        | サブクエリ             | `SELECT * FROM products WHERE price >= (SELECT AVG(price) FROM products);`                 | 平均以上抽出           |
| `MAX(price)`      | 最大値取得             | `SELECT MAX(price) FROM products;`                                                         | 最大価格               |
| `MIN(price)`      | 最小値取得             | `SELECT MIN(price) FROM products;`                                                         | 最小価格               |
| `IS NULL`         | nullを取得             | `SELECT * FROM members WHERE email IS NULL;`                                               | nullな人               |
| `IS NOT NULL`     | null以外取得           | `SELECT * FROM users WHERE name IS NOT NULL;`                                              | nameあり               |

---

## サブクエリ vs HAVING の違い

| 用途         | サブクエリ       | HAVING                   |
| ------------ | ---------------- | ------------------------ |
| 目的         | 値を取得→絞込    | 集計結果→絞込            |
| 例え         | 平均点以上を探す | クラス平均80点以上を出す |
| SELECT内使用 | ✅                | ❌（GROUP BY後）          |

```sql
SELECT CASE
  WHEN 条件 THEN 値
  WHEN 条件 THEN 値
  ELSE 値
END FROM テーブル名;
```

---

## よく使うデータ型

| データ型           | 説明                 | 例                       |
| ------------------ | -------------------- | ------------------------ |
| `INT`              | 整数                 | `10, 200, -5`            |
| `VARCHAR(n)`       | 可変文字列           | `'たろう', 'abc123'`     |
| `TEXT`             | 長文                 | `'これは長文です...'`    |
| `DATE`             | 日付                 | `'2025-05-17'`           |
| `DATETIME`         | 日時                 | `'2025-05-17 15:30:00'`  |
| `TIMESTAMP`        | 自動日付             | `CURRENT_TIMESTAMP`      |
| `BOOLEAN`          | 真偽値               | `1（true） / 0（false）` |
| `FLOAT`, `DECIMAL` | 小数                 | `12.34, 99.99`           |
| `BLOB`             | バイナリ（画像など） | バイナリデータ           |
