---
title: Xamarin のスライダー、スイッチ、およびセグメント化されたコントロール
description: このドキュメントでは、Xamarin のスライド、スイッチ、およびセグメント化されたコントロールについて説明します。プログラムと iOS Designer の両方で操作する方法について説明します。
ms.prod: xamarin
ms.assetid: 85BF0EC8-E581-49CD-B9E7-98BE4C5A0F6B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 763165f1e09f847745b820987f8dbbae8f834fd7
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021950"
---
# <a name="sliders-switches-and-segmented-controls-in-xamarinios"></a>Xamarin のスライダー、スイッチ、およびセグメント化されたコントロール

<a name="Sliders" />

## <a name="sliders"></a>スライダー

スライダーコントロールを使用すると、範囲内の数値を簡単に選択できます。 コントロールの既定値は0と1の間の値ですが、これらの制限はカスタマイズできます。

 [![](slider-switch-segmented-controls-images/image25a.png "Slider")](slider-switch-segmented-controls-images/image25a.png#lightbox)

次のスクリーンショットは、デザイナーで編集できるプロパティを示しています。

 [![](slider-switch-segmented-controls-images/image26a.png "Slider Properties")](slider-switch-segmented-controls-images/image25a.png#lightbox)

次に示すように、コードでこれらの値を設定できます。これには、`UILabel` コントロールで現在選択されている値を表示するハンドラーの接続などがあります。

```csharp
slider1.MinValue = -1;
slider1.MaxValue = 2;
slider1.Value = 0.5f; // the current value
slider1.ValueChanged += (sender,e) => label1.Text = ((UISlider)sender).Value.ToString ();
```

スライダーの外観をカスタマイズするには、

```csharp
slider1.ThumbTintColor = UIColor.Blue;
slider1.MinimumTrackTintColor = UIColor.Gray;
slider1.MaximumTrackTintColor = UIColor.Green;
```

カスタマイズしたスライダーは次のようになります。

 [![](slider-switch-segmented-controls-images/image27a.png "Custom Slider")](slider-switch-segmented-controls-images/image28a.png#lightbox)

> [!IMPORTANT]
> 現在、[バグ](https://stackoverflow.com/a/19496179)が発生したため、`ThumbTint` は実行時に期待どおりにレンダリングされません。 回避策として、上記のコードの**前に**次のコード行を追加できます。 [[ソース](https://stackoverflow.com/a/21396794)]:
>
> `slider1.SetThumbImage(UIImage.FromBundle("thumb.png"),UIControlState.Normal);`
> 
> オーバーライドされるイメージを使用できますが、リソースディレクトリ_に_配置され、コード内で呼び出されることを確認してください。

<a name="Switch" />

## <a name="switch"></a>切り替え

iOS では、他のプラットフォームのラジオボタンで表すことができるブール型の入力として `UISwitch` を使用します。 ユーザーは、*つまみ*を**オン/オフ**の位置に移動することによって、コントロールを操作できます。

 [![](slider-switch-segmented-controls-images/image28a.png "Switch")](slider-switch-segmented-controls-images/image28a.png#lightbox)

スイッチの外観は、デザイナーの**Properties Pad**でカスタマイズできます。これにより、既定の状態、**オン/オフの濃淡**の色、**オン/オフイメージ**を制御できます。 これを次の図に示します。

 [![](slider-switch-segmented-controls-images/image29a.png "Switch Properties")](slider-switch-segmented-controls-images/image29a.png#lightbox)

スイッチのプロパティをコードで設定することもできます。たとえば、次のコードでは、`On`の既定値を持つスイッチが表示されます。

```csharp
switch1.On = true;
```

 <a name="Segmented_Controls" />

## <a name="segmented-controls"></a>セグメント付きコントロール

セグメント化されたコントロールは、ユーザーが少数のオプションと対話できるようにするための整理された方法です。 水平方向にレイアウトされ、各セグメントは個別のボタンとして機能します。 デザイナーを使用すると、セグメント化されたコントロールは **[ツールボックス > コントロール]** の下にあり、次の図のようになります。

 [![](slider-switch-segmented-controls-images/segmentedcontrol.png "Segmented Control")](slider-switch-segmented-controls-images/segmentedcontrol.png#lightbox)

デザイナーの固有の機能を使用すると、次に示すように、デザイン画面で各セグメントを個別に選択できます。

 [![](slider-switch-segmented-controls-images/segmentedcontrolselection.png "Segmented Control")](slider-switch-segmented-controls-images/segmentedcontrolselection.png#lightbox)

これにより、Properties Pad を使用して、各セグメントのプロパティをより厳密に制御できます。 編集可能なプロパティは、次のスクリーンショットで確認できます。

 [![](slider-switch-segmented-controls-images/segmentedcontrolproperties.png "Segmented Control")](slider-switch-segmented-controls-images/segmentedcontrolproperties.png#lightbox)

セグメント化されたコントロールスタイルは iOS7 で非推奨とされているため、iOS7 アプリケーションでこのオプションを調整しても効果はありません。

## <a name="related-links"></a>関連リンク

- [コントロール (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
- [警告コントローラー](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller)
