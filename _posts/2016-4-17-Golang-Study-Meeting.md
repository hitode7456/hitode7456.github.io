---
layout: post
title: 実践Go言語勉強会〜サービス導入している3社がGo言語の魅力をお伝えします〜 参加報告
---

[実践Go言語勉強会〜サービス導入している3社がGo言語の魅力をお伝えします〜](https://atnd.org/events/75883)
に参加してきました。

私自身 Go 言語を使い始めたばかりで、他社様の活用事例を参考にさせていただ
きたいと思っていたところなので、ちょうどいい機会でした。

3 社様の事例を聞かせていただきましたが、内容に共通点が多かったのが印象
的です。

WAF にしても Mapper にしても乱立状態前のようで、どの会社様も同じよな構
成をとっているように思えました。

時間が過ぎてしまってうろ覚えなので、メモを箇条書きにします。

# Go言語の実践運用で得たノウハウと、気づいた魅力について

**発表者: 株式会社インテリジェンス 吉元 裕人 様**

+ コマンドラインツールは最初から go 言語を利用していた
+ ユーザー画面は [Hack](http://hacklang.org/) を利用している
+ 管理画面は [Revel](https://revel.github.io/) を利用している
+ オンラインリファレンスが充実
  + [A Tour of Go](https://go-tour-jp.appspot.com/list)
  + [Effective Go](https://golang.org/doc/effective_go.html)
+ 標準のツールが充実
  + [goimports](https://godoc.org/golang.org/x/tools/cmd/goimports)
  + [gofmt](https://golang.org/cmd/gofmt/)
  + [golint](https://godoc.org/github.com/golang/lint/golint)
  + [vet](http://golang-jp.org/pkg/code.google.com/p/go.tools/cmd/vet/)
+ WAF も色々
  + [Revel](https://revel.github.io/)
  + [Gin](https://gin-gonic.github.io/gin/)
  + [Martini](https://github.com/go-martini/martini)
  + [Goji](https://github.com/zenazn/goji)
  + [Echo](https://github.com/labstack/echo)
+ WAF は Revel を使っている。Revel は
  + Play Framework の影響を受けている
  + 機能豊富
  + Go のシンプルさが失われている、という話もある
+ ORMapper
  + [Gorm](https://github.com/jinzhu/gorm)
  + [Xorm](http://xorm.io/docs)
  + [Gorp](https://github.com/go-gorp/gorp)
+ Testing
  + [Testify](https://github.com/stretchr/testify)
  + [Ginko](https://onsi.github.io/ginkgo/)
  + [Gocheck](https://labix.org/gocheck)
  + [Goconvey](http://goconvey.co/)
+ インテリジェンス様では Testify 採用
+ Vendaring
  + Go 言語の公開ライブラリは標準のパッケージ管理ツールではバージョンまで指定できない
  + そのような場合は自分のレポジトリに取り込み、一緒に管理するようにする
  + go submodule command
  + [Glide](https://github.com/Masterminds/glide)
+ go 1.4 から 1.5 でコンパイルが遅くなった
+ コンパイラが Go で書かれていることが原因?
+ コンパイル時に -i オプションをつけると速くなる
+ コマンドラインツールは複数作っていたが、バイナリは一つにしてサブコマンドで処理内容を分けるようにした
  + 共通部分のコンパイルをいっしょにするようにするため
+ 開発に使って感じたこと
  + 開発しやすい
  + 使っていて楽しい

# トークノートの事例紹介(仮)

**発表者トークノート株式会社 三浦 堅右 様**

+ 既存のシステムで解決したかった課題
  + 既存のシステムは PHP を使っていた
  + 並行処理が簡単にできない
  + バグがテストでは抑えきれない
  + モノリシックな作り
+ Go の良い点
  + 習得容易
  + 高速動作
  + デフォルトのコードフォーマッティング機能
  + コンパイル時の厳格なエラーチェック
  + スタックとヒープをよしなに管理
+ Go の見劣りする点
  + 言語仕様が若干古い(map とか reduce がない)
+ Go を採用した理由
  + プログラミングガバナンスをきかせやすい
  + デプロイが簡単
  + 並行処理
  + チャンネルによるメッセージパッシング
+ ORMapper は Gorp を採用
+ Auth は [Osin](https://github.com/RangelReale/osin) を採用
+ Venting は Glide を採用
  
# エウレカの事例紹介(仮)

**株式会社エウレカ 三津澤 サルバドール 将司 様**

+ PHP -> Go へ。一年かけたプロジェクト
+ gofmt もあるし、他人の書いたコードを読むのが容易
+ スタンダードライブラリが秀逸
  + net/http
    + これがあるから WAF が流行らない?
  + database/sql
    + connection buffering も完備
+ モダンな syntax がないが、チーム開発には不要
+ WAF や Testing Framework にはまだスタンダード無し
+ Migration tool には [Goose](https://bitbucket.org/liamstask/goose/) を採用
+ db が null だと、go にいれると 0 にってしまう。あえて 2 を false 扱いにした
+ null を pointer でうけるか、null string を採用するかは迷った
+ 日付解析が毎回面倒
+ 使いやすいテンプレートエンジンがない












