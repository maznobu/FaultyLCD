# FaultyLCD

Amazonで購入した16桁×2行タイプの不良LCDについて

## 経緯（background）

<p>2024年10月にAmazon Japanで中国製のLCDを購入しました。<br/>
価格は6,000円、16桁×2行のバックライト付きLCDが２つ入ったものを2つ買いました。
届いてすぐに実験しましたが、うまく点灯しませんでした。それでも、何とか見えますし、使っていくことにしました。<br/>そして半年ほどたった今日、若干の手直しすれば、動作することが分かったのでお知らせします。
プロジェクトと言う程の大所帯なものではないのですが、広く皆さんに知って戴きたいと思い筆を執りました。</p>
---
<p>In October 2024, I purchased a Chinese-made LCD from Amazon Japan. The price was 6,000 yen, and I bought two sets containing two 16-digit x 2-line backlit LCDs.
I tested it as soon as it arrived, but it didn't light up properly. Still, I could see it somehow, so I decided to use it. And today, about half a year later, I found that it works with a little tweaking, so I'd like to let you know.
It's not a large project, but I decided to write this article because I wanted to let everyone know about it.</p>

## どのような商品？（What kind of product?）

<p>取扱説明書も図面も何もついていない商品です。Amazonあるある、ですね。<br/>
でもパッケージにQRコードが付いていますので、製造メーカサイトを訪れプログラムサンプルをダウンロードできます。サンプルの中にはデータシートやおすすめの商品のデータシートなどが入っています。サンプルにはドライバらしきものも入っていますが商品とは全然関係ありませんでした。商品に関するものは、LCDのデータシート、I2Cインターフェイスチップのデータシートと、Arduino、C++、Pythonでのサンプルコードでした。そのうち、Pythonのものは、先ほど挙げたドライバを使用しているようですが、当該商品にはUSBはついていませんので、恐らくPythonのサンプルコードは役に立ちません。</p>
---
<p>This product does not come with an instruction manual or diagrams. That's typical of Amazon, isn't it? ... Maybe that's just in Japan?<br/>
However, there is a QR code on the package, so you can visit the manufacturer's website and download program samples. The samples include data sheets and data sheets for recommended products. The samples also include what appear to be drivers, but they are not related to the product at all. The product-related items were an LCD data sheet, an I2C interface chip data sheet, and sample code for Arduino, C++, and Python. Of these, the Python one seems to use the driver mentioned above, but since the product does not come with USB, the Python sample code is probably not useful.</p>

## 構成（composed）

<p>このLCDは、以下の構成です</p>
- HD44780にほぼ準拠しているLCDチップ（TC1602B-01という型番）
- PCF8574（シフトレジスタにI2Cインターフェイスが付いた様なもの）
<p>PCF8574のデータラインは、D0～D7について、順に、LCDのRS、RW、E、バックライト,D4,D5,D6,D7の各ラインにつながっています。PCF8574は単に書き込まれたデータをLCDに送るだけです。データ幅が8ビットしかないため、LCDを4ビットモードで制御している様です。</p>
---
<p>This LCD is composed of the following:</p>
- An LCD chip (model number TC1602B-01) that is almost compatible with HD44780
- PCF8574 (a shift register with an I2C interface)
<p>The data lines of the PCF8574, D0 to D7, are connected to the RS, RW, E, backlight, D4, D5, D6, and D7 lines of the LCD, in that order. The PCF8574 simply sends the written data to the LCD. Since the data width is only 8 bits, it appears to control the LCD in 4-bit mode.</p>


## 適正に使用するための手直し（Adjustments for proper use）

1. 電源電圧を下げる（Lower the power supply voltage）
<p>LCDモジュール（液晶コントローラとバックライト部分）の定格電圧は、データシートを見る限りでは5Vです。しかし、LCDモジュールの定格は4.2Vとなっており、5Vの電圧を加えても壊れることはありませんが動作が不安定なり、文字化けが発生したりと問題が発生します。5V電源で使用するならば、電源ラインに75～100Ωの抵抗を入れて降圧させて使用するのが良いでしょう。</p>
---
<p>The rated voltage of the LCD module (liquid crystal controller and backlight part) is 5V according to the data sheet. However, the rated voltage of the LCD module is 4.2V, and although it will not break if a voltage of 5V is applied, it will cause problems such as unstable operation and garbled characters. If you use it with a 5V power supply, it is better to use it by inserting a 75-100Ω resistor in the power line to step down the voltage.</p>

2. バックライトの電流制限抵抗を交換（Replace the backlight current limiting resistor）
<p>LCDが黒く変色する原因は、バックライトの点灯と思われます。バックライトが点灯していないときは、液晶部分を明るい光にかざすと、白く抜けていることが分かります。バックライトを点灯すると、じわじわと、でもほぼ瞬時に黒く変色するのが分かります。つまり、この現象は、バックライトの消費電力が大きいために、液晶部分での著しい電圧降下が発生し黒くなっているものと思われます。
私が手にしたLCDには、バックライトのラインに100オーム1/4Wの抵抗が入っていました。これを、220オーム1/4Wのチップ抵抗に差し替えます。抵抗の交換には、元の抵抗の取り外しと、ハンダ付けが必要になります。
</p>
---
<p>The reason why the LCD turns black is probably because the backlight is on. When the backlight is not on, you can see that the LCD part is white when you hold it up to a bright light. When the backlight is on, you can see that it turns black gradually but almost instantly. In other words, this phenomenon is probably caused by the large power consumption of the backlight, which causes a significant voltage drop in the LCD part, causing it to turn black.
The LCD I got had a 100 ohm 1/4W resistor in the backlight line. Replace this with a 220 ohm 1/4W chip resistor. To replace the resistor, you will need to remove the original resistor and solder it.
</p>

## 最後に（Finally）

<p>これですべての手順は終わりです。<br/>
如何だったでしょうか？<br/>
私はこのLCDをRaspberryPiに被せて使う予定です。<br/>
後で、LCDを点灯させるために作ったPythonのコードも載せておきますね。<br/>
では、また！</p>
---
<p>This completes all the steps.<br/>
What do you think?<br/>
I plan to use this LCD by placing it over a RaspberryPi.<br/>
Later, I will post the Python code I wrote to light up the LCD.<br/>
<br/>
See you next time!</p>
