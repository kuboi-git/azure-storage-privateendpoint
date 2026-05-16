# azure-storage-privateendpoint

## 概要
- Azure Storage Accountに対してPrivate Endpointを構成し、Private IP経由で接続確認を実施
- 
## 使用サービス
- Azure Storage Account
- Blob Container
- Private Endpoint
- Private DNS Zone
- Virtual Network
- Ubuntu VM

## 作成順番
1. Resource Group作成
2. Storage Account作成
3. Blob Container作成
4. Private Endpoint作成
5. Private DNS Zone統合
6. Linux VMからnslookup実行

## 動作確認
- nslookup結果について、Storage AccountがPrivate IP (10.0.10.5) に名前解決されることを確認
<img width="535" height="123" alt="nslookup" src="https://github.com/user-attachments/assets/6c180d03-e57b-4484-bb13-e3be351bfc8d" />

## 学んだこと
- Private Endpointの構築方法
- Azure Private DNS の基本
- nslookupによる名前解決確認
- Azure内部通信の仕組み
- 
## 苦戦した点
Storage Account名を誤って入力し、nslookupがNXDOMAINになった。
Storage Account名を確認して解決。（Resource Groupから再作成しなおしました）
