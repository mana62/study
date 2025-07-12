# gitコマンド
### git stash系
```bash
git stash drop               # スタッシュを1件削除
git stash clear              # 全てのスタッシュを削除
git stash                   # 作業を一時保存
git stash list              # 保存したスタッシュ一覧
git stash apply             # スタッシュを適用（削除しない）
git stash push -m "msg" -- path/to/file  # ファイルを指定して部分スタッシュ
```

---

### コミット操作
```bash
git commit --amend                      # 直前のコミットを修正
git reset --soft HEAD~1                # 直前のコミット取消、作業状態・ステージは維持
git reset --hard HEAD~1                # 完全に巻き戻し、作業状態も破棄
git revert <コミットID>                # コミットを打ち消す新しいコミットを作成
git log --oneline                      # コミットログの簡易表示
```

---

### rebase（インタラクティブ）
```bash
git rebase -i HEAD~2                   # 直前2件を1つにまとめる
# squash でコミットまとめ、editで再編集
```

### cherry-pick
```bash
git cherry-pick <コミットID>          # 特定のコミットだけを取り込む
```

---

### タグ関連
```bash
git tag -a v1.0 -m "初リリース"       # タグ作成
git tag                                # 一覧表示
git tag -l "v1.*"                      # パターン検索
git show タグ名                        # タグ詳細
git push origin タグ名                 # タグ送信
git push origin --tags                 # 全タグ送信
git tag タグ名 コミットID             # 後から付ける
```

---

### ブランチ操作
```bash
git branch -d ブランチ名              # ローカルブランチ削除
git push origin --delete ブランチ名   # リモートブランチ削除
git branch -m old new                 # ブランチ名変更
git switch -c ブランチ名              # 新規ブランチ作成
```

---

### ファイルの復元・取り消し
```bash
git restore <ファイル名>              # ステージ前の変更を元に戻す
git checkout HEAD^ -- <ファイル名>    # 直前コミットの状態に戻す
```

---

### 他ブランチとの統合
```bash
git merge origin/main                 # mainの最新を取り込む
git merge origin/develop              # developの最新を取り込む
git rebase origin/main                # mainに対してrebaseする
```

---

### コンフリクト対応
```bash
# === で衝突部分を手動解決
git add .
git rebase --continue
# push には --force オプション必要
git push --force-with-lease
```

---

### mainにコミットしてしまった場合の修正
```bash
git switch -c feature/your-branch     # ブランチを切る
git push origin feature/your-branch

# mainに戻ってコミット削除
git switch main
git reset --hard HEAD~1
```

---

### マージ取消（developに誤ってマージ）
```bash
git log --oneline
# マージコミットIDを確認
git revert -m 1 <マージコミットID>
git push origin develop
```

---

### コミット1つにまとめる手順（push含む）
```bash
git rebase -i HEAD~3                  # squash で1つに
# エディタでpick → squash
git push --force-with-lease
```

---

### ログと差分
```bash
git fetch origin
git log origin/develop                # リモートログ確認
git log HEAD..origin/develop         # 差分確認

git diff main..develop               # ブランチ間差分
```

---

### ファイル操作
```bash
git reset HEAD <ファイル名>          # addの取り消し
git restore <ファイル名>             # ステージ戻す
git checkout <コミットID> -- <ファイル>  # 特定の時点に戻す
```

---

