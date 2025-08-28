graph TD
    subgraph "フェーズ1: データ収集"
        direction LR
        HW1[<br>ハードウェア<br>ロボットアーム<br>各種センサー<br>] --> P1{データ収集・監視};
        P1 --> SW1[<br>ソフトウェア<br>自動収集プログラム<br>];
        SW1 --> CLOUD1[<br>クラウド<br>Datalake / ストレージ<br>];
    end

    subgraph "フェーズ2: データ処理・訓練"
        CLOUD1 --> ONSERVER[<br>オンプレミス<br>PC / サーバー<br>];
        ONSERVER --> P2{データ前処理<br>(ROSbagから動画抽出)};
        P2 --> P3[<br>GenAI Studio<br>プロンプト調整 & オートラベリング<br>];
        P3 --> P4(発話とストーリー生成);
        P4 --> P5["<br>クラウドでのモデル訓練<br>GPU: A6000+ (40h+)<br>CPU: 128core+, RAM 500GB+<br>"];
        P5 --> CQ{エラーチェック};
        CQ -- エラーあり --> P3;
        CQ -- 正常 --> MODEL[<br><b>訓練済みVLAモデル</b><br>];
    end

    subgraph "フェーズ3: モデル実装"
        direction LR
        MODEL --> HW2[<br>ハードウェア<br>ロボット搭載PC (NUC+GPU)<br>];
        HW2 --> SW2[<br>ソフトウェア<br>カスタム制御ソフトへ統合<br>];
        SW2 --> ROBOT[<br><b>実機ロボット</b><br>自律的なタスク実行<br>];
    end# vla-model-flow
