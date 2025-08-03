# Gemini CLI Dev Container Environment

このDev Container環境は、Google Gemini CLIを簡単に使い始めるための完全な開発環境を提供します。

## 🚀 クイックスタート

### 前提条件
- Docker Desktop または Docker Engine
- Visual Studio Code
- VS Code Dev Containers 拡張機能

### セットアップ手順

1. **プロジェクトフォルダを作成**
```bash
mkdir gemini-cli-devcontainer
cd gemini-cli-devcontainer
```

2. **必要なファイルを配置**
以下のディレクトリ構造を作成:
```
gemini-cli-devcontainer/
├── .devcontainer/
│   ├── devcontainer.json
│   └── Dockerfile
├── .gemini/
│   └── settings.json
├── .env.example
├── .gitignore
└── README.md
```

3. **VS Codeで開く**
```bash
code .
```

4. **Dev Containerで再開**
- VS Codeで `Cmd/Ctrl + Shift + P` を押す
- 「Dev Containers: Reopen in Container」を選択
- コンテナのビルドが完了するまで待つ（初回は数分かかります）

5. **環境の確認**
コンテナが起動したら、ターミナルで以下を実行:
```bash
gemini --version
```

## 🔐 認証方法

### 方法1: ブラウザ認証（推奨）

最も簡単な方法で、無料枠（60リクエスト/分、1000リクエスト/日）が利用可能です。

1. ターミナルで `gemini` を実行
2. テーマを選択
3. 「Login with Google」を選択
4. 表示されたURLをブラウザで開く
5. Googleアカウントでログイン
6. 認証完了後、ターミナルに戻る

**注意**: Dev Container環境でのブラウザ認証の場合、以下の手順が必要な場合があります：

1. URLが表示されたら、それをコピー
2. ホストマシンのブラウザで開く
3. ログイン後のリダイレクトURLをコピー
4. ターミナルに戻って貼り付け

### 方法2: API Key認証

より高い制限が必要な場合や、CI/CD環境での使用に適しています。

1. **API Keyの取得**
   - [Google AI Studio](https://aistudio.google.com/app/apikey)にアクセス
   - 「Create API key」をクリック
   - プロジェクトを選択または作成
   - API Keyをコピー

2. **環境変数の設定**

   **オプションA: .envファイル（推奨）**
   ```bash
   cp .env.example .env
   ```
   `.env`ファイルを編集:
   ```
   GEMINI_API_KEY=your-api-key-here
   ```

   **オプションB: exportコマンド**
   ```bash
   export GEMINI_API_KEY="your-api-key-here"
   ```

   **オプションC: VS Code設定**
   `.devcontainer/devcontainer.json`の`containerEnv`セクションで設定

3. **Gemini CLIの起動**
   ```bash
   gemini
   ```
   認証方法で「Gemini API Key」を選択

### 方法3: Vertex AI認証

Google Cloud Platformを使用している場合:

```bash
export GOOGLE_API_KEY="your-google-api-key"
export GOOGLE_CLOUD_PROJECT="your-project-id"
export GOOGLE_GENAI_USE_VERTEXAI=true
```

## 📝 使用例

### 基本的な使い方

**対話モード:**
```bash
gemini
> プロジェクトの構造を説明してください
> このコードのバグを見つけて修正してください @main.js
> READMEファイルを生成してください
```

**非対話モード:**
```bash
gemini -p "このファイルの要約を作成してください @package.json"
```

**パイプライン:**
```bash
cat error.log | gemini -p "このエラーログを分析してください"
```

### コマンド例

```bash
# ファイルを参照
> このファイルを最適化してください @src/index.js

# ディレクトリ全体を分析
> srcディレクトリの構造を説明してください @src/

# Webコンテンツを取得
> https://example.com の内容を要約してください

# コード生成
> Express.jsでREST APIサーバーを作成してください

# デバッグ支援
> このエラーメッセージの原因と解決方法を教えてください
```

## 🛠️ カスタマイズ

### settings.jsonの編集

`.gemini/settings.json`ファイルで動作をカスタマイズ:

```json
{
  "theme": "GitHub",
  "model": "gemini-2.5-pro",
  "autoAccept": false,
  "sandbox": "docker"
}
```

### GEMINI.mdファイル

プロジェクト固有の指示を`.gemini/GEMINI.md`に記述:

```markdown
# プロジェクト: My App

## コーディング規約
- TypeScriptを使用
- ESLintルールに従う
- テストを必ず書く

## アーキテクチャ
- Clean Architecture を採用
- DIコンテナを使用
```

## 🔧 トラブルシューティング

### ブラウザ認証が動作しない場合

1. **ポートフォワーディングの確認**
   - VS Codeの「PORTS」タブで3000, 5000ポートが転送されているか確認
   
2. **手動でURLを開く**
   - 表示されたURLをコピーしてホストのブラウザで開く
   
3. **API Key認証に切り替える**
   - より確実な方法として、API Key認証を使用

### API Keyが認識されない場合

1. **環境変数の確認**
   ```bash
   echo $GEMINI_API_KEY
   ```

2. **.envファイルの位置**
   - プロジェクトルートまたは`~/.gemini/`に配置

3. **コンテナの再起動**
   - 環境変数を変更した後は、コンテナを再ビルド

### レート制限に達した場合

- 無料枠: 60リクエスト/分、1000リクエスト/日
- API Key使用時は制限が緩和される
- Vertex AI使用時はさらに高い制限

## 📚 リソース

- [Gemini CLI GitHub](https://github.com/google-gemini/gemini-cli)
- [Gemini API Documentation](https://ai.google.dev/gemini-api/docs)
- [Google AI Studio](https://aistudio.google.com)
- [Vertex AI](https://cloud.google.com/vertex-ai)

## 💡 Tips

1. **プロジェクトコンテキスト**: `GEMINI.md`ファイルを使用してプロジェクト固有の情報を提供
2. **ファイル参照**: `@`記号を使用してファイルやディレクトリを参照
3. **コマンド実行**: `/shell`コマンドでシェルコマンドを実行
4. **メモリ機能**: `/save_memory`で重要な情報を保存
5. **セッション復元**: `/restore`でファイル変更を元に戻す

## 🤝 サポート

問題が発生した場合:
1. [Gemini CLI Issues](https://github.com/google-gemini/gemini-cli/issues)
2. [Google AI Forum](https://discuss.ai.google.dev/)
3. [Stack Overflow](https://stackoverflow.com/questions/tagged/google-gemini)

---

Happy coding with Gemini CLI! 🚀