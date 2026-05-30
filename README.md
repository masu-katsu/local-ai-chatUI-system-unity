# Local AI Chat UI System

Unityで構築されたローカルAIチャットUIシステムです。FastAPIバックエンドと連携して、ローカル環境でAIと対話することができます。

## 概要

このプロジェクトはUnityベースのフロントエンドUIシステムで、ローカルAIモデル（Phi-3、Qwenなど）と対話するためのチャットインターフェースを提供します。バックエンドシステムは以下のリポジトリで管理されています：

- **バックエンドシステム**: [https://github.com/masu-katsu/local-ai-unity-system](https://github.com/masu-katsu/local-ai-unity-system)

バックエンドのセットアップ、AIモデルの設定、RAG（検索拡張生成）機能などの詳細については、上記バックエンドリポジトリを参照してください。

## 機能

- **リアルタイムチャット**: ローカルAIモデルとの対話
- **複数モデル対応**: Phi-3、QwenなどのAIモデル
- **日本語対応**: 日本語フォントと入力の完全サポート
- **接続監視**: 自動ヘルスチェックと接続状態表示
- **設定管理**: サーバーURL、APIキー、ユーザーIDの設定
- **レスポンス情報**: 使用モデル、処理時間、コンテキスト使用状況の表示

## システムアーキテクチャ

### 主要コンポーネント

#### API層 (`Assets/Scripts/API/`)
- **ApiClient.cs**: FastAPIサーバーとのHTTP通信を管理
  - チャットメッセージ送信 (`/api/chat`)
  - ヘルスチェック (`/api/health`)
  - 設定の永続化（PlayerPrefs）
- **ApiModels.cs**: APIリクエスト/レスポンスのデータモデル

#### チャットロジック (`Assets/Scripts/Chat/`)
- **ChatManager.cs**: チャットのロジックを管理
  - メッセージ送信・受信
  - イベントベースのUI通知
  - ユーザーID管理

#### UI管理 (`Assets/Scripts/UI/`)
- **ChatUIManager.cs**: チャット画面のUI管理
  - メッセージバブルの動的生成
  - スクロール管理
  - タイピングインジケーター
  - 接続状態表示
- **SettingsUIManager.cs**: 設定画面のUI管理
  - サーバーURL、APIキー、ユーザーID設定
  - 接続テスト機能
- **MessageBubble.cs**: メッセージバブルコンポーネント
- **MobileInputHelper.cs**: モバイル入力支援

#### ネットワーク (`Assets/Scripts/Network/`)
- **NetworkMonitor.cs**: ネットワーク接続の定期監視
  - 自動ヘルスチェック（30秒間隔）
  - 接続断時の再試行（5秒間隔）
  - 接続状態変化イベント

#### エディタツール (`Assets/Scripts/Editor/`)
- **AutoWire.cs**: UI参照の自動配線
- **SceneBuilder.cs**: シーン構築ツール

## セットアップ

### 前提条件

1. **Unity**: Unity 2022.3以降
2. **バックエンドサーバー**: [local-ai-unity-system](https://github.com/masu-katsu/local-ai-unity-system) のセットアップと起動
   - デフォルトURL: `http://localhost:8000`

### インストール手順

1. このリポジトリをクローン
2. Unityでプロジェクトを開く
3. バックエンドサーバーを起動（[バックエンドリポジトリ](https://github.com/masu-katsu/local-ai-unity-system)の手順に従う）
4. Unityで `SampleScene` を開く
5. 実行

### 設定

チャット画面の設定ボタン（歯車アイコン）から以下を設定：

- **Server URL**: バックエンドFastAPIサーバーのURL（デフォルト: `http://localhost:8000`）
- **API Key**: バックエンドで設定されたAPIキー（必要な場合）
- **User ID**: ユーザー識別子（デフォルト: `default_user`）

設定はPlayerPrefsに保存され、次回起動時に自動的に読み込まれます。

## プロジェクト構成

```
Assets/
├── Fonts/           # 日本語フォント
├── Prefabs/         # UIプレハブ
│   ├── AIMessageBubble.prefab
│   └── UserMessageBubble.prefab
├── Scenes/          # Unityシーン
│   └── SampleScene.unity
├── Scripts/         # C#スクリプト
│   ├── API/         # API通信
│   ├── Chat/        # チャットロジック
│   ├── Editor/      # エディタツール
│   ├── Network/     # ネットワーク監視
│   └── UI/          # UI管理
└── TextMesh Pro/    # TextMeshProパッケージ
```

## APIエンドポイント

このUIシステムは以下のバックエンドAPIエンドポイントを使用します：

### POST `/api/chat`
チャットメッセージを送信し、AIの応答を取得

**リクエスト**:
```json
{
  "message": "ユーザーメッセージ",
  "user_id": "ユーザーID",
  "web_search_confirmed": false,
  "web_search_action": null
}
```

**レスポンス**:
```json
{
  "response": "AI応答",
  "model_used": "phi3",
  "processing_time": 1.23,
  "context_used": true,
  "web_search_used": false,
  "requires_confirmation": false,
  "pending_web_search": "",
  "search_in_progress": false
}
```

### GET `/api/health`
サーバーとAIモデルのヘルスチェック

**レスポンス**:
```json
{
  "status": "healthy",
  "phi3": "loaded",
  "qwen": "loaded",
  "chromadb": "connected"
}
```

## 使用技術

- **Unity**: ゲームエンジン
- **TextMeshPro**: テキストレンダリング
- **Unity uGUI**: UIシステム
- **UnityWebRequest**: HTTP通信

## バックエンドについて

バックエンドシステム（FastAPI、AIモデル、RAG機能など）の詳細については、以下のリポジトリを参照してください：

[https://github.com/masu-katsu/local-ai-unity-system](https://github.com/masu-katsu/local-ai-unity-system)

## ライセンス

このプロジェクトのライセンスについては、バックエンドリポジトリのライセンスに従ってください。

## 貢献

バグ報告や機能リクエストはIssueにてお願いします。
