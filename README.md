# GitHub Codespaces × Jupyter Notebook - 脱初心者ガイド

## 🎯 このガイドについて

基本的なJupyter Notebookの使い方は理解している方向けに、GitHub Codespaces環境でより効率的に開発するためのテクニックとベストプラクティスをまとめました。

## 📋 目次

- [環境構築の最適化](#環境構築の最適化)
- [devcontainer.jsonの活用](#devcontainerjsonの活用)
- [拡張機能の推奨設定](#拡張機能の推奨設定)
- [効率的なワークフロー](#効率的なワークフロー)
- [パフォーマンス最適化](#パフォーマンス最適化)
- [チーム開発での運用](#チーム開発での運用)

## 🚀 環境構築の最適化

### devcontainer.jsonの配置

プロジェクトルートに `.devcontainer/devcontainer.json` を配置することで、チーム全体で統一された環境を構築できます。

```json
{
  "name": "Python Data Science",
  "image": "mcr.microsoft.com/devcontainers/python:3.11",
  "features": {
    "ghcr.io/devcontainers/features/python:1": {
      "version": "3.11"
    }
  },
  "customizations": {
    "vscode": {
      "extensions": [
        "ms-python.python",
        "ms-toolsai.jupyter",
        "ms-python.vscode-pylance"
      ],
      "settings": {
        "python.defaultInterpreterPath": "/usr/local/bin/python",
        "jupyter.askForKernelRestart": false
      }
    }
  },
  "postCreateCommand": "pip install -r requirements.txt",
  "forwardPorts": [8888],
  "remoteUser": "vscode"
}
```

### requirements.txtの管理

依存関係を明確に管理し、バージョンを固定することで再現性を確保します。

```txt
jupyter==1.0.0
jupyterlab==4.0.0
numpy==1.24.3
pandas==2.0.3
matplotlib==3.7.2
seaborn==0.12.2
scikit-learn==1.3.0
ipywidgets==8.1.0
```

## ⚙️ devcontainer.jsonの活用

### 環境変数の設定

機密情報はGitHub Secretsを使用し、環境変数として注入します。

```json
{
  "remoteEnv": {
    "PYTHONPATH": "${containerWorkspaceFolder}/src",
    "DATA_DIR": "${containerWorkspaceFolder}/data"
  }
}
```

### ライフサイクルコマンド

開発環境の初期化を自動化します。

```json
{
  "postCreateCommand": "bash .devcontainer/setup.sh",
  "postStartCommand": "jupyter lab --ip=0.0.0.0 --port=8888 --no-browser --allow-root &"
}
```

## 🔧 拡張機能の推奨設定

### Jupyter拡張機能の設定

`.vscode/settings.json` で細かな動作をカスタマイズします。

```json
{
  "jupyter.interactiveWindow.textEditor.executeSelection": true,
  "jupyter.notebookFileRoot": "${workspaceFolder}",
  "jupyter.widgetScriptSources": ["jsdelivr.com", "unpkg.com"],
  "notebook.output.textLineLimit": 30,
  "notebook.cellToolbarLocation": {
    "default": "right",
    "jupyter-notebook": "left"
  }
}
```

### 推奨拡張機能リスト

`.vscode/extensions.json` を作成してチームで共有します。

```json
{
  "recommendations": [
    "ms-python.python",
    "ms-toolsai.jupyter",
    "ms-python.vscode-pylance",
    "charliermarsh.ruff",
    "njpwerner.autodocstring",
    "kevinrose.vsc-python-indent"
  ]
}
```

## ⚡ 効率的なワークフロー

### カーネルの管理

複数のカーネルを使い分ける場合は、`ipykernel`を使用して仮想環境ごとにカーネルを登録します。

```bash
# 仮想環境内で実行
python -m ipykernel install --user --name=myenv --display-name "Python (myenv)"
```

### Notebookのモジュール化

共通処理は `.py` ファイルとして切り出し、Notebookから読み込みます。

```python
# src/utils.py
import pandas as pd

def load_data(filepath):
    """データ読み込みの共通関数"""
    return pd.read_csv(filepath)

# Notebook内
%load_ext autoreload
%autoreload 2

from src.utils import load_data
df = load_data('data/sample.csv')
```

### マジックコマンドの活用

```python
# 実行時間の計測
%timeit your_function()

# プロファイリング
%prun your_function()

# シェルコマンドの実行
!ls -la data/

# 変数の確認
%whos

# デバッグモード
%debug
```

## 🎨 パフォーマンス最適化

### メモリ使用量の監視

```python
# メモリ使用量の表示
%load_ext memory_profiler
%memit df.groupby('category').sum()
```

### 大規模データの扱い

```python
# チャンクでの読み込み
chunk_size = 10000
for chunk in pd.read_csv('large_file.csv', chunksize=chunk_size):
    process(chunk)

# データ型の最適化
df['int_col'] = df['int_col'].astype('int32')
df['category_col'] = df['category_col'].astype('category')
```

### 並列処理

```python
from joblib import Parallel, delayed

results = Parallel(n_jobs=-1)(
    delayed(process_item)(item) for item in items
)
```

## 👥 チーム開発での運用

### Notebookのバージョン管理

出力結果を含めないようにする `.gitattributes` の設定：

```
*.ipynb filter=jupyter_clear_output
```

Git filterの設定：

```bash
# Notebookの出力をクリアするフィルター設定
git config --global filter.jupyter_clear_output.clean \
  "jupyter nbconvert --ClearOutputPreprocessor.enabled=True --to=notebook --stdin --stdout --log-level=ERROR"
```

または、`nbstripout`を使用：

```bash
pip install nbstripout
nbstripout --install
```

### コードレビューのポイント

1. **セルの実行順序**: `Run All`で最初から最後まで実行できることを確認
2. **ハードコードの排除**: パスやパラメータは設定ファイルまたは環境変数から読み込む
3. **出力の適切性**: 不要な大量出力は削除
4. **ドキュメント**: Markdownセルで処理の意図を明確に記述

### CI/CDへの組み込み

GitHub Actionsで自動テストを実行：

```yaml
name: Test Notebooks

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v4
        with:
          python-version: '3.11'
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          pip install nbconvert pytest
      - name: Execute notebooks
        run: |
          jupyter nbconvert --to notebook --execute notebooks/*.ipynb
```

## 📊 デバッグテクニック

### インタラクティブデバッガ

```python
# セル内でブレークポイント設定
from IPython.core.debugger import set_trace
set_trace()

# または Python 3.7+
breakpoint()
```

### 変数インスペクタ

JupyterLabの変数インスペクタ拡張機能を有効化：

```bash
jupyter labextension install @lckr/jupyterlab_variableinspector
```

## 🔒 セキュリティのベストプラクティス

1. **機密情報の管理**: `.env`ファイルと`python-dotenv`を使用
2. **Secretsの利用**: GitHub Secretsを環境変数として注入
3. **出力のサニタイズ**: 機密情報を含む出力は削除してからコミット

```python
# .envファイルの読み込み
from dotenv import load_dotenv
import os

load_dotenv()
api_key = os.getenv('API_KEY')
```

## 🛠️ トラブルシューティング

### カーネルが起動しない

```bash
# カーネルスペックの確認
jupyter kernelspec list

# カーネルの再インストール
pip install --upgrade ipykernel
python -m ipykernel install --user
```

### パッケージが見つからない

```python
# 現在のPythonパスを確認
import sys
print(sys.executable)
print(sys.path)

# pip installは同じPythonを使用
import sys
!{sys.executable} -m pip install package_name
```

## 📚 参考リソース

- [GitHub Codespaces Documentation](https://docs.github.com/en/codespaces)
- [Dev Container Specification](https://containers.dev/)
- [JupyterLab Documentation](https://jupyterlab.readthedocs.io/)
- [IPython Documentation](https://ipython.readthedocs.io/)

---

**作成日**: 2025年  
**対象**: 脱初心者レベルのデータサイエンティスト・エンジニア