# LineSense
NHK24用ラインセンサ基板。BOMは[こちら](https://docs.google.com/spreadsheets/d/12akROPWRgP_KYzk62EP3h0m6oYP8IBUy/edit?usp=sharing&ouid=100044542046978284478&rtpof=true&sd=true)

## バージョン履歴
### 初期版(2023/9/9)
初期バージョン。
### v1.1(2023/9/9)
センサが5V仕様だったので12Vから5Vに電源を変更。それに伴う回路定数変更。基板にシルク印刷のバージョン表記を追加。初期版の基板の一部の抵抗値が変わったのと、一部回路が不要になった(部品をつけなくていい部分ができた)だけなので、5Vのセンサが欲しい人は初期版の基板がそのまま使える。

## 概要
地面の色(水性塗料でペイントされている)を判別することができるセンサ。白かそれ以外(青、緑、オレンジ、黄、赤のいずれか)かを判別する。地面の下が白色である確率に応じた振幅の正弦波(10kHz)がSIG_OUTから出力される。出力された信号を包括線検波することで反射率を取得できる。LEDを10kHzで点滅させることで光の反射率をAM変調し、光学フィルタでLEDの波長以外をカットし、Q値の高いLCフィルタで10kHzの信号のみを抽出しているため、比較的地面と離れていても実用に耐えられるよう配慮している。これは、センサの位置を高めにすることでフィールドにある傾斜の影響を極力小さくするためである。

## 基板の作り方(推奨)
部品の温度特性を考慮して、未使用時でも3カ月に1回は校正を行うこと。また、実運用時には機械的な振動で半固定抵抗がずれるため、機体を起動させる際に攻勢を行うこと。

### 1. LED点灯回路を組む
Sheet 2/2 の部品、J1(EPコネクタ)、C7(電解コンデンサ)を取り付け、LEDが点灯することを確認する。また、TP1(テストポイント)から9~12kHzの矩形波が観測されるか確認する。

### 2. 受光回路を組む
Q1, R1, R10, C5を取り付け、白い紙に反射させたLED光に反応することを確認する(10kHz付近の波が確認できるはず)。出力波形はTP2(生信号)またはTP3(カップリング済み信号)で確認できる。信号が弱い場合はR10を時計回りに回すことでゲインを上げる。

### 3. プリアンプ/フィルタ/アンプを組む
残りの部品を取り付け、白い紙に反射させたLED光に反応することを確認する。出力波形はTP5またはコネクタから取ってくる。信号が弱い場合はR5を時計回りに回すことでプリアンプのゲインを上げる。

### 4. 光学フィルタをつける
光センサ(Q1)を20mm平方の青いセロハンで覆う。

### 5. 校正を行う
以下のセクションで説明のある方法で校正を行う。

## 校正の仕方
### LEDの点滅周波数の校正
TP1にオシロスコープのプローブを当て、R16を調整しながら10kHzの矩形波が出力されるようにする。R16を時計回りに回すと周波数が上がり、反時計回りに回すと周波数が下がる。

### 受光回路のゲインの校正
周囲を出来るだけ暗くし、センサを白い紙に水平に設置する。この時の紙との距離は、機体にセンサを搭載した時の高さに合わせる。R10を調整しながらTP3から100mVpp(0.1Vpp)の10kHzの矩形波が出力されるようにする。R10を時計回りに回すと波形が大きくなり、反時計回りに回すと波形が小さくなる。

### プリアンプのゲインの校正
受光回路のゲイン校正と同様の条件に加え、SIG_OUTに36Ωの負荷抵抗を接続した状態で、R5を調整しながらTP5で出てくる信号が歪まないギリギリまでゲインを上げる。TP5からは10kHz正弦波**のみ**が出力されるので、その頂点の部分(約3.0V~3.3V程度)が歪まない(つぶれない)限界までR5を時計回りに回す。