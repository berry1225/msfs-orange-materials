# msfs-orange-materials

北日本の滑走路は、降雪時の視認性向上を目的として白ではなくオレンジ色になっています。
しかし、MSFS SDKの機能では、これらのマーキングをオレンジ色にする手段がありません。(v0.13.0時点)

なので「マーキングを置く形で実装しよう！」というのがこのファイルの目的です。

それに合わせて線の色なども揃うように変更した方が見栄えがよくなるので、使用したファイルを一式置いておきます。


## 利用条件

商用利用不可。
無料公開の作品に対しては、どのように加工してお使いいただいても構いません。
ただし画像ファイルの二次配布については、このファイル作者の許可のないものは原則的に禁止です。


## 諸注意

**遠目から見るとフチが白っぽくなるのは仕様です。**
たぶん描画上の仕様だと思われます。近くから見ると(多くのマテリアルは)ちゃんと表示されますし、気にしすぎはよくありません。


## 各ファイル説明

| ファイル名              | 目的         |
|------------------------|--------------|
| orange.png             | 自由変形用   |
| orange_h.png           | ヘリパッド用 |
| orange_line.png        | 線用         |
| orange_num_*.png       | 滑走路端用   |
| orange_o.png           | 吹き流し用   |
| orange_o_*.png         | 汎用(丸)     |
| orange_small_*.png     | エプロン用   |
| orange_tofu_[1-3].png  | 滑走路用     |
| orange_tofu_e_*.png    | 滑走路端用   |
| orange_tofu_w.png      | 着陸帯用     |
| orange_v.png           | 矢印の先端用 |
| orange_x.png           | 使用停止中用 |
| red_tofu_*.png         | HOLD用素材   |

色を変えたい場合は、PtohoshopなりGIMPなりで頑張ってください。
個人的にはCLIP STUDIO PAINTで「線の色を描画色に変更」が一番ラクです。保存して閉じるところまでオートアクションにすると更にラク。


## 引用ファイル 

* GD-AirportRunway Font by pumpCurry
* * http://www.hogera.com/pcb/font/

---

# 実際の作成方法
 
## マテリアル作業の下準備

* "PackageSource" の中にフォルダを＜任意の名称＞で作成。
* 作成したフォルダの中に "Textures" フォルダを作成。
* `PackageSources\＜任意の名称＞\Textures` の中にPNG画像を入れる。
* "PackageDefinitions"内のxmlファイルに以下を追記

```
<AssetGroup Name="＜任意の名称＞">
	<Type>MaterialLib</Type>
	<Flags>
		<FSXCompatibility>false</FSXCompatibility>
	</Flags>
	<AssetDir>PackageSources\＜任意の名称＞\</AssetDir>
	<OutputDir>MaterialLibs\＜任意の名称＞\</OutputDir>
</AssetGroup>
```

＜任意の名称＞のところは、それぞれ同じ文字列が入っていれば何でもいいと思います。アルファベットで。(例："berry_materials"とか)

下準備はここまでです。

## マテリアル登録

* 空港データ読み込み → Material Editor → File → New
* *  Name：任意の名称 (ファイル名と同じだと管理がラク)
* *  Surface：PAINT (なんでもよい？)

マテリアル登録した状態で配布してもよかったんですが、UUIDなどが競合して予期せぬ不整合が出たら嫌なので、各自で登録する形式にしています。

## マテリアル配置 

* 1. Scenery Editor → Objects → ApronをSquareで配置(Add)
* 2. Material Editor → 配置したいマテリアルを選択
* 3. Scenery Editor → さっき配置したオブジェクトを選択
* 4. Properties → Surfaceをクリックすると、選択したマテリアルが登録されるはず

## 線の色の変更

* Material Editor → 線用のマテリアルを選択
* Scenery Editor → 色変更したい"PaintedLine"を選択
* Properties → Optional Material 「...」

種類はそれぞれ以下です。
* 中心線などの一本線：DEFAULT (中心線の実装方法は後述)
* 誘導路両端などの二本線：EDGE_LINE_SOLID 系
* 滑走路周辺などの太い線：WIDE_YELLOW
* 滑走路手前の凸凹：HOLD_SHORT 系 (MARKEDは無しにする、赤い四角の実装方法は後述)
* HOLD手前の点線っぽいやつ：ENHANCED_CENTER

滑走路上のマーキングなどで、不要になったものは非表示にしておきましょう。

## 中心線の色の変更

中心線はうまく置換できませんでした。なので、中心線を描画する方法は以下のようにする必要があります。
途中までは「線の色の変更」と同じです。

* Scenery Editor → 中心線を引きたい部分に"PaintedLine"を配置してマテリアル指定
* 線が二重になっていることを確認して、被っている"TaxiPath"を選択
* Properties → Center lineをオフにしての中心線を無効にする
* 線の重複が無くなったことを確認したら、

"Center line lighted"との兼ね合いで、線とパスはほぼ同じ場所に配置しておくことをオススメします。
オブジェクトを選択するときにややこしいですが。

## 追加で線のタイプを作りたい

線用のマテリアルは、以下の公式ファイルが参考になります。
(DDS形式のファイルは、PhotoshopなりGIMPなりで頑張れば読み込めます)
```
(Cとか:\Users\＜ユーザー＞\AppData\Local\Packages\Microsoft.FlightSimulator＜数字とか＞\LocalCache\Packages\Official\OneStore\fs-base-material-lib\MaterialLibs\Base_MaterialLib\Textures\ASO_TAXIWAY_MARKS.TIF.DDS)
```

線用のファイルは種類ごとにそれぞれエリアが決まっているようです。
本ファイルに付属する線用マテリアルは、基本的にはHOLD用とENHANCED_CENTER用のところしか書いていません。

あと、上記ファイルは不透明度が低めなので、本来はそのくらいの不透明度にした方がいいのかもと思いました。
本ファイルに付属するマテリアルは基本的にすべてOpacity255相当です。
Apronはそれ自体にOpacityを設定できますが、線は今のところできないようでした。

## 赤い四角

滑走路手前のHOLDのところに赤い四角がありますよね。

あれは、MSFS的には"PaintedLine"の種類を"HOLD_SHORT_*_MARKED"系にすると自動で生成してくれるんですが、
**残念ながらマテリアルを指定してしまうとマテリアルと一体になってしまい、赤く表示されなくなります。**

表示されなくなるなら、自分で作るしかありませんね。

というわけで `red_tofu_*.png` を必要分コピーして、その上にPhotoshopなりGIMPなりを使って自分で文字を配置してください。以下のフォントあたりが字体的に近いような気がしました。
* Pathway Gothic One
* Big Shoulders Display ←3の字体だけは微妙

ファイルが完成したら、左右反転してから保存します。
**マテリアルは基本的に左右反転した状態で保存してください。**
何故かはよく分かりませんがそういう仕様です。




## 参考資料

Material Editorに関するSDKのドキュメント
https://docs.flightsimulator.com/html/Developer_Mode/Material_Editor/The_Material_Editor.htm


## 参考動画

~~動画じゃなくてテキストベースで寄越せと何度~~

SDK Tutorial #7 - Custom textures and markings
https://www.youtube.com/watch?v=yI5iZCrkjVc

SDK Tutorial #8 - Custom painted lines
https://www.youtube.com/watch?v=bz5VfVs17xo




