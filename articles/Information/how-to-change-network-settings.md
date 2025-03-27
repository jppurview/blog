---
title: Purviewアカウントのネットワーク設定の変更
date: 2025-03-24 12:00:00
tags:
  - Information
disableDisclaimer: false
---

この記事では Purviewアカウントのネットワーク設定の変更の際に表示されるエラーについてご説明します。

<!-- more -->

## 概要

Purviewアカウントのネットワーク設定の変更の際に、以下のエラーが表示される事象が発生します。
![](./how-to-change-network-settings/error.png)

## 現象
概要でご説明した事象が発生する手順は次の通りです。

1. AzureポータルからAzureサービス「Microsoft Purview アカウント」を選択します。
2. ネットワーク設定をしたい Purviewアカウントを選択します。
3. 左タブの 設定 > ネットワークを開きます。
4. ネットワーク設定を変更し、保存ボタンを押します。
5. 概要に掲載しているエラーが右上の通知部分に表示されます。

## 現象の発生条件
Microsoft Purview アカウントをホストする Azure サブスクリプションに「IAM 閲覧者ロール」以上の権限が付与されていない場合、
概要に掲載のエラーが発生します。

## 原因
内部で設定変更ロジックとは別にAzure Resource Managerで設定の状況を追跡しているため、
Microsoft Purview アカウントをホストする Azure サブスクリプションに「IAM 閲覧者ロール」以上の権限が必要となります。

## 影響
Microsoft Purview アカウントをホストする Azure サブスクリプションに「IAM 閲覧者ロール」以上の権限がない場合、
概要欄のエラーキャプチャの通り、
「ネットワーク設定の保存失敗
エラーが発生したため、Microsoft Purview アカウント[アカウント名]のネットワーク設定を保存できませんでした。」というエラーが出ておりますが、
ネットワーク設定の変更自体には影響がございません。（ネットワーク設定の保存は完了いたします。）

次の文で、「HTTP403:・・・[ユーザ]には、スコープ '/subscriptions/[サブスクリプションID]/providers/Microsoft.Purview/locations/[ロケーション]/operationResults/[オペレーションID]' でアクション 'Microsoft.Purview/locations/operationResults/read' を実行する認可がないか、スコープが無効です。」というエラー内容が出ており、
オペレーションIDをリクエスト後、閲覧者権限が不足している旨のエラーとなります。
これはAzure Resource Managerで設定保存の状況を追跡しており、その追跡リクエストが通らないためのエラーです。

エラー表示なく、設定を変更するために、Microsoft Purview アカウントをホストする Azure サブスクリプションに「IAM 閲覧者ロール」の付与をお願いいたします。

## 解決法

「Microsoft Purview アカウントをホストする Azure サブスクリプションに IAM 閲覧者ロール」の権限が付与されているかご確認ください。
> [!TIP]
> Azure Resource Managerで設定の状況を追跡をするため、上記の権限が必要になります。

Purviewアカウントのネットワーク設定の際に必要な権限・情報は以下の公開情報にもまとめられております。
[Microsoft Purview アカウントのファイアウォール設定を構成する](https://learn.microsoft.com/ja-jp/purview/catalog-firewall)

