# StateRetainer

<img src="https://github.com/akita11/StateRetainer/blob/main/StateRetainer.jpg" width="320px">

<img src="https://github.com/akita11/StateRetainer/blob/main/StateRetainer_back.jpg" width="320px">

Grove端子のデジタル信号の変化を、マイコン等で扱いやすいように「保持」するものです。スイッチを短い時間押す、などの単発信号をとらえ、一定時間、またはリセットを書けるまで、出力にその値を保持します。

内部的は、いわゆる"555"をそのまま使っています。（参考：[555の動作](https://www.marutsu.co.jp/contents/shop/marutsu/mame/166.html)）


## ピン配置

- Grove（In側）: IN/x/VDD/GND ※INは入力（プルアップ）
- Grove（Out側）: OUT/RST/VDD/GND ※OUTは出力、RSTは入力（プルダウン）

OUT=1の時には、LEDが点灯し、出力の状態を確認できます。

※内部の回路(555)はGrove端子から供給される電源電圧で動作するため、RST入力を"1"とするには、電源電圧の2/3以上の電圧が必要です。またOUT出力の"1"の電圧も電源電圧となります。M5StackシリーズではVDDは5VですがIO電圧は3.3Vのため、RSTはぎりぎり"1"に足りません（個体差で"1"と判別される場合もあります）。またOUTの"1"は5Vのため、IOピンの入力電圧の範囲を超えます。そのため、確実な動作のためには、Grove端子の電源電圧として3.3Vを供給してください（[こちら](https://www.switch-science.com/products/9461)を使うとGroveケーブルの中継としてはさむことで3.3Vを生成して供給できます）


## 動作

目的にあわせて、以下の3通りの動作状態を設定できます。


### 保持動作（デフォルト状態）

INの状態変化を出力に保持します。いわゆるRSフリップフロップとしての動作です。なおINは内部でプルアップ、RSTはプルダウンされているので、IN、RSTの開放時には、それぞれ"1"、"0"と同等です（待機状態）。


- INが1→0になると、OUT=1となり、その状態を保持。
- その状態で、RSTを0→1とすると、OUT=0に復帰。

【構成】: C1/C2、R1/R3、R2/R4に部品を取り付けなず、JP1/JP2を開放（初期状態）


### 単発パルス発生動作

INが1→0になると、指定時間（T=1.1*CR[s]）間のみ、OUT=1となり、その後、OUT=1に復帰します。

- 【構成】: R7を除去し、C1/C2にコンデンサC、R1/R3に抵抗Rを接続し、R2/R4は抵抗を取り付けず、JP1を短絡、JP2を開放


### 発振動作

入力に関係なく、f=1.44 / (Ra + 2*Rb)C [Hz]で発振します。

- 【構成】: R6, R7を除去し、R1/R3にRa、R2/R4にRb、C1/C2にCを接続し、JP2を短絡、JP1を開放


## 出力の極性の切り替え

出力OUTはJP3によって、正論理（デフォルト、OFF時=0、ON時=1）・負論理（オープンコレクタ、OFF時=開放、ON時=0）を切り替えできます。負論理の場合は保持動作時にはR1/R3にプルアップ抵抗を取り付け可能。上記のOUTの値の表記は正論理時の値の表記です。


## Author

Junichi Akita (akita@ifdl.jp, @akita11)
