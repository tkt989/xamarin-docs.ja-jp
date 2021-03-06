---
title: Android SDK の場所はどこで設定できますか
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 11/16/2017
ms.openlocfilehash: 8685be4bb1cc45ff04dc8d9f7d8e64e7b1483b60
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027026"
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>Android SDK の場所はどこで設定できますか

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio で、**ツール > オプション > Xamarin > Android の設定**の順に移動し、Android SDK の場所を表示して設定します。

[![設定の [場所] タブの例](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

各パスの既定の場所は次のとおりです。

- Java Development Kit の場所: 

    **C:\\Program Files\\Java\\jdk 1.8.0 以降1**

- Android SDK の場所: 

    **C:\\Program Files (x86)\\Android\\android-sdk**

- Android NDK の場所: 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\android-ndk-r13b**

NDK のバージョン番号は異なる場合があることに注意してください。 たとえば、 **r13b**の代わりに、 **r10e**などの以前のバージョンである可能性があります。

Android SDK の場所を設定するには、Android SDK ディレクトリの完全なパスを **[Android SDK の場所]** ボックスに入力します。 エクスプローラーで Android SDK の場所に移動し、アドレスバーからパスをコピーして、 **[Android SDK の場所]** ボックスにこのパスを貼り付けます。
たとえば、Android SDK の場所が**C:\\ユーザー\\ユーザー名\\AppData\\Local\\Android\\SDK**の場合は、 **[Android SDK 場所]** ボックスで古いパスをクリアし、このパスを貼り付けます。 をクリックし、[ **OK]** をクリックします。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

Visual Studio for Mac で、 **Android > > SDK の場所に > プロジェクトの [基本設定**] に移動します。 **[Android]** ページで、 **[場所]** タブをクリックして、SDK の場所を表示および設定します。

[![設定の [場所] タブの例](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

各パスの既定の場所は次のとおりです。

- Android SDK の場所: 

    **~/Library/Developer/Xamarin/android-sdk-macosx**

- Android NDK の場所: 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- Java SDK (JDK) の場所: 

    **/usr**

NDK のバージョン番号は異なる場合があることに注意してください。 たとえば、 **r14b**の代わりに、 **r10e**などの以前のバージョンである可能性があります。

Android SDK の場所を設定するには、Android SDK ディレクトリの完全なパスを **[Android SDK の場所]** ボックスに入力します。 ファインダーで Android SDK フォルダーを選択し、 **CTRL&#8984;+ + I**キーを押してフォルダー情報を表示し、**場所**の右側のパスをクリックしてドラッグし、 **[場所]** タブの **[Android SDK の場所]** ボックスに貼り付けます。たとえば、Android SDK の場所が **~/Library/Developer/Android/Sdk**の場合は、 **[Android SDK の場所]** ボックスで古いパスをクリアし、このパスを貼り付けて、[ **OK]** をクリックします。

-----
