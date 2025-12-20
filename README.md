
# RailsプロジェクトをGitHubからcloneして揃える手順（WSL）

## 1. 既存の作業フォルダを退避（すでにローカルで環境構築した場合、未作成の場合2.から進める）

メンバーが既に `~/aiit_colab/rails_apps/team_app` を作っている前提です。上書きしないため、フォルダ名を変更してバックアップとして残します。

```bash
cd ~/aiit_colab/rails_apps
mv team_app team_app_backup_$(date +%Y%m%d_%H%M)
````

### 解説

* `mv` は削除ではなく「名前変更」です。バックアップとして残ります。
* `$(date +%Y%m%d_%H%M)` を付けることで、重複しないバックアップ名になります。

---

## 2. GitHubから clone

GitHub上の共通リポジトリをローカルに取得します。以後、作業は clone した `team_app` で行います（バックアップ側は触りません）。

```bash
git clone git@github.com:a25109te/team_app.git team_app
cd team_app
```

### 解説

* `git clone <URL> <保存先フォルダ名>`
  → `team_app` というフォルダを作って、その中にリポジトリを取得します。
* 以後、この `team_app` がチームで共有する正規の作業ディレクトリになります。

---

## 3. ブランチとリモートの確認

取得したリポジトリが正しい状態になっているか確認します。

```bash
git status -sb
git remote -v
```

### 期待例

* `## main...origin/main` のように出る
* `origin` が `github.com:a25109te/team_app.git` を指している

---

## 4. Railsの依存（Gem）をプロジェクト内に閉じ込める

Bundlerの依存パッケージを `vendor/bundle` に揃えます（プロジェクト内に閉じ込めて環境差を減らす）。

```bash
bundle _2.5.6_ config set --local path vendor/bundle
bundle _2.5.6_ install
```

### 解説

* `config set --local`：このプロジェクトだけの Bundler 設定を保存します
* `install`：`Gemfile.lock` に従って必要なGemをインストールします

---

## 5. DB作成（SQLite）

SQLiteの開発DBを作成します。

```bash
bin/rails db:create
```

### 解説

* SQLiteのDBファイルを作成します（通常 `db/development.sqlite3`）

---

## 6. 起動確認

Railsサーバを起動して、ブラウザで表示確認します。

```bash
bin/rails s -b 0.0.0.0 -p 3000
```

### ブラウザ

* [http://localhost:3000/](http://localhost:3000)

### 止める

* Ctrl + C

