# ClassicalCipher パッケージ (v0.1)

## 概要

古典暗号による暗号化・復号化処理を TeX（TeX on LaTeX）で実現するパッケージです．現在対応している暗号方式は以下の3種です．

* シーザー暗号
* アフィン暗号
* バーナム暗号

## 動作環境

LaTeX2e 上で動作します．TeX の処理系に依存する実装はしていないはずですが，軽い気持ちでカウンタを6つも新設してしまっているので，非e-拡張の TeX で使用する場合は注意してください．

## 使い方

### シーザー暗号

暗号化を行う際には，\Caesar コマンドを使用します．

```tex
\Caesar[〈区切り文字数（オプション）〉]{〈鍵〉}{〈平文〉}
```

* 区切り文字数（整数値）  
指定した文字数ごとに空白が入ります．何も指定しない場合，平文と同じ位置に空白が出力されます．また，負の値を指定すると一切空白を出力しません．
* 鍵（整数値）  
シーザー暗号の鍵です．整数であれば正負を問わず使用可能ですが，あまり絶対値の大きすぎる値を指定すると TeX がパンクします．
* 平文（任意の文字トークン列）  
暗号化対象の文字列を入力します．アルファベット・空白以外の文字はそのまま出力されます．

復号化を行う際は，\DecodeCaesar コマンドを使用します．

```tex
\DecodeCaesar[〈区切り文字数（オプション）〉]{〈鍵〉}{〈暗号文〉}
```

各引数の扱いは \Caesar と同様です．鍵は自動的に復号用に変換されるので，暗号化に用いた数値をそのまま指定します．

### アフィン暗号

暗号化を行う際には，\Affine コマンドを使用します．

```tex
\Affine[〈区切り文字数（オプション）〉]{〈鍵A〉}{〈鍵B〉}{〈平文〉}
```

* 鍵A（整数値）  
アフィン暗号の1つ目の鍵です．扱いはシーザー暗号の鍵と同様です．
* 鍵B（整数値：1,3,5,7,9,11,15,17,19,21,23,25）  
アフィン暗号の2つ目の鍵です．上記以外の数値を指定してもエラーにはなりませんが，復号化不能となります．

その他の引数の扱いは \Ceasar と同様です．

復号化を行う際は，\DecodeCaesar コマンドを使用します．

```tex
\DecodeAffine[〈区切り文字数（オプション）〉]{〈鍵A〉}{〈鍵B〉}{〈暗号文〉}
```

各引数の扱いは \Affine と同様です．鍵は自動的に復号用に変換されるので，暗号化に用いた数値をそのまま指定します．

### バーナム暗号

暗号化を行う際には，\Vernam コマンドを使用します．

```tex
\Vernam[〈区切り文字数（オプション）〉]{〈鍵〉}{〈平文〉}
```

* 鍵（ASCIIコード表に含まれる文字の列）  
バーナム暗号の鍵です．平文以上の長さである必要があります．なお，現時点では TeX で特殊な役割をもつほとんどの記号はエスケープしないと使用できません（空白はエスケープ不要で使用できます）．
* 平文（ASCIIコード表に含まれる文字の列）  
暗号化対象の文字列を入力します．なお，現時点では TeX で特殊な役割をもつほとんどの記号はエスケープしないと使用できません（空白はエスケープ不要で使用できます）．

復号化用に \DecodeVernam が用意されていますが，バーナム暗号は暗号化と復号化の処理方法がまったく同一なので，使用法と動作は \Vernam と同じです．なお「暗号化されていた空白」は空白記号 (␣) で出力され（環境によっては他の記号が出力されるかもしれません）「区切り文字数を指定したことによって出力される空白」は通常の空白として出力されます．

また，\Vernam コマンドでは TeX のソースで扱いにくい文字が出力されることがままあるので，暗号化結果のASCIIコード（2進数）をそのまま出力するための命令も用意されています．こちらも使い方は \Vernam コマンドと同様です．

ASCIIコード（2進数）で表された暗号文を復号化する際は \DecodeBinVernam が利用できます．

```tex
\DecodeBinVernam[〈区切り文字数（オプション）〉]{〈鍵〉}{〈暗号文〉}
```

* 鍵（ASCIIコード表に含まれる文字の列）  
\Vernam コマンドの鍵と同様です．
* 暗号文（ASCIIコード列）  
復号化対象となるASCIIコード（2進数）の列です．現時点では，間に空白を挟むとエラーになるので注意してください．

## 注意事項

実装上の問題で，他の命令の引数内で使うと空白の処理がおかしくなります．バーナム暗号以外のコマンドに関しては，オプション引数で区切り文字数を指定すれば意図通りの結果が得られる場合があります（バーナム暗号）．ただし（いずれの命令も完全展開可能では**ない**ため）そもそも引数内で使用するとエラーになるケースも多いです．

## 今後の課題

* 空白文字のカテゴリーコードをマクロ内部で変換（ただし完全展開可能である必要性あり）
* \DecodeBinVernum の第2引数中の空白を許容（無視するだけでよい）
* 内部処理のサブルーチン化を進め，パッケージの拡張を容易にする