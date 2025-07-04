# JavaSilver 学習アプリ

## 概要

このアプリケーションは、Java Silver認定試験の合格を目指す学習者をサポートするために開発されたAndroid向け学習ツールです。クイズ形式での実践的な問題演習、分野別の体系的な学習、詳細な学習履歴および統計機能を提供し、効率的な学習体験を実現します。

## 特徴

* **ランダム出題**: 全範囲からランダムに問題が出題され、総合的な理解度を確認できます。
* **分野別学習**: Java Silver試験の出題範囲に基づき、章およびカテゴリー別に問題を絞り込んで集中的に学習できます。
* **問題一覧**: アプリに収録されている全ての問題を一覧で参照し、解答状況を確認できます。
* **学習履歴**: 過去に解答した問題の記録（問題ID、正誤、解答日時）を時系列で確認できます。
* **学習統計**: 問題ごとの正誤回数や正答率を分析し、自身の弱点を把握できます。
* **ブックマーク機能**: 後で復習したい問題や重要だと感じた問題をマークして、簡単にアクセスできます。
* **テーマ設定**: 複数のカラーテーマから選択し、ユーザーの好みに合わせた学習環境を構築できます。
* **ユーザー認証**: Firebase Authenticationを利用したログイン・新規登録機能により、学習データをユーザーアカウントと紐づけて管理し、異なるデバイス間での同期やデータの永続化に対応します。
* **コンテンツ管理**: ヘッドレスCMSであるContentfulからクイズデータを取得し、アプリのコンテンツを柔軟に更新・管理します。

## 技術スタック

本プロジェクトは、以下の技術要素を組み合わせて開発されています。各技術の選定理由とアプリケーション内での役割を説明します。

* **言語**:
    * **Kotlin**: Android開発における公式の推奨言語であり、Javaとの相互運用性に優れています。Null安全、コルーチン、拡張関数などのモダンな言語機能により、コードの記述量を削減し、可読性と安全性を向上させるために主要言語として採用しました。
    * **Java**: 既存コードベースの一部や、特定のライブラリとの連携のために併用しています。将来的にはKotlinへの完全移行を検討しています。
* **UI フレームワーク**:
    * **Jetpack Compose**: Androidの宣言的UIフレームワークです。UI開発の生産性向上、コードの簡潔化、リアクティブなUI構築を目指し、ログイン・登録画面などの一部画面に導入しました。
    * **Android Views (XML)**: アプリケーションの大部分の画面（ホーム、クイズ、リストなど）は従来のXMLレイアウトで構築されています。安定性と既存リソースの活用を考慮し併用しています。
* **アーキテクチャ**:
    * **MVVM (Model-View-ViewModel)**: UI層（Activity/Fragment/Compose）、ビジネスロジック層（ViewModel）、データ層（Repository）を明確に分離する設計パターンです。これにより、各コンポーネントの責務を明確にし、コードのテスト容易性、保守性、拡張性を高めています。
* **データ永続化**:
    * **Room Database**: Android Jetpackの一部であり、SQLiteデータベースを抽象化するライブラリです。Contentfulから取得したクイズデータやユーザーの学習履歴、ブックマークなどのローカルデータストアとして使用し、オフラインでの利用や高速なデータアクセスを実現します。
* **リモートデータ取得**:
    * **Contentful CDA SDK**: ContentfulのContent Delivery APIを利用するための公式SDKです。アプリのクイズコンテンツをContentfulから効率的にフェッチし、アプリの更新なしにコンテンツの追加や変更を可能にします。
* **認証**:
    * **Firebase Authentication**: Googleが提供する認証サービスです。メールアドレス/パスワード認証およびGoogleアカウント連携による認証機能を容易かつ安全に実装するために利用しています。
* **依存性注入 (DI)**:
    * **Hilt**: DaggerをベースとしたAndroid開発に特化したDIライブラリです。依存オブジェクトの生成と管理を自動化し、コンポーネント間の依存関係を疎結合に保ち、テストコードの記述を容易にします。ViewModelやRepositoryなどの主要コンポーネントの依存解決に活用しています。
