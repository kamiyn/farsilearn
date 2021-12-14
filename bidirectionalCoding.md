# ソースコード内に RightToLeft文字列リテラルを埋め込む時に文字がすっ飛んでいくのを解決したい

*以下の解決方法は VSCode, VS2022 において2021年12月現在使えなくなっています。*

セキュリティ上の理由で制御文字として機能しなくなったため。

https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-42574
https://code.visualstudio.com/updates/v1_62#_unicode-directional-formatting-characters
https://docs.microsoft.com/en-us/visualstudio/releases/2022/release-notes#17.0.3.0

## 解決方法 (2021年10月まで)

RightToLeft な文字列を

```
RIGHT‑TO‑LEFT ISOLATE (U+2067 略称 RLI)
```
と
```
POP DIRECTIONAL ISOLATE(U+2069 略称 PDI)
```
で囲む。

入力方法: IMEで 2067 [F5] ,2069 [F5]

(MS-IME には文字コード + F5 で直接入力する機能がある。少なくとも MS-IME 2007 から存在しているようだ)

## 何が問題か

This is a farsi phrase without RLI+PDI:
> "این زلزله در سال ۲۰۱۱ اتفاق افتاده است."
( [カーソル移動動画1](https://raw.githubusercontent.com/kamiyn/farsilearn/images/bidirectionalVScode1.mp4) )

ピリオド自体は方向を持たない Neutral 文字で、
文全体が Implicit LTR (既定の挙動が Left To Right) であることから、
Left To Right として評価されることに起因している。

結果として **文末の** ダブルクオートにくっついて表示される。

書き手の意図としては、ピリオドがペルシア語文の末尾
= **文頭の** ダブルクオートにくっついて表示してほしい。

## 解決方法を適用したらどうなるか

ここでダブルクオートの内側に RLI, PDI を追加すると次のようになる。

This is a farsi phrase with RLI+PDI:
> "⁧این زلزله در سال ۲۰۱۱ اتفاق افتاده است.⁩"
( [カーソル移動動画2](https://raw.githubusercontent.com/kamiyn/farsilearn/images/bidirectionalVScode2.mp4) )
```
"[RLI]（ペルシア語文）[PDI]"
```

書き手の意図通り、ペルシア語文の末尾、すなわち **文頭の** ダブルクオートにくっついて表示される。

## Right To Left 指定された領域中でも 数字列 は Left To Right

ペルシア語(おそらくアラビア語も)では数字列を Left To Right で記述する。

![ScreenShot Of VSCode](https://raw.githubusercontent.com/kamiyn/farsilearn/images/vscode.png)

RLI/PDI で囲んだとしても、VSCode は数字を Left To Right で表示してくれる。ここでは4桁の数字 2011

## RLI の UTF-8 表現

![RLICode](https://raw.githubusercontent.com/kamiyn/farsilearn/images/rliCode.png)

この文章の HexDump 表示で
RLI(UTF-8 でのバイト列 0xE2, 0x81, 0xA7) が実際に埋まっている様子を確認した。

# 参考資料

http://www.unicode.org/reports/tr9/tr9-42.html Unicode® Standard Annex #9 (Revision 42)

[日本語WikiPedia 双方向テキスト](https://ja.wikipedia.org/wiki/%E5%8F%8C%E6%96%B9%E5%90%91%E3%83%86%E3%82%AD%E3%82%B9%E3%83%88)
