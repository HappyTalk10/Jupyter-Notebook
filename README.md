# Jupyter Notebook on GitHub Codespaces

GitHub CodespacesでJupyter Notebookを使うための初学者向けガイドです。

## 📚 目次

- [GitHub Codespacesとは](#github-codespacesとは)
- [Jupyter Notebookとは](#jupyter-notebookとは)
- [始め方](#始め方)
- [基本的な使い方](#基本的な使い方)
- [よくある質問](#よくある質問)

## 🌟 GitHub Codespacesとは

GitHub Codespacesは、ブラウザ上で動作する開発環境です。自分のパソコンに何もインストールすることなく、すぐにプログラミングを始められます。

**メリット:**
- パソコンへのソフトウェアインストールが不要
- どこからでもアクセス可能
- 環境設定が簡単

## 📓 Jupyter Notebookとは

Jupyter Notebookは、コードと説明文を一緒に書けるツールです。データ分析や機械学習の学習に最適です。

**特徴:**
- コードをセル単位で実行できる
- 実行結果をその場で確認できる
- グラフや表を表示できる
- メモや説明を Markdown形式で記述できる

## 🚀 始め方

### 1. リポジトリの準備

```bash
# 新しいリポジトリを作成するか、既存のリポジトリを使用します
```

### 2. Codespaceの起動

1. GitHubのリポジトリページを開く
2. 緑色の「Code」ボタンをクリック
3. 「Codespaces」タブを選択
4. 「Create codespace on main」をクリック

数分待つと、ブラウザ上にVS Codeのような画面が表示されます。

### 3. Jupyter Notebookのインストール

ターミナル（画面下部）で以下のコマンドを実行します:

```bash
pip install jupyter notebook
```

または、`requirements.txt`ファイルを作成して以下を記述:

```
jupyter
notebook
```

そして以下のコマンドで一括インストール:

```bash
pip install -r requirements.txt
```

### 4. Jupyter Notebookの起動方法

**方法1: VS Code拡張機能を使う（推奨）**

1. 左側のメニューから「拡張機能」アイコンをクリック
2. 「Jupyter」で検索
3. 「Jupyter」をインストール
4. `.ipynb` ファイルを作成または開く

**方法2: ターミナルから起動**

```bash
jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser
```

表示されたURLをクリックしてアクセスします。

## 💡 基本的な使い方

### 新しいノートブックの作成

1. エクスプローラー（左側のファイル一覧）で右クリック
2. 「新しいファイル」を選択
3. ファイル名を `example.ipynb` のように `.ipynb` 拡張子で作成

### セルの種類

**Codeセル**: Pythonコードを書いて実行
```python
print("Hello, Jupyter!")
x = 10
y = 20
print(x + y)
```

**Markdownセル**: 説明文やメモを記述
```markdown
# 見出し
これは説明文です。
- リスト項目1
- リスト項目2
```

### セルの実行

- **Shift + Enter**: セルを実行して次のセルへ移動
- **Ctrl + Enter**: セルを実行してそのセルに留まる
- 左側の▶ボタン: セルを実行

### よく使うコード例

**簡単な計算:**
```python
# 変数の定義
a = 5
b = 3

# 計算
result = a + b
print(f"{a} + {b} = {result}")
```

**リストの作成と表示:**
```python
fruits = ["りんご", "バナナ", "オレンジ"]
for fruit in fruits:
    print(fruit)
```

**グラフの描画:**
```python
import matplotlib.pyplot as plt

x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

plt.plot(x, y)
plt.xlabel("X軸")
plt.ylabel("Y軸")
plt.title("簡単なグラフ")
plt.show()
```

## ❓ よくある質問

### Q1: Codespacesの無料枠はどれくらい？

個人アカウントでは月に60時間まで無料で使用できます（2コアマシンの場合）。

### Q2: 作業を保存するには？

ファイルは自動的に保存されますが、GitHubにコミット・プッシュすることで永続的に保存できます:

```bash
git add .
git commit -m "作業の保存"
git push
```

### Q3: パッケージが見つからないエラーが出る

必要なパッケージをインストールしてください:

```bash
pip install パッケージ名
```

例: `pip install pandas numpy matplotlib`

### Q4: Codespaceを停止するには？

1. 左下の「Codespaces」をクリック
2. 「Stop Current Codespace」を選択

または、GitHubのCodespacesページから停止できます。

### Q5: セルの実行順序を間違えた

「Kernel」メニューから「Restart & Clear Output」を選択し、最初から実行し直してください。

## 📦 便利なパッケージ

データ分析や機械学習でよく使うパッケージ:

```bash
pip install numpy pandas matplotlib seaborn scikit-learn
```

- **numpy**: 数値計算
- **pandas**: データ分析
- **matplotlib/seaborn**: グラフ描画
- **scikit-learn**: 機械学習

## 🎓 次のステップ

1. Pythonの基礎を学ぶ
2. データ分析の基本を学ぶ
3. 機械学習の入門を試す
4. 自分のプロジェクトを作成する

## 🔗 参考リンク

- [Jupyter Notebook公式ドキュメント](https://jupyter-notebook.readthedocs.io/)
- [GitHub Codespaces公式ドキュメント](https://docs.github.com/ja/codespaces)
- [Python公式チュートリアル](https://docs.python.org/ja/3/tutorial/)

---

**ハッピーコーディング！** 🚀

何か問題があれば、GitHubのIssuesで質問してください。