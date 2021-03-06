---
title: iOS Designer でのカスタム コントロールの使用
description: このドキュメントでは、カスタムコントロールを作成し、Xamarin Designer for iOS と共に使用する方法について説明します。 IOS デザイナーのツールボックスでコントロールを使用できるようにする方法、コントロールを実装して、正しくレンダリングされるようにする方法などについて説明します。
ms.prod: xamarin
ms.assetid: 9032B32E-97BD-4DA6-9955-811B84682578
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 4b8c72da1e280b83e215bca9316bc0b9de99402c
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73003799"
---
# <a name="using-custom-controls-with-the-ios-designer"></a>iOS Designer でのカスタム コントロールの使用

## <a name="requirements"></a>［要件］

Xamarin Designer for iOS は Visual Studio for Mac と Visual Studio 2017 以降の Windows で使用できます。

このガイドでは、[はじめにガイド](~/ios/get-started/index.md)で説明されている内容を理解していることを前提としています。

## <a name="walkthrough"></a>チュートリアル

> [!IMPORTANT]
> Xamarin. Studio 5.5 以降では、カスタムコントロールの作成方法は、以前のバージョンと少し異なります。 カスタムコントロールを作成するには、(関連する実装メソッドを使用して) `IComponent` インターフェイスが必要であるか、または `[DesignTimeVisible(true)]`でクラスに注釈を付けることができます。 後者の方法は、次のチュートリアルの例で使用されています。

