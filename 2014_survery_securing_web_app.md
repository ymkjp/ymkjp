A Survey on Server-side Approaches to Securing ウェブアプリケーションlications
===

2014年4月のCSURに載ったサーベイ論文。

## 概要
#### ウェブアプリケーションの一般的な脆弱性
* 入力値バリデーションの脆弱性
* セッション管理の脆弱性
* アプリケーションロジックの脆弱性

#### 既存のセキュリティ技法の2つの次元
1. 対処中のセキュリティ上の脆弱性と攻撃
2. 実行可能な方針・段階
  * 新しくウェブアプリケーションを作るときのセキュアな構成
  * レガシーなウェブアプリケーションへのセキュリティ分析・テスト
  * レガシーなウェブアプリケーションでの実行時保護

#### 今後の研究領域
{}


## 1. INTRODUCTION

ベライゾンの調査ではウェブアプリケーションはセキュリティ侵害の件数や漏洩したデータの量でトップ [Verison 2010]

現在のウェブアプリケーション開発・テストフレームワークのセキュリティに関するサポートは限られていてセキュアなウェブアプリケーションを速くつくるのは難しい

結果 49% のウェブアプリケーションには脆弱性があり、完全に情報が漏洩してしまうような深刻なリスクを含むウェブサイトは 13% にのぼっている [WASS 2007]

最近の調査では 80% 以上のウェブサイトが一度は深刻な脆弱性を有していたことが分かった [WiteHat 2010]

#### 脅威モデル
1. ウェブアプリケーションが提供しようとしている機能そのものは無害 (悪意のあるサービスではない) であり、ハードニングされた信頼できるインフラストラクチャで運用されている
2. アタッカーはコンテンツもしくはウェブアプリケーションに送られるリクエストを操作できるが、直接サーバ上のアプリケーションコードを操作することはできない


## 2. UNDERSTANDING UNIQUE CHARACTERISTICS OF WEB APPLICATIONS

#### ウェブアプリケーションの概略図

![Fig. 1. Overview of Web application](http://i.gyazo.com/331527eceb115866c7120f1cd8746422.png)

#### 2.1  Input Validation
i.e., HTML tags, URI attribute (「サニタイズ言うな」)

#### 2.2  Session Management
* Server: application session state
* Client: cookie, hidden form, URL rewriting

#### 2.3  Logic Implementation


#### 既存のセキュリティ技法まとめ

![Fig. 2. Summary of existing techniques](http://i.gyazo.com/1396a664d4731758ae7d558c82a548a9.png)

## 出典
Li, Xiaowei, and Yuan Xue. "A survey on server-side approaches to securing Web applications." ACM Computing Surveys (CSUR) 46.4 (2014): 54.
https://www.truststc.org/pubs/910/Survey-Final.pdf
