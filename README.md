# オープンデータ作成支援ツール（プロトタイプ版）

## 概要

これはオープンデータのフォーマット/ルールに沿ったデータの作成を支援するアプリケーションです。

本アプリケーションでは政府が公開している[推奨データセット](https://www.digital.go.jp/resources/data_dataset/)で指定されているものを対象としています。

推奨データセットには17種類のデータが指定されており、それぞれのデータには10近くの項目（住所や位置情報、電話番号など）があり、各項目の共通フォーマット/ルールが[項目定義書](https://cio.go.jp/sites/default/files/uploads/documents/opendata_suisyou_dataset_teigisyo.xlsx)として策定されています。

この項目定義書に書いてある通りに地道に手作業でデータ作成を進めていくことも可能ですが、ルールそのものが複雑だったり、ルールが書いてある場所が異なるのでリンクをたどる必要があったり、単純な手作業でなく、複雑な手作業になります。その結果、ヒューマンエラーを避けられなかったり、オープンデータ作成にはじめて取り組もうとするときの学習コストが高かったりする問題があります。

このアプリケーションでは、画面の案内に従ってマウスとキーボードを操作するだけで、手元のCSVファイルを項目定義書の要件に沿った形式のオープンデータに整形できます。

## アプリケーションについて

### デモ

http://open-data-tools.vercel.app/

### 留意事項

2022年12月17日現在、本アプリケーションはプロトタイプ版であるため動作保証はいたしません。

### 対応しているデータセット（2022年12月17日現在）

- 公共施設一覧

### データバリデーションの処理内容

| 項目名              | バリデーション（確認処理内容）                                                                |
|------------------|--------------------------------------------------------------------------------|
| 都道府県コード又は市区町村コード | 6桁数値であるか                                                                       |
| NO               | 10桁数値であるか                                                                      |
| 都道府県名            | 県や都まで含めた、正式名称であるか                                                              |
| 市区町村名            | 市区町村まで含めた、正式名称であるか                                                             |
| 名称               | 確認処理無し                                                                         |
| 名称_カナ            | カタカナであるか（半角入力自動処理）                                                             |
| 名称_通称            | 確認処理無し                                                                         |
| POIコード           | ４桁数値又は、実在するPOI数値であるか（独自にPOI一覧を用意している）                                          |
| 住所               | Geoloniaの住所データ（オープンソース）を利用し正規化                                                 |
| 方書               | 確認処理無し                                                                         |
| 緯度               | 10進数、小数点以下6桁であるか、日本領域であるか                                                      |
| 経度               | 10進数、小数点以下6桁であるか、日本領域であるか                                                      |
| 電話番号             | 10桁以上15桁以内であるか   国際電話及び市外局番始まりであるか（独自に市外局番一覧を用意している）   セパレーター（半角ハイフン「-」）が含まれるか |
| 内線番号             | 確認処理無し                                                                         |
| 法人番号             | 確認処理無し                                                                         |
| 団体名              | 確認処理無し                                                                         |
| 利用可能曜日           | 曜日漢字（月火水木金土日）であるか                                                              |
| 開始時間             | 時間表記がhh:mmであるか                                                                 |
| 終了時間             | 時間表記がhh:mmであるか                                                                 |
| 利用可能日時特記事項       | 確認処理無し                                                                         |
| 説明               | 確認処理無し                                                                         |
| バリアフリー情報         | 確認処理無し                                                                         |
| URL              | `https://` または `http://` 書き出しと、それに続く表記であるか                                     |
| 備考               | 確認処理無し                                                                         |

### 推奨（非推奨）のブラウザ環境

- 推奨
    - Microsoft Edge 最新版
    - Google Chrome 最新版
- 非推奨
    - Internet Explorer
    - macOS Safari

## 開発について

### 開発環境

- Node.js: v16.x

### 起動方法

プロジェクトのルートで、

```
$ sh webclient_dev.sh
```

を実行し、http://localhost:3000/ にアクセスしてください。


### ディレクトリ構成

```
.
├── datamanager # データマネージャ（データ構造の分析/提案ロジック）
│   └── nodejs # Node.js版
├── docs
└── webclient # フロントアプリ
    ├── public
    ├── src
    │   ├── assets # assetsを入れるディレクトリ
    │   ├── components # 共通コンポーネントを入れるディレクトリ
    │   ├── features # 機能ごとのディレクトリ。ここではページ毎に分けています。
    │   ├── hooks # カスタムhooks
    │   ├── providers # アプリケーションプロバイダ
    │   ├── routes # ルーティング
    │   ├── stores # recoilのstore関連
    │   ├── theme # chakra-uiのthemeを上書き
    │   └── utils # ユーティリティ
    ├── test # 動作テストに使用しているサンプルcsv
    └── types # 型定義
```

## ライセンス

本ソフトウェアは、[MIT ライセンス](./LICENSE.md)の元提供されています。

### 依存ライブラリのライセンス情報

- [webclient（フロントアプリ）](./webclient/LICENSE.txt)
- datamanager（データマネージャ）
  - [Node.js版](./datamanager/nodejs/LICENSE.txt)

### キャラクター「シビたん」

キャラクター「シビたん」のデザインは一般社団法人コード・フォー・ジャパン（Code for Japan）で著作管理を行っています。
本アプリケーションでは許諾を得て使用しています。

## コントリビュート

プルリクエスト歓迎です。まずIssueを作成して、変更したい内容を議論してください。