1. IOS > アプリから新しいソリューションを作成し **> 単一ビューアプリケーション > C#** テンプレートを作成し、`ScratchTicket`という名前を指定して、新しいプロジェクトウィザードを続行します。

    [![](ios-designable-controls-walkthrough-images/01new.png "Create a new solution")](ios-designable-controls-walkthrough-images/01new.png#lightbox)

1. `ScratchTicketView`という名前の新しい空のクラスファイルを作成します。

    [![](ios-designable-controls-walkthrough-images/02new.png "Create a new ScratchTicketView class")](ios-designable-controls-walkthrough-images/02new.png#lightbox)

1. `ScratchTicketView` クラスに次のコードを追加します。

    ```csharp
    using System;
    using System.ComponentModel;
    using CoreGraphics;
    using Foundation;
    using UIKit;

    namespace ScratchTicket
    {
        [Register("ScratchTicketView"), DesignTimeVisible(true)]
        public class ScratchTicketView : UIView
        {
            CGPath path;
            CGPoint initialPoint;
            CGPoint latestPoint;
            bool startNewPath = false;
            UIImage image;

            [Export("Image"), Browsable(true)]
            public UIImage Image
            {
                get { return image; }
                set
                {
                    image = value;
                    SetNeedsDisplay();
                }
            }

            public ScratchTicketView(IntPtr p)
                : base(p)
            {
                Initialize();
            }

            public ScratchTicketView()
            {
                Initialize();
            }

            void Initialize()
            {
                initialPoint = CGPoint.Empty;
                latestPoint = CGPoint.Empty;
                BackgroundColor = UIColor.Clear;
                Opaque = false;
                path = new CGPath();
                SetNeedsDisplay();
            }

            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);

                var touch = touches.AnyObject as UITouch;

                if (touch != null)
                {
                    initialPoint = touch.LocationInView(this);
                }
            }

            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);

                var touch = touches.AnyObject as UITouch;

                if (touch != null)
                {
                    latestPoint = touch.LocationInView(this);
                    SetNeedsDisplay();
                }
            }

            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                startNewPath = true;
            }

            public override void Draw(CGRect rect)
            {
                base.Draw(rect);

                using (var g = UIGraphics.GetCurrentContext())
                {
                    if (image != null)
                        g.SetFillColor((UIColor.FromPatternImage(image).CGColor));
                    else
                        g.SetFillColor(UIColor.LightGray.CGColor);
                    g.FillRect(rect);

                    if (!initialPoint.IsEmpty)
                    {
                        g.SetLineWidth(20);
                        g.SetBlendMode(CGBlendMode.Clear);
                        UIColor.Clear.SetColor();

                        if (path.IsEmpty || startNewPath)
                        {
                            path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                            startNewPath = false;
                        }
                        else
                        {
                            path.AddLineToPoint(latestPoint);
                        }

                        g.SetLineCap(CGLineCap.Round);
                        g.AddPath(path);
                        g.DrawPath(CGPathDrawingMode.Stroke);
                    }
                }
            }
        }
    }
    ```

1. `FillTexture.png`、`FillTexture2.png` および `Monkey.png` ファイル ( [GitHub から](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)入手可能) を**Resources**フォルダーに追加します。

1. `Main.storyboard` ファイルをダブルクリックして、デザイナーで開きます。

    [![](ios-designable-controls-walkthrough-images/03new.png "The iOS Designer")](ios-designable-controls-walkthrough-images/03new.png#lightbox)

1. **[ツールボックス]** から**イメージビュー**をストーリーボードのビューにドラッグアンドドロップします。

    [![](ios-designable-controls-walkthrough-images/04new.png "An Image View added to the layout")](ios-designable-controls-walkthrough-images/04new.png#lightbox)

1. **イメージビュー**を選択し、 **image**プロパティを `Monkey.png`に変更します。

    [![](ios-designable-controls-walkthrough-images/05new.png "Setting Image View Image property to Monkey.png")](ios-designable-controls-walkthrough-images/05new.png#lightbox)

1. サイズクラスを使用しているため、このイメージビューを制限する必要があります。 イメージを2回クリックして、制約モードにします。 中央にピン留めするハンドルをクリックし、垂直方向と水平方向に並べて配置します。

    [![](ios-designable-controls-walkthrough-images/06new.png "Centering the image")](ios-designable-controls-walkthrough-images/06new.png#lightbox)

1. 高さと幅を制限するには、サイズ固定ハンドル ("ボーン" の形のハンドル) をクリックし、[幅] と [高さ] をそれぞれ選択します。

    [![](ios-designable-controls-walkthrough-images/07new.png "Adding Constraints")](ios-designable-controls-walkthrough-images/07new.png#lightbox)

1. ツールバーの [更新] ボタンをクリックして、制約に基づいてフレームを更新します。

    [![](ios-designable-controls-walkthrough-images/08new.png "The Constraints toolbar")](ios-designable-controls-walkthrough-images/08new.png#lightbox)

1. 次に、プロジェクトをビルドして、ツールボックス の **カスタムコンポーネント** の下に **スクラッチチケット ビュー**が表示されるようにします。

    [![](ios-designable-controls-walkthrough-images/09new.png "The Custom Components toolbox")](ios-designable-controls-walkthrough-images/09new.png#lightbox)

1. **スクラッチチケットビュー**をドラッグアンドドロップして、サル画像の上に表示されるようにします。 次に示すように、スクラッチチケットビューがサルを完全にカバーするように、ドラッグハンドルを調整します。

    [![](ios-designable-controls-walkthrough-images/10new.png "A Scratch Ticket View over the Image View")](ios-designable-controls-walkthrough-images/10new.png#lightbox)

1. 両方のビューを選択するには、外接する四角形を描画して、スクラッチチケットビューをイメージビューに制限します。 次に示すように、制約に基づいて、幅、高さ、中心、中央に制限するオプションを選択し、制約に基づいてフレームを更新します。

    [![](ios-designable-controls-walkthrough-images/11new.png "Centering and adding Constraints")](ios-designable-controls-walkthrough-images/11new.png#lightbox)

1. アプリケーションを実行し、イメージを "ゼロオフ" にして、サルを表示します。

    [![](ios-designable-controls-walkthrough-images/10-app.png "A sample app run")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="adding-design-time-properties"></a>デザイン時プロパティの追加

このデザイナーには、numeric、enumeration、string、bool、CGSize、UIColor、および UIImage の各プロパティ型のカスタムコントロールのデザイン時サポートも含まれています。 例として、プロパティを `ScratchTicketView` に追加して、"傷がある" イメージを設定してみましょう。

プロパティの `ScratchTicketView` クラスに次のコードを追加します。

```csharp
[Export("Image"), Browsable(true)]
public UIImage Image
{
    get { return image; }
    set {
            image = value;
              SetNeedsDisplay ();
        }
}
```

次のように、`Draw` メソッドに null チェックを追加することもできます。

```csharp
public override void Draw(CGRect rect)
{
    base.Draw(rect);

    using (var g = UIGraphics.GetCurrentContext())
    {
        if (image != null)
            g.SetFillColor ((UIColor.FromPatternImage (image).CGColor));
        else
            g.SetFillColor (UIColor.LightGray.CGColor);

        g.FillRect(rect);

        if (!initialPoint.IsEmpty)
        {
             g.SetLineWidth(20);
             g.SetBlendMode(CGBlendMode.Clear);
             UIColor.Clear.SetColor();

             if (path.IsEmpty || startNewPath)
             {
                 path.AddLines(new CGPoint[] { initialPoint, latestPoint });
                 startNewPath = false;
             }
             else
             {
                 path.AddLineToPoint(latestPoint);
             }

             g.SetLineCap(CGLineCap.Round);
             g.AddPath(path);
             g.DrawPath(CGPathDrawingMode.Stroke);
        }
    }
}
```

`ExportAttribute` と引数が `true` に設定された `BrowsableAttribute` を含めると、デザイナーの **[プロパティ]** パネルにプロパティが表示されます。 次に示すように、プロパティをプロジェクトに含まれる別のイメージ (`FillTexture2.png`など) に変更すると、デザイン時にコントロールが更新されます。

 [![](ios-designable-controls-walkthrough-images/11-customproperty.png "Editing Design Time properties")](ios-designable-controls-walkthrough-images/10-app.png#lightbox)

## <a name="summary"></a>まとめ

この記事では、iOS デザイナーを使用して、カスタムコントロールを作成し、iOS アプリケーションで使用する方法について説明します。 デザイナーの**ツールボックス**で、コントロールを作成してビルドし、アプリケーションで使用できるようにする方法を説明しました。 また、デザイン時と実行時の両方で適切にレンダリングされるようにコントロールを実装する方法についても説明しました。また、デザイナーでカスタムコントロールのプロパティを公開する方法についても説明しました。

## <a name="related-links"></a>関連リンク

- [ScratchTicket (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/scratchticket)
- [必要なイメージ (サンプル)](https://github.com/xamarin/ios-samples/blob/master/ScratchTicket/Resources/images.zip?raw=true)