* **非同期処理**:
    * **Kotlin Coroutines**: Kotlinの非同期処理フレームワークです。ネットワーク通信、データベースアクセス、時間のかかる処理などをノンブロッキングに実行し、UIスレッドをブロックしない快適なユーザー体験を提供するために活用しています。`kotlinx-coroutines-play-services` を使用し、Firebase SDKとの連携もスムーズに行っています。
* **ナビゲーション**:
    * **Android Navigation Component**: アプリ内の画面遷移を管理するためのAndroid Jetpackライブラリです。複雑な画面遷移をシンプルに定義・管理し、安全な引数渡し（Safe Args）などを利用しています。
* **その他ライブラリ/ツール**:
    * **Lottie**: Adobe After Effectsで作成されたアニメーションをモバイルアプリで簡単に表示できるライブラリです。ローディング画面やホーム画面でのキャラクターアニメーションに使用しています。
    * **EncryptedSharedPreferences**: Android Security-Cryptoライブラリの一部です。APIキーなどの機密情報をローカルに安全に暗号化して保存するために使用しています。
    * **ktlint, detekt**: Kotlinコードの静的解析ツールです。コードスタイルの一貫性を保ち、潜在的な問題を早期に発見するために導入しています。

## アーキテクチャと技術間の関連性 (Mermaid図)

本アプリの主要なアーキテクチャコンポーネントと技術要素間の関連性、およびデータの流れを以下のMermaid図で示します。

```mermaid
graph TD
    A[UI: Fragment / Compose] --> B(ViewModel);
    B --> C(Repository);
    C --> D[Room Database];
    C --> E[Contentful CDA SDK];
    B --> F[Firebase Authentication];
    F --> G[Firebase Backend];
    C --> G;
    E -- クイズデータ --> C;
    D -- ローカルデータ --> C;
    G -- ユーザーデータ<br/>学習履歴<br/>ブックマーク --> C;
    A -- ユーザー操作 --> B;
    B -- 状態更新 --> A;
    F -- 認証リクエスト --> G;
    G -- 認証結果 --> F;
    C -- データ保存 --> G;
    
    %% Styling
    style A fill:#bbf,stroke:#333,stroke-width:2px
    style B fill:#f9f,stroke:#333,stroke-width:2px
    style C fill:#ccf,stroke:#333,stroke-width:2px
    style D fill:#cfc,stroke:#333,stroke-width:2px
    style E fill:#ffc,stroke:#333,stroke-width:2px
    style F fill:#fcf,stroke:#333,stroke-width:2px
    style G fill:#cff,stroke:#333,stroke-width:2px

    %% Subgraphs (Optional, for grouping)
    subgraph Presentation Layer
        A
        B
    end

    subgraph Domain Layer
        C
    end

    subgraph Data Layer
        D
        E
        F
        G
    end

    %% Link styles (Optional, for clarity)
    linkStyle 0,1,2,3,4,5,6,7,8,9,10,11,12 stroke:#555
```

図の説明：

* **UI (Fragment / Compose)**: ユーザーが見て操作する画面です。ユーザーからの入力を受け取り、ViewModelに伝えます。また、ViewModelから受け取ったデータを表示します。
* **ViewModel**: UIの表示に必要なデータを保持し、UIロジックを処理します。Repositoryを通じてデータにアクセスし、その結果をUIに提供します。UIの状態を管理し、画面回転などの構成変更にも耐えうるように設計されています。
* **Repository**: アプリケーションのデータアクセス層へのクリーンなAPIを提供します。複数のデータソース（Room、Contentful、Firebase）からのデータの取得、保存、更新のロジックをカプセル化し、ViewModelはデータソースの詳細を知る必要がありません。
* **Room Database**: アプリケーションのローカルデータベースです。Contentfulからフェッチしたクイズデータや、ユーザーの学習履歴、ブックマークなどのデータを構造化して永続化します。
* **Contentful CDA SDK**: Contentfulからクイズコンテンツを取得するためのライブラリです。Repositoryから呼び出され、リモートのクイズデータを供給します。
* **Firebase Authentication**: ユーザーの認証状態を管理します。ログイン、新規登録、ログアウトなどの機能を提供し、RepositoryやViewModelから利用されます。
* **Firebase Backend**: FirestoreなどのFirebaseのクラウドサービスを指します。ユーザーごとの学習履歴やブックマークなどのデータをサーバーに保存し、異なるデバイス間でのデータ同期やバックアップに利用します。

