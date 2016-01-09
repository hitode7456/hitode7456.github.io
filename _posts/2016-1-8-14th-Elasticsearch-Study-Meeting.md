---
layout: post
title: 第 14 回 Elasticsearch 勉強 参加報告
---

[第 14 回 Elasticsearch 勉強会](https://elasticsearch.doorkeeper.jp/events/36330)
に参加してきました。

通常は利用例の発表が多いのですが、今回はそれだけでなく、機械学習や
QueryParser 等、いつもより技術よりの話が聞けました。

# ココが辛いよ Elasticsearch

+ 発表者:株式会社リクルートテクノロジー 高林貴仁 さん (@tatakaba)
+ 発表資料: 

最初にリクルートテクノロジー社での Elasticsearch の活用方法を解説してい
ただき、その後、ご経験された Elasticsearch のツラい点を聞くことができま
いた。

発表資料を見つけることができませんでした。しかし、活用事例は以下の資料
に見覚えがあるスライドがいくつか有りました。

[リクルート流Elasticsearchの使い方](http://www.slideshare.net/recruitcojp/elasticsearch-56355817)

去年末に行わ得れた
[elastic{ON}](https://www.elastic.co/elasticon/tour/2015/tokyo) での発
表資料のだと思います。大変勉強になります。

ツラいポイントとしてメモできたのは

**Elasticsearch バージョン間のデータ非互換性がツラい**

Solr にも同様の縛りが有りますね。メジャーバージョンが変わるとデータの互
換性がなくなるようです。

対策としては、

1. [マイグレーションツール](https://github.com/elastic/elasticsearch-migration)を使う
2. 新しいバージョンのシステムを用意し、現バージョンのシステムのデータに
   追いついたら切り替える

があるそうです。

(1.) については、これから調査します。

(2. )については、クラウド環境なら手軽に出来るかもしれませんが、オンプレ
だと大変だ、という話がありました。

**フィード名に"."が使えなくなったのがツラい**

Elasticsearch では nest したデータ構造があつかえます。曖昧さを排除する
ために、 2.0 系から "." をフィールド名に含めることができなくなりました。

新たに 2.0 系から使いはじめる場合は大きな問題には無さそうです。しかし、
すでに "." が含まれているフィールド名を使って運用しているシステムの場合、
修正が大変そうです。

**River が非推奨(2.0 系では全般)になったのがツラい**

River は Elasticsearch にデータを投入するためのインターフェースみたいで
す。様々なデータの供給元に対応する Plugin があります。

Solr の Data Import Handler みたいなものでしょうか。

以前 johtani さんが、負荷が高くなりがちなので、廃止。代わりに Logstash
を使いましょう、ということを発表していた記憶があります。

**データ更新時にずれるのがツラい**

私自身、まだそれほど Elasticsearch を使いこでいないません。したがって、
本事象が発生する構成や、原因についてピンとはきませんでした。

リクルートテクノロジー社では、投入ドキュメント数を記録し、
Elasticsearch のドキュメント数と差分が生じた場合は再投入する、という対
策をとっているそうです。

**replication asyncが非推奨(2.0 系からは廃止)**

これにより何かのスピードが遅くなったという内容だったと思います。

**その他**

(ツラい点だったかどうか覚えていませんが) 2.0 系で Aggregation の機能は
拡充されたが、検索機能自身に目立った拡張がない。したがって、バージョン
アップのモチベーションが上がらない、というのも有りました。

ツラいポイント、とは別に興味深く重ったポイントとしては

- Zookeeper 使っている
- Elasticsearch のバージョンは結構頻繁
- 機械学習使っている
- A/B テストしている
- Kibana を社内ツールとして利用している
- Plugin を使って Solr の Query を Elasticsearch でも受け付けるようにしている

などが有りました。

(やっぱり最初の発表は集中している分、メモが一杯残っています。)

# 機械学習を利用したちょっとリッチな検索

+ 発表者:  Preferred Networks America, Inc. CTO 久保田展行 さん  (@nobu_k)
+ 発表資料: http://www.slideshare.net/nobu_k/ss-56810268

検索システムの外部で、機械学習を利用する内容でした。

検索対象になるデータを機械学習にかけることにより、元データにはない情報
を加えます。

それを検索システムに投入することにより、元データだけで利用するのとは、
違った角度での検索が可能になります。

個人的に機械学習を勉強し始めたところなので、とても興味深く聞けました。

少し敷居の高さを感じていたのですが、nobu_k さんの「機械学習をツールとし
て使うことに割り切る」という言葉が印象に残りました。

発表の内容は機械学習の1つのジャンルである「分類」をつかって、リッチな検
索機能を提供しよう、と言った内容です。

R, Python, Jubitus, Fluentd, Chainer 等の単語がチラホラ出てきていました。

# Lucene Query 再考 - Domain Specific Query 実装 -

+ 発表者: Supership株式会社 インフラ事業開発本部検索グループ 大川真吾 さん (@okawa_shingo)
+ 発表資料: [http://www.slideshare.net/ShingoOKAWA/elasticsearch-20150107-56772462](http://www.slideshare.net/ShingoOKAWA/elasticsearch-20150107-56772462)

知り合いの方でした。。。

Elasticsearch -> Lucene 間のプラグインの話で、[ANTLR](http://www.antlr.org/) をつかって構文定義
できる QueryParser を作ったという内容です。

発表資料を見る前に、

[Domain Specific Query Parser (Hetero Grammatical Query Parser) 実装](http://qiita.com/ShingoOKAWA/items/3e2e195a923e08a47388)

を参照されたほうが、内容がつかみやすいと思います。

QueryParser そのものは GitHub で
[公開](https://github.com/supership-jp/elasticsearch-ss-query-parser)し
てくれています。

# LT 1 : Fluentd meets Beats

+ 発表者: Treasure Data Inc Masahit Nakagawa さん (@repeatedly)
+ 発表資料: [http://www.slideshare.net/repeatedly/fluentpluginbeats-at-elasticsearch-meetup-14](http://www.slideshare.net/repeatedly/fluentpluginbeats-at-elasticsearch-meetup-14)

発表者は fluentd のメンテのかた。

Beats といのは Elastic 社が開発しているモニタリングツールです。

エージェントとしてサーバーに常駐し、取得したデータを Elasticsearch や
Logstash に発信します。

例えば Zabbix エージェントは、サーバーにつき 1 つのエージェントを立ち上
げ、それに対して Plugin を導入することにより、様々なモニタリング対象に
対応します。

対して、Beats のエージェントは、1つのエージェントのモニタリング対象は1
つになっておいて、複数の対象をモニタリングするさいには、複数のエージェ
ントを起動することになる、という特徴があります。

発表内容は Beats から Fluentd にデータを流し込めるようにした、というも
のでした。

[こちら](http://qiita.com/repeatedly/items/77af41788f0b3ccdefd2)も参考
になります。

# LT 2 : Elasticsearch インデクシングのパフォーマンスを測ってみた

+ 発表者: 日本 IBM 黒澤亮二さん (@rjkuro)
+ 発表資料: [http://www.slideshare.net/kuron99/elasticsearch-56784623](http://www.slideshare.net/kuron99/elasticsearch-56784623)

インデクシングの効率は、システム設計上非常に重要です。

一度運用をはじめたら、簡単に変更できる部分ではないので、事前にこういっ
た情報があると、非常に助かります。

いろいろな計測結果があるので、後でじっくり確認して見ます。

[Elasticsearchインデクシングパフォーマンスのための考慮事項](http://qiita.com/rjkuro/items/e79eec7ffb0511b7c678)
も一緒に読むとより理解が深まりそうです。









