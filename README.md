# azure-storage-privateendpoint

## 概要
- Azure Storage Accountに対してPrivate Endpointを構成し、Private IP経由で接続確認を実施
  
## 使用サービス
- Azure Storage Account
- Blob Container
- Private Endpoint
- Private DNS Zone
- Virtual Network
- Ubuntu VM

## 作成順番
1. Resource Group作成      「rg-storage-privateendpoint」
2. Storage Account作成     「storagekuboi01」
3. Blob Container作成      「container-test-01」
4. Private Endpoint作成    「privateendpoint-test-01」
5. Private DNS Zone統合
6. Linux VMからnslookup実行

## 動作確認
- nslookup結果について、Storage AccountがPrivate IP (10.0.10.5) に名前解決されることを確認
- nslookup
↓
UbuntuのDNS(127.0.0.53)
↓
Azure Private DNS Zone
↓
storagekuboi01.privatelink.blob.core.windows.net
↓
10.0.10.5
<img width="535" height="123" alt="nslookup" src="https://github.com/user-attachments/assets/6c180d03-e57b-4484-bb13-e3be351bfc8d" />

## 学んだこと
- DNS名前解決の流れ
- Azure Private DNS Zoneの役割
- Private IPを利用したAzure内部通信
- 

## 苦戦した点
- Storage Account名を誤って入力し、nslookupがNXDOMAINになった。
- Storage Account名を確認して解決。（Resource Groupから再作成しなおしました）
