# ミニ電光掲示板プログラミング体験
## 2025年6月28日

## 宇部高専 制御情報工学科
教員：江原 史郎・田辺 誠  
学生（4年生）



---

## 資料

- [今回の実習資料](https://github.com/cslab-nituc/ise-taiken2025)
![QRコード](/img/QR2.png)


- [仮想実習環境（Tinkercad）](https://www.tinkercad.com/joinclass/76STCHZQY)
- [Arduino Reference Manual](https://docs.arduino.cc/language-reference/)

<!--
![QRコード](/img/image-7.png)
-->
<div style="height: 200px;"></div>

---

## じつは <span v-click>**→コンピュータとプログラムが活躍！**</span><span v-click>（8x8のLEDで体験）</span>

<img src="/img/image-1.png" alt="マツダスタジアムの電光掲示板" style="width:80%;height:auto;">

写真は(株)広島東洋カープ提供  
技術情報はパナソニックシステムネットワークス（株）提供

---
layout: two-cols-header
---

## 8x8のLED（Matrix LEDとよびます）のしくみ

::left::
![Matrix LED](/img/image-2.png)

::right::

## 1. <span v-click>赤ボタンと黒ボタンを押す。</span>
## 2. <span v-click>赤がプラス（5V）<br/>　黒がマイナス（0V）<br/>　に接続され、<br/>　交点のLEDが点灯する。</span>

---
layout: two-cols-header
---

## やってみよう！

::left::
### 演習1: 左下の4x4のLEDを光らせよう！
<img src="/img/問題1.svg" alt="左下を光らせる" style="width:100%;height:auto;">

::right::
### 演習2: 右上の4x4のLEDを光らせよう！
<img src="/img/問題2.svg" alt="右上を光らせる" style="width:100%;height:auto;">

---
layout: two-cols-header
---

## やってみよう！

::left::
### 解答1: 左下の4x4のLEDを光らせよう！
<img src="/img/解答1.svg" alt="左下を光らせる" style="width:100%;height:auto;">

::right::
### 解答2: 右上の4x4のLEDを光らせよう！
<img src="/img/解答2.svg" alt="右上を光らせる" style="width:100%;height:auto;">

---

## やってみよう！

## 演習3: 左下と右上を光らせよう！
<img src="/img/問題3.svg" alt="右上を光らせる" style="width:60%;height:auto;">


---

## やってみよう！

## 解答3: 
<img src="/img/dynamic_led.gif" alt="ダイナミック点灯" style="width:60%;height:auto;"> 
<div style="height: 200px;"></div>


---
layout: two-cols
---

<img src="/img/tired.png" alt="ダイナミック点灯" style="width:80%;height:auto;"> 

::right::
# <span v-click> <br/>コンピュータに<br/><br/>　　まかせてしまえ<br/><br/>　　　　**ダイナミック点灯**<br/><br/></span>

---
layout: two-cols
---

<img src="/img/ミニ電光掲示板.png" alt="Arduino" style="width:70%;height:auto;"> 

1. スイッチの代わりにコンピュータを回路に接続する。
2. 「**すごい勢いで**①②を繰り返す」プログラムを作成する。  
① 「赤の左半分だけ押す」「黒の下半分だけ押す」  
② 「赤の右半分だけ押す」「黒の上半分だけ押す」  
3. プログラムをコンピュータで実行する  
→手で無茶苦茶早く押すのと同じ効果！<br/>（**ダイナミック点灯**）

::right::

```mermaid {theme: 'neutral', scale: 0.75}
flowchart
  O2["初期設定"]
  O2 --> A("loop() 開始")
  A --> B["①赤の左半分と黒の下半分を押す<br/>（左下点灯)"]
  B --> C["②赤の右半分と黒の上半分を押す<br/>（右上点灯）"]
  C --> D("loop()ここまで")
  D --> A


```
## プログラムの設計図（フローチャート）

---

## 演習4: シミュレーションしよう！

<img src="/img/image-6.png" alt="Tinkercad" style="width:80%;height:auto;"> 

1. `▶シミュレーション開始`をクリックして開始する。
2. Matrix LEDの変化を見る。
3. プログラム内の`loop()`を確認する。ただし、57行目～71行目（`/*`と`*/`の間）は実行されない
4. `シミュレーションを停止`をクリックして停止する。

---
layout: two-cols
---

## 演習5: ダイナミック点灯しよう！
57行目の`/*`と71行目の`*/`を削除して
### `▶シミュレーション開始`
しよう。

::right::
<img src="/img/image8.png" alt="Tinkercad" style="width:90%;height:auto;"> 

---
layout: two-cols-header
---

## 演習5: ダイナミック点灯しよう！

::left::
<img src="/img/image8.png" alt="Tinkercad" style="width:80%;height:auto;"> 

::right::

<img src="/img/dynamic_led.gif" alt="ダイナミック点灯" style="width:80%;height:auto;"> 

---

## まとめ：わかったこと
1. コンピュータを使って、**機械の制御**を安全かつ正確に行うことができる。
2. コンピュータへの指示を**プログラム**で行う。
3. 制御情報工学科では「**機械の制御ができる情報技術者**」を育成する！
4. 設計開発のV字プロセス（設計・実装・検証）を教えます。

---
layout: two-cols-header
---

## プログラミング（実装）

::left::
<img src="/img/V.png" alt="V" style="width:80%;height:auto;"> 
<div style="height: 200px;"></div>

::right::


```c
void motor_fwd(void)
{
	static motorcp pmc=NULL;
	if (pmc==NULL) pmc=mc();
  if (pmc->cnt!=0x18) { // 同じ場合は実行しない。
		pmc->mflag = (pmc->mflag&~3)|1;
		pmc->cnt = 0x18 ;
		PADR=pmc->cnt;
	}
	return;
}
```

---
layout: two-cols-header
---

## 設計

::left::
<img src="/img/V.png" alt="V" style="width:80%;height:auto;"> 
<div style="height: 200px;"></div>
::right::


```plantuml
@startuml
autoactivate on

title LED点灯のシーケンス図：2022.3.23 Ver1.0 (Makoto TANABE)
mainframe LED点滅（点滅モードbflag=1の場合）

participant OS
participant main
participant led


[-> OS: OS起動

OS -> main : プログラム開始
main -> OS : init_timer0(コンペアマッチ_100ミリ秒相当, NULL)
    OS -> OS : t0v.count = 0
    return
return
main -> OS : start_timer0()
return （タイマー起動）


par タイマー
group 繰返し [0.1秒間隔]
   OS -> OS : タイマー割込み
        OS -> OS : t0v.count値++
        return
    return
end

else メイン

'' 追加
main -> led : ledbflag(bflag)
note right: パラメータ設定\n_ledValue.bflag = bflag
return
'' 追加
main -> led : ledblinktime(blmsec)
note right: パラメータ設定\n_ledValue.blmsec = blmsec
return

group 繰返し [メインループ]

 main -> led : led(num)
    note right: パラメータ設定\n_ledValue.num = num
    return
    main -> led : ledexec()
    note right: 点灯・消灯切替の判断・実行
       led -> led : _ledValue.bflag
 
       return 1（点滅モード）
       led -> led : _ledValue.blmsec/2 ミリ秒経過？
         led -> OS : t0v.count値取得
         return
       return YES/NO
       alt YESかつ現在点灯
            led -> led: ledout(0)
            return (消灯)
       else YESかつ現在消灯
            led -> led: ledout(_ledvalue.num)
            return (点灯)
       else NO→何もしない
            
       end
    return (ledexec終了)
end
main -> OS : stop_timer0()
note right: プログラム再実行のために絶対必要
return (タイマー停止)
return プログラム終了

end 
return OS終了
@enduml
```

---
layout: two-cols-header
---

## テスト（検証）

::left::
<img src="/img/V.png" alt="V" style="width:80%;height:auto;"> 
<div style="height: 200px;"></div>
::right::

<img src="/img/test.png" alt="V" style="width:80%;height:auto;"> 


---

## 自宅でもできます！

- [今回の実習資料](https://github.com/cslab-nituc/ise-taiken2025)
![QRコード](/img/QR2.png)


- [シミュレータ（Tinkercad）](https://www.tinkercad.com/things/4novUPWfF1w-?sharecode=Onf_GE3mlEn0Sq_Zi_aWAtnwpw8Z4ybddtX33gzPmZc)
- [Arduino Reference Manual](https://docs.arduino.cc/language-reference/)

<!--
![QRコード](/img/image-7.png)
-->
<div style="height: 200px;"></div>