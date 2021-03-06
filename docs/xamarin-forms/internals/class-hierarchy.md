---
title: Xamarin. Forms Controls クラスの階層構造
description: 開発者は、Xamarin. フォームアプリケーションのユーザーインターフェイスの作成に使用される型の階層について理解している必要があります。
ms.prod: xamarin
ms.assetid: C89E6B98-464D-4BBE-BF11-13A5FCBBF420
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/07/2020
ms.openlocfilehash: 7c55ed3082a031352a8eefb8ef579060a9dd6404
ms.sourcegitcommit: 4899cf4aaa73aa56c48f7ee2339c0af8112e1feb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75717303"
---
# <a name="xamarinforms-controls-class-hierarchy"></a>Xamarin. Forms Controls クラスの階層構造

Xamarin は、複数の名前空間で、数百の型で構成されています。 開発者は、`Xamarin.Forms` 名前空間に存在する Xamarin. フォームアプリケーションのユーザーインターフェイスを作成するために使用される型の階層について最もよく理解している必要があります。

これらの型は、ページ、レイアウト、ビュー、およびセルに分割できます。 通常、Xamarin のフォームページは画面全体を占め、すべてのページの種類は[`Page`](xref:Xamarin.Forms.Page)クラスから派生します。 通常、ページにはレイアウトが含まれ、すべてのレイアウトの種類は[`Layout`](xref:Xamarin.Forms.Layout)クラスから派生します。 通常、レイアウトにはビューとその他のレイアウトが含まれ、すべてのビューの種類は最終的に[`View`](xref:Xamarin.Forms.View)クラスから派生します。 最後に、セルは、 [`TableView`](xref:Xamarin.Forms.TableView)および[`ListView`](xref:Xamarin.Forms.ListView)コントロールの表示データで使用される特殊なコントロールです。 ページ、レイアウト、ビュー、セルはすべて、最終的には[`Element`](xref:Xamarin.Forms.Element)クラスから派生します。

次のクラス図は、Xamarin でユーザーインターフェイスを構築するために通常使用される型の階層を示しています。

[![Xamarin. Forms Controls クラスダイアグラム](class-hierarchy-images/class-diagram.png "Xamarin. Forms controls クラスダイアグラム")](class-hierarchy-images/class-diagram-large.png#lightbox "Xamarin. Forms controls クラスダイアグラム")

> [!NOTE]
> クラスダイアグラムの高解像度バージョンは、[こちら](class-hierarchy-images/class-diagram-high-resolution.png)からダウンロードできます。 ただし、この図にはシェルの種類が1つしか表示されないことに注意してください。

## <a name="related-links"></a>関連リンク

- [Xamarin. フォームコントロールのリファレンス](~/xamarin-forms/user-interface/controls/index.md)
