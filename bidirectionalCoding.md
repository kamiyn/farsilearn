# ソースコード内に RightToLeft文字列リテラルを埋め込む時に文字がすっ飛んでいくのを解決したい

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

This is a farsi phrase without RLI+PDI:
> "این زلزله در سال ۲۰۱۱ اتفاق افتاده است."

ピリオドがペルシア語文の末尾=左端に来てほしいが、右端に飛んでいる。
ここでダブルクオートの内側に RLI, PDI を追加すると次のようになる。

This is a farsi phrase with RLI+PDI:
> "⁧این زلزله در سال ۲۰۱۱ اتفاق افتاده است.⁩"
```
"[RLI]（ペルシア語文）[PDI]"
```

![ScreenShot Of VSCode](https://raw.githubusercontent.com/kamiyn/farsilearn/images/vscode.png)

RLI/PDI で囲んだとしても、VSCode は数字を Left To Right で表示してくれる。ここでは4桁の数字 2011

# 参考資料

http://www.unicode.org/reports/tr9/tr9-42.html Unicode® Standard Annex #9 (Revision 42)

[日本語WikiPedia 双方向テキスト](https://ja.wikipedia.org/wiki/%E5%8F%8C%E6%96%B9%E5%90%91%E3%83%86%E3%82%AD%E3%82%B9%E3%83%88)
