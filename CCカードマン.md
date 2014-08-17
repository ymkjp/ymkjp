PaymentAdvocate
===
ぼくのかんがえたさいきょうのペイメントレビューサイト

["時間 (ランチタイム不可など)", "金額 (5000円以上のみなど)"]

## 仕様
#### モック
* [iOS](https://popapp.in/w/projects/539df04ea5148d250e1014e9/preview/539df076f8e2acbd3c0332fa?transition=dismiss&t=1402861252946)

#### 検索機能
検索クエリ
* Current Location
* Tag (icon で簡単絞り込み)
  * Category
  * Payment

検索結果
* Pins <sub>[1](#1)</sub>
  * Shop URL
  * Map URL
  * Score
  * Name
  * Reviews

<a name="1">1.</a> 一緒に複数のカテゴリをみたくなることはないのでここは icon でなく pin にして Goodness (based on 51%+ reviewers) を表示

#### お店登録
クレジット払い可の店をクローリング
* 主要3区 (Minato, Shinjuku, Shibuya)

shops.length < 1
* Name
* Location
* Category
* Payment
* URL

report shops.@id outdated

#### タグ機能
公式カテゴリタグ
* レストラン (knife & fork)
* カフェ (coffee cup)
* 小売店 (shopping bag)
* 美容院 (Scissors & comb)
* 医療機関 (Hospital)

支払い方法タグ
* Visa
* Suica

## Web API 設計
#### {GET|POST|DELETE|PUT} `example.com/v1/*`
* `RESOURCE shops/@id/`
* `GET shops?q=roppongi+drink&lat=-33.8750460&lng=151.2052720`

```
{
    "id" : 303,
    "location": {
      "lat" : -33.8750460,
      "lng" : 151.2052720
    },
    "categories": ["Shop", "Alcohol"],
    "score": 20,
    "moreInfo": "http://example.com/v1/shops/@id/detail"
}
```

* `GET shops/@id/detail`

```
{
    // Same to @id ...
    "name": "鈴坂六本木店",
    "reviewCount": 12,
    "siteUrl": "http://tabelog.com/tokyo/A1309/A130905/13004158/"
    "icon" : "http://maps.gstatic.com/mapfiles/place_api/icons/restaurant-71.png",
    "score": 30,
    "repReview": {
      "score": 0,
      "moreToRead": "http://example.com/v1/comments/@id/",
      "comment": "この店は六本木ヒルズカードを持っている人には...",
      "userId": 1003
    },
    "reviewsUrl": "shops/@id/reviews"
}
```

* `GET users/@id/`

```
{
    "id": 1003,
    "name": "ymkjp",
    "lnag": "ja",
    "rank": 2,
    "averageScore": 0.30,
    "reviewCount": 45,
}
```

* `GET users/@id/territories`
 * territories: 過去訪れたエリア

```
{
    "id": 1003,
    "territories": [{"regionId": 30, name": "Minato", "ReviewCount": 45}]
}
```

* `GET reviews/@id/`

```
{
    "id": 18003,
    "userId": 1003,
    "shopId": 303,
    "score": 0,
    "queryString": "六本木 酒屋 鈴鹿",
    "comment": "この店は六本木ヒルズカードを持っている人には5%OFFで売っているのに、クレジットカード払いの際には割引を適用してくれません",
    "shopUrl": "http://tabelog.com/tokyo/A1309/A130905/13004158/",
    "location": {
      "lat" : -33.8750460,
      "lng" : 151.2052720
    },
    "lastModified": 1402845400
}
```

* `GET favorites/@id/`

```
[
  {
    "userId": 18003,
    "shopId": 303,
    "addedAt": 1402845400
  }
]
```

* `GET /search?q  =fluffy+fur&type=shop`
 * named entity extraction by GeoNLP

```
{
  results: []
}
```








## RESTful API 参考
#### JSONレスポンス
```
{
    "developerMessage": "開発者向けメッセージ",
    "userMessage": "ユーザ向けメッセージ. 必要に応じて",
    "errorCode": 1234,
    "moreInfo": "http://dev.example.com/errors/1234"
}
```

JavaScript の規約にあわせる
先頭小文字のキャメルケース

#### バージョニング
v と整数のバージョン番号を URL のトップレベルにつける. e.g. `/v1/dogs`

#### Partial response
`fields` パラメータにカンマ区切りで指定
e.g. `/dogs?fields=name,color,location`

#### Pagination
`limit` と `offset` パラメータで指定する. offse 番目から limit 件取得
e.g. `/dogs?limit=25&offset=50`
全レコード件数を metadata としてレスポンスに含めてあげる
省略時のデフォルト件数はデータサイズやアプリケーションによって決める

#### ソース操作ではない API のデザイン
計算・翻訳・変換など, ドメインによってはリソース操作でない API も存在しうる
その場合名詞でなく動詞をつかう
e.g. `/convert?from=EUR&to=CNY&amount=100`

#### 検索のための tips
検索は複雑なクエリが想定されるので, Google にならって動詞の URL にするのがよい
`/search?q=fluffy+fur`
`q` に検索クエリを指定
検索のスコープを絞る場合は search の前にスコープをつける
`/owners/5678/dogs?q=fluffy+fur`
結果のフォーマットは拡張子ふうに指定
`/search.xml?q=fluffy+fur`

#### API リクエスト先をひとつの subdomain にまとめる
Facebook や Twitter は種類によってサブドメインがわかれているが, ひとつのまとまっている方がよい
開発者ポータルも `developers.example.com` に作るとよい

#### クライアントが HTTP エラーコードをインターセプトする場合

たとえば Flash は HTTP のエラーレスポンスを受け取ると, エンドユーザのアプリにエラーコードを表示する
これを避けるために `suppress_response_codes=true` というクエリパラメータを提供する
これが指定されていると HTTP レスポンスコードが常に 200 になるようにする
レスポンスボディは通常通りのエラーコードやメッセージを返す. クライアントアプリはこれを見てハンドリングする

#### 認証
OAuth 2.0 を使う

#### Chatty API (おしゃべりな API, つまり情報が少なく何度も呼び出さないといけないもの) をいかに避けるか
まず RESTful に設計し, そのごショートカットを追加する
複数の API 呼び出しを組み合わせないと実現できない使い方を, 一度の呼び出しで実現できる API を追加してあげる
先にショートカットを作ってはいけない. まずは RESTful なデザインで, 必要に応じてショートカットを追加する
partial response にドット演算子を導入して, 別のリソースのフィールドを参照できるようにする
`/owners/5678?fields=name,dogs.name`

#### API Facade Pattern
システムのフロント (API) とバックエンド (システム本体や DB など) のつなぎかた. Facade Pattern をつかう
フロントとバックエンドの間に抽象的なレイヤーを一枚はさむ
設計のしかた
まず理想的な API インタフェースを設計する
stub を用意して上記のインタフェースを実装する
facade と実際のシステムをつなぐ
