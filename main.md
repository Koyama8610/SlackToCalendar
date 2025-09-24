graph TD
    subgraph User[ユーザー環境]
        PC[💻 PC / Webブラウザ<br>・ダッシュボード表示<br>・フィードバック入力]
    end

    subgraph Cluster[Emolink Kubernetes(K3s) Cluster on Local Network]
        direction LR
        subgraph rpi3[rpi3 (ワーカーノード)<br>表情解析担当]
            direction TB
            Cam[📷 USBカメラ] --> App3(表情解析<br>OpenCV / TensorFlow Lite)
        end

        subgraph rpi1[rpi1 (マスターノード)<br>統合・表示・DB担当]
            direction TB
            App1(統合分析 & API<br>Flask)
            DB[(🗃️ SQLite<br>感情データベース)]
            App1 <--> DB
        end
        
        subgraph rpi2[rpi2 (ワーカーノード)<br>音声解析担当]
            direction TB
            Mic[🎤 USBマイク] --> App2(音声解析<br>PyAudio / YAMNet)
        end
    end

    App3 -- 表情データ --> App1
    App2 -- 音声データ --> App1

    App1 -- 統合感情データ<br>(リアルタイム表示) --> PC
    PC -- ユーザーフィードバック<br>(教師データ) --> App1

    style User fill:#e3f2fd,stroke:#333,stroke-width:2px
    style Cluster fill:#fbe9e7,stroke:#333,stroke-width:2px,stroke-dasharray: 5 5
    style rpi1 fill:#fff,stroke:#d32f2f,stroke-width:2px
    style rpi2 fill:#fff,stroke:#303f9f,stroke-width:2px
    style rpi3 fill:#fff,stroke:#303f9f,stroke-width:2px