このアーキテクチャにより、UIはデータの表示とユーザー操作の伝達のみに集中でき、ビジネスロジックはViewModelに、データアクセスはRepositoryに集約され、各層が独立してテスト・保守しやすい構造になっています。

## プロジェクト構造の概要

```
.
├── app
│   ├── build.gradle          # アプリモジュールのGradle設定
│   └── src
│       ├── androidTest       # Androidインストルメンテーションテスト
│       ├── main
│       │   ├── AndroidManifest.xml # アプリのマニフェストファイル
│       │   ├── java          # Java/Kotlinソースコード
│       │   │   └── com
│       │   │       └── example
│       │   │           └── contentful_javasilver # アプリケーションコード
│       │   │               ├── adapter         # RecyclerViewアダプター
│       │   │               ├── data            # データ関連クラス (Room Entity, DAO, Repository)
│       │   │               ├── decoration      # RecyclerViewデコレーション
│       │   │               ├── di              # Hiltモジュール
│       │   │               ├── model           # UI用データモデル
│       │   │               ├── models          # (Kotlin) データモデル
│       │   │               ├── ui              # Jetpack Compose UI
│       │   │               ├── utils           # ユーティリティクラス
│       │   │               └── viewmodels      # ViewModelクラス
│       │   └── res           # リソースファイル (layout, drawable, values, menu, navigationなど)
│       └── test              # ローカルユニットテスト
├── build.gradle              # プロジェクト全体のGradle設定
├── gradle.properties         # Gradleプロパティ
├── gradlew / gradlew.bat     # Gradleラッパースクリプト
├── settings.gradle / settings.gradle.kts # Gradle設定ファイル
└── docs                      # プロジェクトに関するドキュメント
    └── code-review.md        # コードレビューに関するメモ
```

## 始め方

### 前提条件

* Android Studio Hedgehog | 2023.1.1 以降
* JDK 17 以降
* Contentful Space ID および Access Token
* Firebase プロジェクト設定 (`google-services.json`) および Web Client ID

### セットアップ

1.  このリポジトリをクローンします。
    ```bash
    git clone [リポジトリURL]
    cd [リポジトリ名]
    ```
2.  Android Studioでプロジェクトを開きます。
3.  プロジェクトルートディレクトリに`local.properties` ファイルを作成し、Contentfulの認証情報を追加します。
    ```properties
    CONTENTFUL_SPACE_ID="あなたのContentful Space ID"
    CONTENTFUL_ACCESS_TOKEN="あなたのContentful Access Token"
    ```
4.  Firebaseプロジェクトからダウンロードした`google-services.json`ファイルを`app/`ディレクトリに配置します。
5.  `app/src/main/res/values/strings.xml` ファイルに、FirebaseプロジェクトのWeb Client IDを追加します。
    ```xml
    <string name="default_web_client_id" translatable="false">あなたのWeb Client ID</string>
    ```
6.  プロジェクトをビルドし、エミュレーターまたは実機で実行します。

## コードレビューに関するメモ

`docs/code-review.md` に、オープンソース化に向けたコードの評価と改善提案がまとめられています。可読性、保守性、テストカバレッジ、設計パターンに関する課題や、静的解析ツールの活用などが提案されており、今後のプロジェクト改善のロードマップとして活用しています。

## 貢献

貢献を歓迎します！コントリビューションの方法については、別途 `CONTRIBUTING.md` ファイルを作成する予定です。

## ライセンス

[ここにライセンス情報を記載]

---
