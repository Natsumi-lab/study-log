---
layout: post
title: "暗号化について"
date: 2026-08-11
categories: [Infrastructure]  
permalink: /encryption/
---

# セキュアなアーキテクチャの設計（暗号化）の要点まとめ

AWSにおける「セキュアなアーキテクチャの設計（Design Secure Architectures）」において、  
データの暗号化（Encryption）は機密性・完全性を維持し、コンプライアンス要件を満たすための最重要テーマです。

暗号化の基本原則は **「保管時の暗号化（Encryption at Rest）」** と **「伝送時の暗号化（Encryption in Transit）」** の2つに大別されます。

---

## 1. 保管時の暗号化（Encryption at Rest）

ディスクやストレージ（S3, EBS, RDSなど）に保存されているデータを保護するための暗号化技術です。

### 鍵管理サービス：AWS KMS (Key Management Service)
AWS環境における暗号化鍵の生成・管理を行うマネージドサービスです。

| KMSキーの種類 | 管理主体 | 概要・特徴 |
| :--- | :--- | :--- |
| **AWS 所有のキー (AWS Owned Keys)** | AWS | ユーザーが直接操作・管理しない共有キー（基本無料）。 |
| **AWS マネージドキー (AWS Managed Keys)** | ユーザー / AWS | AWSサービス側が自動生成。表示は可能だが、鍵の回転（ローテーション）や直接の権限管理は制限される。 |
| **カスタマーマネージドキー (Customer Managed Keys: CMK)** | ユーザー | ユーザー自身が作成・管理・キーの有効化/無効化・自動回転を設定できる。SAA試験で推奨されるパターン。 |

### エンベロープ暗号化 (Envelope Encryption)
* **概要**: データを暗号化する「データキー (Data Key)」を、さらに親鍵である「マスターキー (KMS KMS Key)」で暗号化する構造。
* **メリット**: 大容量データを直接 KMS に送る必要がなくなり、ネットワークのパフォーマンス向上とAPI制限（クォータ）の回避が可能。

---

## 2. 主要サービスにおける保管時の暗号化

### Amazon S3 の暗号化オプション
S3では、サーバー側暗号化（SSE: Server-Side Encryption）およびクライアント側暗号化が利用できます。

```
[ Amazon S3 暗号化オプション ]
├─ SSE-S3 (S3マネージドキー) ───────> 完全自動、追加コストなし
├─ SSE-KMS (KMSマネージドキー) ──────> CMK利用可能、監査ログ(CloudTrail)が記録される
├─ SSE-C (顧客提供キー) ─────────> 鍵の管理はオンプレ側（暗号化処理のみS3）
└─ Client-Side (クライアント側暗号化) ─> アップロード前にローカルで暗号化
```

* **SSE-S3**: S3が自動的にキーを管理。追加費用なし。
* **SSE-KMS**: AWS KMSキーを使用。どの鍵で誰が復号したかの監査ログを CloudTrail に記録できるため、SAA試験でのセキュリティ要件でよく選ばれる。
* **バケットポリシーでの強制**: `s3:x-amz-server-side-encryption` ヘッダーを検証し、未暗号化オブジェクトのアップロード（`PutObject`）を拒否・強制暗号化できる。

### EBS, RDS, DynamoDB の暗号化
* **Amazon EBS**: 
  * 暗号化を有効にすると、データ・スナップショット・インスタンス間を行き来するI/Oがすべて自動的に暗号化される。
  * 暗号化されていない既存ボリュームを直接暗号化することは不可（暗号化スナップショットを作成し、そこから復元する）。
* **Amazon RDS**: 
  * インスタンス作成時に暗号化を有効化。バックアップやリードレプリカも連動して暗号化される。
* **AWS Secrets Manager vs Parameter Store (SSM)**:
  * **Secrets Manager**: DBの接続文字列やAPIキーを安全に保管。**自動ローテーション機能**（Lambda連携）をサポート。
  * **Parameter Store (SecureString)**: 設定値や暗号化されたパスワードを保管。シンプルで低コスト。

---

## 3. 伝送時の暗号化（Encryption in Transit）

ネットワーク上を通行するパケットの盗聴・改ざんを防ぐための暗号化（TLS/SSL）です。

### SSL/TLS 証明書の管理：AWS Certificate Manager (ACM)
* **役割**: SSL/TLS 証明書の作成・配布・**自動更新**を行うマネージドサービス。
* **適用対象**: ALB (Application Load Balancer), CloudFront, API Gateway 等と統合して使用。

### 通信経路の暗号化構成パターン
1. **エンドツーエンド（E2E）暗号化**:
   * クライアント ↔ ALB ↔ EC2（バックエンド）の全経路を HTTPS/TLS で暗号化する。
2. **SSL ターミネーション (SSL Offloading)**:
   * クライアント ↔ ALB 間のみ HTTPS を利用し、ALB ↔ EC2 内部ネットワーク間は HTTP（平文）通信にすることで、EC2 の CPU 負荷を軽減する。

---

## 4. セキュリティとコンプライアンス管理

* **AWS CloudHSM**:
  * コンプライアンス上の理由で、共有基盤（KMS）ではなく**専用の単一占有型ハードウェア（HSM）**で暗号化キーを厳格に管理・操作したい場合に使用（FIPS 140-2 Level 3 準拠）。
* **AWS KMS のアクセス制御**:
  * KMSキーの使用権限は、**「キーポリシー (Key Policy)」** および **「IAMポリシー」** の組み合わせで制御する（キーポリシーの設定が必須）。
* **AWS CloudTrail による追跡**:
  * KMS キーの使用（`kms:Decrypt`, `kms:GenerateDataKey` など）はすべて CloudTrail にイベントログとして記録され、セキュリティ監査が可能。