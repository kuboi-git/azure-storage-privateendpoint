# Azure Storage Private Endpoint

# 1. 概要
Azure Storage Accountに対して、Private Endpointを構成し、  
Private DNSを利用した名前解決と閉域通信を確認しました。

Public Accessを制限し、Azure VMからPrivate IP経由でStorage Accountへ接続できることを確認しました。

---

# 2. 構成図
```text
Resource Group: rg-storage-privateendpoint
│
├─ Virtual Network: vnet-test-01
│  ├─ Subnet: subnet-app
│  │  └─ Ubuntu VM: vm-linux-01
│  │
│  └─ Subnet: subnet-private-endpoint
│     └─ Private Endpoint: privateendpoint-test-01
│        └─ 接続先: storagekuboi01 の Blob
│
├─ Storage Account: storagekuboi01
│  └─ Blob Container
│
└─ Private DNS Zone: privatelink.blob.core.windows.net
   ├─ VNetリンク: vnet-test-01
   └─ Aレコード: storagekuboi01 → Private EndpointのプライベートIP
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
## 5-1 Private DNS名前解決

```bash
nslookup storagekuboi01.blob.core.windows.net
```

### 実行結果
<img width="535" height="123" alt="nslookup" src="https://github.com/user-attachments/assets/7ff0cd7f-8fd3-4797-8af3-cd7159bee012" />

---

### 5-2. 追加検証：VMからBlobを読み取る

VMのマネージドIDに `demo` コンテナーの読み取り権限を付けました。
VMからPrivate Endpoint経由でアクセスし、テストファイルの内容と `HTTP 200` が返ることを確認しました。

---

# 6. 苦戦したこと

## Blobのテストファイルを取得できなかった

### 原因
VMからテストファイルを取得しようとすると、`404 BlobNotFound` が返りました。
Azureポータルのファイル名とリクエストURLを照合したところ、
URLのファイル名に余分なスペースが入っていました。

### 解決方法
ファイル名を `hello Privateendpoint.txt` に修正して再実行しました。
接続先がPrivate Endpointの `10.0.20.4` であることと、
HTTP `200` でファイルの中身が返ることを確認しました。

---

# 7. 学んだこと

- Azureのネットワーク基礎
- NSGによる通信制御
- Public通信とPrivate通信の違い
- Private Endpointによる閉域化
- Private DNSによる名前解決 
