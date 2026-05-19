# Azure Storage Private Endpoint

## 1. 概要
Azure Storage Accountに対して、Private Endpointを構成し、  
Private DNSを利用した名前解決と閉域通信を確認しました。

Public Accessを制限し、Azure VMからPrivate IP経由でStorage Accountへ接続できることを確認しました。

---

# 2. 構成図

```text
vm-test-01 (Ubuntu)
          ↓
rg-storage-privateendpoint
          ↓
vnet-test-01
├─ subnet-app
│
└─ subnet-private-endpoint
          ↓
privateendpoint-test-01
          ↓
storagekuboi01
          ↓
Private DNS Zone
```

---

# 3. 使用サービス

| サービス | 用途 |
|---|---|
| Resource Group | リソース管理 |
| Virtual Network | 仮想ネットワーク |
| Subnet | ネットワーク分離 |
| NSG | 通信制御 |
| Ubuntu VM | 接続確認用 |
| Storage Account | Blob Storage |
| Blob Container | データ保存 |
| Private Endpoint | 閉域接続 |
| Private DNS Zone | DNS名前解決 |

---

# 4. 作成順序

1. Resource Group 作成
2. Virtual Network 作成
3. subnet-app 作成
4. subnet-private-endpoint 作成
5. NSG 作成
6. Ubuntu VM 作成
7. Storage Account 作成
8. Blob Container 作成
9. Private Endpoint 作成
10. Private DNS Zone 自動作成確認
11. Public Network Access 制限
12. SSH接続
13. nslookup による名前解決確認

---

# 5. 動作確認

## Private DNS 名前解決

```bash
nslookup storagekuboi01.blob.core.windows.net
```

### 実行結果
<img width="535" height="123" alt="nslookup" src="https://github.com/user-attachments/assets/7ff0cd7f-8fd3-4797-8af3-cd7159bee012" />


---

# 6. 苦戦したこと

## Storage Account が見つからなかった

### 原因
Storage Account 名がグローバルで重複していた。

### 解決方法
一意になる名前へ変更し解決。

---

## SSH接続できなかった

### 原因
.pem ファイル配置ミス。

### 解決方法
PowerShell の実行ディレクトリを修正し解決。

---

# 7. 学んだこと

- Azure のネットワーク基礎
- NSG による通信制御
- Public通信とPrivate通信の違い
- Private Endpoint による閉域化
- Private DNS による名前解決 
