PerfectRails
===

Rails を趣味でしか使ったことがないプログラマが『パーフェクト Ruby on Rails』を読んだら
## 目次
```
Part1 Rails ~ overview
・1章 Ruby on Railsの概要
・2章 Ruby on RailsとMVC

Part2 実践テクニック
・3章 アセット
・4章 Railsのロードパスとレイヤーの定義方法
・5章 開発を効率化するgem

Part3 実践Webアプリケーション開発
・6章 Railsアプリケーション開発
・7章 Railsアプリケーションのテスト
・8章 Railsのインフラと運用

Part4 一歩先行くRails
・9章 より実践的なモデルの使い方
・10章 Railsを拡張する
```

## Railsの思想
趣味だったからというより触れる時間の短さから思想まで意識することは少なかった。
こうしてまとめられてみると確かにそうだよねっていう感覚。

* CoC (Convention over Configuration)
* DRY (Donit Repeat Yourself)
* REST (Representational State ttransfer)
* 自動テスト

## Part1 Overview
#### 例外処理
趣味プログラミングだったのでどうしても例外処理はおろそかになりがちだった。

![例外とステータスコードの対応表](http://i.gyazo.com/947863874dc804f10b01d922d202a4e6.png)


例: ログイン失敗

```ruby
class LoginFailed < StandardError
end

class ApplicationController < ActionController::Base
  rescure_from LoginFailed, with: :login_failed

  def login_failed
    render template: 'shared/login_failed', status: 401
  end
```

```ruby
class LoginController < ApplicationController
  def create
    @user = User.where(name: params[:name], password: params[:password]).find
    raise LoginFailed unless @user
  end
```

## Part2 実践テクニック
さすがに Rails でアプリケーションを何度かつくったことはあったので2章の内容くらいまではだいたい把握していた。
しかし3章以降の怒涛は「なるほど〜」の嵐。

#### アセット周り
* Sprockets
* Turbolinks

要するに Rails で使われているいくつかの gem ライブラリを理解するということなんだけど、一度理解するとどうってことはないことも理解していなかったころは問題にぶちあたっては回避方法を場当たり的に試しているだけだったので、今回を機に理解を深められたのはよかった。


何だかんだ `README.md` を読んだだけだと他の gem と重要度の差が分からないのでこうしてフューチャーされるとありがたい。

```
//= require jquery
```

ってよくみるけど Sprockets の独自記法だったのね、とか。


#### MVC以外のディレクトリの使い方
* lib
 * api client
 * rake tasks
* app
 * 非同期処理 sidekiq
 * デコレータ active_decorator

うおお active_decorator よさそうだー、なんて思ったりしてた。
`app/decorators/user/decorator.rb`
```ruby
module UserDecorator
  def email_link(body = nil, &block)
    if block_given?
      link_to("mailto:#{email}", &block)
    else
      link_to(body, "mailto:#{email}")
  end
end
```
