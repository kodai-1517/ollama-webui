
## アーキテクチャ図

```mermaid
    graph LR
    subgraph global
        user[User]
    end

    subgraph local
        subgraph front
            webui[Open WebUI]
        end

        subgraph back
            tika[Apache Tika]
            ollama[Ollama]
            llm[LLM Model]
        end
    end

    ngrok[ngrok]

    user --   "01.送信" --> ngrok
    ngrok --  "02.経由" --> webui
    webui --  "03.ナレッジ検索" --> tika
    tika --   "04.ナレッジ提供" --> webui
    webui --  "05.リクエスト" --> ollama
    ollama -- "06.推論" --> llm
    llm --    "07.出力" --> ollama
    ollama -- "08.応答" --> webui
    webui --  "09.経由" --> ngrok
    ngrok --  "10.返答" --> user
```

## シーケンス図

```mermaid
sequenceDiagram
    participant User
    participant ngrok
    participant Open WebUI
    participant Apache Tika
    participant Ollama
    participant LLM Model

    User->>ngrok: 01. 送信
    ngrok->>Open WebUI: 02. 経由
    Open WebUI->>Apache Tika: 03. ナレッジ検索
    Apache Tika-->>Open WebUI: 04. ナレッジ提供
    Open WebUI->>Ollama: 05. リクエスト
    Ollama->>LLM Model: 06. 推論
    LLM Model-->>Ollama: 07. 出力
    Ollama-->>Open WebUI: 08. 応答
    Open WebUI-->>ngrok: 09. 経由
    ngrok-->>User: 10. 返答
```

## セットアップ

1. [Docker](https://www.docker.com/ja-jp/products/docker-desktop/)をインストール

1. [VSCode](https://code.visualstudio.com/)をインストール

1. VSCodeにDockerの拡張機能をインストール

    ![Image](images/vscode_docker.png)

1. リポジトリをクローン

    ![Image](images/vscode_clone.png)
    ![Image](images/vscode_repository.png)

1. docker-compose.yamlを右クリックし、Compose Upを選択

    ![Image](images/compose_up.png)

1. shellを開く

    ![Image](images/attach_shell.png)

1. モデルのダウンロード（[モデルの一覧](https://ollama.com/library)）
    ```
    ollama pull <model_name>
    ```

1. https://localhost:3000 にアクセス

## インターネットに公開

1. [ngrok](https://ngrok.com/)のアカウントを作成

1. WinGetを使用してngrokをインストール
    ```
    winget install ngrok -s msstore
    ```

1. ngrokの認証
    ```
    ngrok config add-authtoken <your_auth_token>
    ```

1. ローカルサーバーを公開
    ```
    ngrok http localhost:3000
    ```

## 参考サイト

- ローカルLLMの使用 - OllamaとOpen WebUIの連携について解説  
https://qiita.com/RyutoYoda/items/ecdfbef8c73aae64aa45

- LLM推論マシンの環境構築メモ(ollama open-webui ubuntu2204)
https://qiita.com/katakaku/items/eb807594ea9f1ec425d3

- ローカルLLMを外部PC/スマホで！【ngrok＋Open WebUI+Ollama】
https://note.com/josh_/n/n74b7cc0fdd1a
