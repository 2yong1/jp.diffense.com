---
title: DIFF-2019-001 

subtitle: Coinbase 取引所ハッキング攻撃 | RDP Bluekeep | AppXSVC 権限昇格脆弱性 | Win10 ヒープオーバーフロー

---


--- 

# Issues

### Coinbase 仮想通貨取引所攻撃

2019年6月 19日、 仮想通貨取引所Coinbaseのセキュリティ担当として勤めている Philip Martinという人が以下の内容をツイートしました。 ([https://twitter.com/SecurityGuyPhil/status/1141466335592869888](https://twitter.com/SecurityGuyPhil/status/1141466335592869888))

![](https://user-images.githubusercontent.com/50191798/60066734-068cbb00-9743-11e9-807a-e6c559f2ec39.png)

2019年6月17日、自社従業員のPCを対象としたFirefoxブラウザの0-Day脆弱性を用いたハッキング攻撃試しを検知したとの内容でした。
この事件はお金を直接取り扱う取引所に対する攻撃だったため、結構大きな話題になりました。


攻撃に使われた脆弱性はFirefoxを踏み台としたリモートコード実行脆弱性(CVE-2019-11707)とサンドボックス脱出（実行権限昇格）脆弱性 (CVE-2019-11708)の組み合わせでした。
Coinbaseは攻撃チェーンに使われた2個の脆弱性をMozillaへ報告しました。ここで面白いのが本件により報告された脆弱性の一つがGoogle Project Zeroチームの Saeloがすでに報告 (2019/04/15)と同じものだったことです。

ハッカーはどうやってその脆弱性情報が入手できたでしょう？
ハッカーも同じ脆弱性を発見してた可能性もありますが、Mozillaのバグトラッキングシステムのアクセス権限を持っている可能性もあります。本脆弱性情報の入手経緯についてはいろんな可能性の噂が出ている状態です。
今回の攻撃方式はターゲットとなった従業員にフィッシングメールを送り、添付のリンクをクリックするとFirefoxの脆弱性が叩かれるような仕組みとして動作します。
この攻撃が成功すると最終的にRAT(Remote Administrator Tool)が PCにインストールされます。

RATは WindowsとMac用がそれぞれ存在しMac用コードはここ([https://objective-see.com/](https://objective-see.com/blog/blog_0x43.html))に分析情報があります。悪性コードのダウンロードも可能です。（分析用途だけにお使いください）
悪性コードの分析内容を簡単に整理すると以下になります。(詳細は上記のサイトを参照してください。)

* 本悪性コード(OSX.Netwire)は分析時点のVirustotalに登録されているベンダーの中Tencent だけが探知した。
* 本悪性コードは2012年にDr.Webにより始めて検知されたもののLinux/Macを対象とする初めてのパスワードスティーラーだった。
* キーローギングとディスク内のファイルからパスワード情報を盗む
* 2012年の検体と2019年の検体（今回の検体）は似ていながら異なる部分も多く、おそらく同一開発者により作られたものと思われるが、作成目的は完全に異なるようで詳細は追って掲示する予定


取引所の従業員のPCをハッキングする目的は最終的に取引所の電算システムへアクセスし仮想通貨を奪取することでしょう。
しかし、すでに国内外の金融機関や取引所のハッキング事故事例からこのような機関の従業員のPCがハッキングされると単純な金銭奪取以上の被害が発生しうることを経験しています。
最近は金融機関に比べ比較的規制が劣る仮想通貨取引所に対する攻撃が増加している推移で関係各位に注意が必要です。


参考URL:

* [Firefox 0-day Used in Targeted Attacks Against Cryptocurrency Firms](https://www.bleepingcomputer.com/news/security/firefox-0-day-used-in-targeted-attacks-against-cryptocurrency-firms/)
* [Burned by Fire(fox)](https://objective-see.com/blog/blog_0x43.html)
* [Martin's a Tweet](https://twitter.com/SecurityGuyPhil/status/1141466335592869888)


<br>

### RDP BlueKeep(CVE-2019-0708)

![Micrsoft Security Updates](https://user-images.githubusercontent.com/50191798/60060521-fa493380-972b-11e9-9f92-8ba9273f04e5.png)
2019年5月 14日, RDP(Remote Desktop Services) サーバ脆弱性の [アップデート](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/CVE-2019-0708)がリリースされました。

この脆弱性は発表当時大きな波紋を起こしました。なぜなら、この脆弱性は <U>認証を通さずRDPサーバを遠隔からハッキングできるものの、本脆弱性の影響を受けるPCが数百万台も至る</U>と推定されたためです。 Windows8 以前のOS(WinXP, 7, 2008など)が全てこの脆弱性の影響を受ける対象でした。

本脆弱性の発表直後は偽PoC（Proof of concept)コードが Githubに多数登録され、cve-2019-0708.comというサイトでは偽物のExploitコードも販売されるハプニングもありました。

![Fake Exploit](https://user-images.githubusercontent.com/50191798/60064826-9da24480-973c-11e9-93dd-6bbe736892a7.png)

このような騒動があった後、数々の研究者とセキュリティ会社では本脆弱性の分析内容を共有しました。 

* McAfee, [RDP Stands for “Really DO Patch!” – Understanding the Wormable RDP Vulnerability CVE-2019-0708](https://securingtomorrow.mcafee.com/other-blogs/mcafee-labs/rdp-stands-for-really-do-patch-understanding-the-wormable-rdp-vulnerability-cve-2019-0708/)
* ZDI, [CVE-2019-0708: A COMPREHENSIVE ANALYSIS OF A REMOTE DESKTOP SERVICES VULNERABILITY](https://www.zerodayinitiative.com/blog/2019/5/27/cve-2019-0708-a-comprehensive-analysis-of-a-remote-desktop-services-vulnerability)
* MalwareTech, [Analysis of CVE-2019-0708 (BlueKeep)](https://www.malwaretech.com/2019/05/analysis-of-cve-2019-0708-bluekeep.html)

本脆弱性はカーネル(termdd.sys)で発生する UAF(Use-After-Free)に起因する脆弱性で詳細は上記のリンクを参照してください。

本脆弱性に対しては脆弱性スキャナーも多数公開されています。 Qihoo360社はウェブ上で使えるスキャナーの公開(2019/04/19)をはじめ、 (eternalblue分析で有名な)zerosum0x0という研究者もスキャナーを公開(2019/04/22)しました。

* [CVE-2019-0708 remote scan tool by 360Vulcan team](https://twitter.com/mj0011sec/status/1130387741538054144)
* Qihoo360社のスキャナーは一般ユーザ向けには公開せず、 メールにて問い合わせが必要に見えます。
![](https://user-images.githubusercontent.com/50191798/60062762-7bf18f00-9735-11e9-9bb0-0d31df976fa8.png)
* zerosum0x0 Github, [Scanner PoC for CVE-2019-0708 RDP RCE vuln](https://github.com/zerosum0x0/CVE-2019-0708)
  * rdesktopを修正して作ったスキャナーです。 github からダウンロードしテストできます。

Qihoo360, McAfee, Theori などのセキュリティ会社は本脆弱性を用いた攻撃デモの動画は公開しましたが、Exploit Codeに関する内容は世界平和のため(?)公開していません。その内容が公開されたらいろんな攻撃ツールキットに攻撃コードが載せられ被害が急増するでしょう。

パッチがリリースされてから１ヶ月も立っている2019年6月末、現在も数百万台のPCがアップデートされずこの脆弱性に露出されています。

![](https://user-images.githubusercontent.com/50191798/60063263-48176900-9737-11e9-8291-43486f8bb234.png)

EternalBlueが Shadow Brokerにより世に公開された(2017年 4月)後、１ヶ月くらいで ランサムウェアで有名なWannaCry(2017年 5月)にその攻撃が搭載されたことを考えると近いうちにBlueKeep の攻撃コードが大規模攻撃に使われる日が来る可能性もあると思います。 

RDPを使っている方々（Pre-Windows8 ユーザも)は必ずWindowsアップデートを行いましょう。

---


# Vulnerabilities

### [CVE-2019-1064 AppXSVC Local Privilege Escalation](https://www.rythmstick.net/posts/cve-2019-1064/)

SandboxEscaperが 2019年 6月に公開した *Windows AppX Deployment Service(AppXSVC)* 権限昇格0-dayに対する内容です。本脆弱性のパッチは6月のWindows Updateに含まれています。
脆弱性の内容を要約すると

* AppData\Local\Packagesのサブフォルダー(例:LocalState)が削除されると、AppXSVCがそのフォルダーを再度生成する
* <U>LocalState フォルダーが再度生成される時、当該フォルダー内のファイル DACLが変更される</U>(一般ユーザ権限でもFull Controlが可能になる)
* Race Conditionを用いて任意ファイルの DACLを変更することが可能
* 攻撃手順
  * LocalStateフォルダーを削除し、再度生成されるまで待機
  * 攻撃コードは当該フォルダーの配下にファイル(rs.txt)を生成し、任意ファイルに(例: c:\windows\system.ini)へ Hard linkして置く
  * AppXSVCは system.iniの DACLを変更するため、一般ユーザ権限のアカウントに任意ファイルに対する Full Control 権限が与えられる



---

# Techniques

### ["Heap Overflow Exploitation on Windows 10 Explained"](https://blog.rapid7.com/2019/06/12/heap-overflow-exploitation-on-windows-10-explained/)

Corelanのメンバーだった Wei Chenという方が Windows 10 ヒープオーバーフローエクスプロイト方法を記述した記事です。
要約すると

* ヒープオーバーフローを成功させるにはメモリチャンクを適切な位置に配置させる必要があり、それら間 のオフセットが予測できないといけません。 
* 同一なサイズのヒープメモリチャンクを連続的に割り当てする際、Win7ではメモリチャンク間のオフセットが一定だが、Win10ではランダムになります。しかし、常にランダムになるわけではなく <U>LFHがアクティブになるまではチャンク間のオフセットが一定であることを利用</U>し、LFHがアクティブになる前にヒープメモリのレイアウトを合わせた後、オーバーフロー攻撃を実行すべきと言っています。.
* 記事ではBSTR 文字列オブジェクトを用いたメモリリーク例を紹介しています。また、Vectorオブジェクトを用いたコード実行例を紹介しています。
