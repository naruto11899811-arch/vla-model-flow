```mermaid
graph TD
    subgraph "フェーズ1: データ収集"
        direction LR;
        HW1[ハードウェア: ロボットアーム・各種センサー] --> P1{データ収集・監視};
        P1 --> SW1[ソフトウェア: 自動収集プログラム];
        SW1 --> CLOUD1[クラウド: Datalake / ストレージ];
    end

    subgraph "フェーズ2: データ処理・訓練"
        CLOUD1 --> ONSERVER[オンプレミス: PC / サーバー];
        ONSERVER --> P2{データ前処理 (ROSbagから動画抽出)};
        P2 --> P3[GenAI Studio: プロンプト調整・オートラベリング];
        P3 --> P4(発話とストーリー生成);
        P4 --> P5["クラウドでのモデル訓練 GPU:A6000+, CPU:128core+"];
        P5 --> CQ{エラーチェック};
        CQ -- エラーあり --> P3;
        CQ -- 正常 --> MODEL[<b>訓練済みVLAモデル</b>];
    end

    subgraph "フェーズ3: モデル実装"
        direction LR;
        MODEL --> HW2[ハードウェア: ロボット搭載PC (NUC+GPU)];
        HW2 --> SW2[ソフトウェア: カスタム制御ソフトへ統合];
        SW2 --> ROBOT[<b>実機ロボット: 自律的なタスク実行</b>];
    end
