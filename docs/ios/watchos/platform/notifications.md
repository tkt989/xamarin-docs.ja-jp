---
title: Xamarin での watchOS 通知
description: このドキュメントでは、Xamarin で watchOS 通知を使用する方法について説明します。 ここでは、通知コントローラーの作成、通知の生成、および通知のテストについて説明します。
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 6be46d31ac2c16d02749519907d650588dbbcbe6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028227"
---
# <a name="watchos-notifications-in-xamarin"></a>Xamarin での watchOS 通知

Watch アプリは、含まれている iOS アプリでサポートされている場合、通知を受け取ることができます。 通知の動作と外観をカスタマイズする場合は、次に示す追加の通知のサポートを追加する*必要*はありませんが、通知の処理が組み込まれています。

ソリューションの iOS アプリに通知サポートを追加する方法の詳細については、 [ios の通知](~/ios/platform/user-notifications/deprecated/index.md)ドキュメントを参照してください。

## <a name="creating-notification-controllers"></a>通知コントローラーの作成

ストーリーボードの通知コントローラーには、特別な種類のセグエがトリガーされます。 新しい**Notification Interface コントローラー**をストーリーボードにドラッグすると、自動的にセグエが添付されます。

![](notifications-images/notification-storyboard1.png "A new Notification Interface Controller with a segue attached")

Notification セグエを選択すると、そのプロパティを編集できます。

![](notifications-images/notification-storyboard2.png "The notification segue selected")

コントローラーをカスタマイズした後、WatchKitCatalog から次の例のようになります。

![](notifications-images/notifications-segue.png "The Notification Properties")

通知には、次の2種類があります。

- システムによって定義されて**いる、スクロール不可能な静的**ビューです。

- **長い外観**で、カスタマイズ可能なビューが定義されています。 より単純な静的バージョンとより複雑な動的バージョンを指定できます。

### <a name="short-look-notification-controller"></a>短い通知コントローラー

ショート表示 UI は、アプリアイコン、アプリ名、通知タイトル文字列で構成されています。

ユーザーが通知を無視しない場合、システムは詳細情報を提供する長い表示通知に自動的に切り替わります。

### <a name="long-look-notification-controller"></a>長い外観の Notification Controller

OS は、さまざまな要因に基づいて静的ビューと動的ビューのどちらを表示するかを決定します。 静的インターフェイスを指定する必要があります。また、必要に応じて、通知の動的インターフェイスを含めることもできます。

#### <a name="static"></a>スタティック

静的ビューは単純で、簡単に表示できます。

![](notifications-images/notification-static.png "The static view")

#### <a name="dynamic"></a>動的

動的ビューでは、より多くのデータを表示し、対話性を高めることができます。

![](notifications-images/notification-dynamic.png "The dynamic view")

## <a name="generating-notifications"></a>通知の生成

通知は、リモートサーバー ([Apple Push Notification Service](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html)または APNS) から取得することも、iOS アプリでローカルに生成することもできます。

ローカル通知の生成方法の例については、 [iOS の通知](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md)に関するチュートリアルを参照してください。実際の例については、 [WatchNotifications サンプル](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications)を参照してください。

ローカル通知では、`AlertTitle` が Apple Watch に表示されるように設定されている必要があります。 `AlertTitle` 文字列は、ショート表示インターフェイスに表示されます。 `AlertTitle` と `AlertBody` の両方が [通知] の一覧に表示されます。また、`AlertBody` が長い外観のインターフェイスに表示されます。

このスクリーンショットは、[通知] の一覧に表示されている `AlertTitle` と、([サンプルコード](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications)を使用して) 長い外観のインターフェイスに表示されている `AlertBody` を示しています。

![](notifications-images/watch-notificationslist-sml.png "このスクリーンショットは、通知の一覧に表示されている AlertTitle を示しています。") ![](notifications-images/watch-notificationcontroller-sml.png "長い外観のインターフェイスに表示される AlertBody")

## <a name="testing-notifications"></a>テスト (通知を)

通知 (ローカルとリモートの両方) は、デバイスでのみ適切にテストできます。ただし、iOS シミュレーターでは、 **json**ファイルを使用してシミュレートできます。

### <a name="testing-on-apple-watch"></a>Apple Watch でのテスト

Apple Watch で通知をテストするときは、 [Apple のドキュメント](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)に次のような内容が記載されていることに注意してください。

> アプリのローカル通知またはリモート通知の1つがユーザーの iPhone に到着すると、iOS は iPhone または Apple Watch でその通知を表示するかどうかを決定します。

これは、iOS によって、alluding に通知を表示するかどうかを決定するという事実です。 通知を受信したときに、ペアになっている iPhone がアクティブになっている場合、通知は iPhone に表示される可能性があり、ウォッチにはルーティングされ*ません*。

監視に通知が表示されるようにするには、iPhone 画面をオフにします (電源ボタンを1回押す) か、スリープ状態にします。 ペアになっている Watch が範囲内にあり、パワーを持ち、手首に装着されていない場合、通知がそこにルーティングされ、ウォッチに表示されます (これについては、微妙になります)。

### <a name="testing-on-the-ios-simulator"></a>IOS シミュレーターでのテスト

IOS シミュレーターで通知モードをテストするときは、テスト用の JSON ペイロードを指定する*必要があり*ます。 Visual Studio for Mac の **[カスタム実行引数]** ウィンドウでパスを設定します。

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

[ウォッチ] 拡張が**スタートアッププロジェクト**として設定されている場合、Visual Studio for Mac には追加のオプションが表示されます。
Watch extension プロジェクトを右クリックし、[ **> のカスタムパラメーターを使用して実行**する] を選択します。

[![](notifications-images/runwith-customparams-sml.png "Running with Custom Properties")](notifications-images/runwith-customparams.png#lightbox)

**[実行引数]** ウィンドウが開き、 **[WatchKit]** タブが表示されます。 **[通知]** を選択し、JSON ペイロードを指定してから、 **[実行]** を押してシミュレーターで watch アプリを起動します。

[![](notifications-images/runwith-execargs-sml.png "Select Notification Payload Default")](notifications-images/runwith-execargs.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio でテスト通知ペイロードを設定するには、ウォッチ拡張機能を右クリックして**プロジェクトのプロパティ**を編集します。 **[デバッグ]** セクションにアクセスし、一覧から通知 json ファイルを選択します (プロジェクトに含まれるすべての json ファイルが自動的に一覧表示されます)。

[![](notifications-images/runwith-execargs-sml-vs.png "Select a notifications JSON file")](notifications-images/runwith-execargs-vs.png#lightbox)

ウォッチ拡張機能が**スタートアッププロジェクト**の場合、Visual Studio では次のように追加のオプションが表示されます。 **通知オプションの**1 つを選択して、**通知**モードでウォッチアプリを起動します ([プロパティ] ウィンドウで選択した JSON ファイルを使用します)。

![](notifications-images/runwith-vs.png "The Device menu")

-----

既定の通知コントローラーは、シミュレーターで既定のペイロード JSON ファイルをテストするときに、次のようになります。

![](notifications-images/notification-debug-sml.png "An example notification")

また、[コマンドライン](~/ios/watchos/troubleshooting.md#command_line)を使用して iOS シミュレーターを起動することもできます。

### <a name="example-notification-payload"></a>通知ペイロードの例

[Watch Kit カタログ](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)サンプルには、ペイロード Json ファイル**notificationpayload. json**の例があります (以下を参照)。

```json
{
    "aps": {
        "alert": "Test message content",
        "title": "Optional title",
        "category": "myCategory"
        },

        "WatchKit Simulator Actions": [
        {
            "title": "First Button",
            "identifier": "firstButtonAction"
        }
        ],

        "customKey": "Use this file to define a testing payload for your notifications. The aps dictionary specifies the category, alert text and title. The WatchKit Simulator Actions array can provide info for one or more action buttons in addition to the standard Dismiss button. Any other top level keys are custom payload. If you have multiple such JSON files in your project, you'll be able to choose between them in when selecting to debug the notification interface of your Watch App."
    }
```

## <a name="related-links"></a>関連リンク

- [WatchNotifications (ローカル通知) (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchkit-watchnotifications)
- [WatchKitCatalog (サンプル)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Apple の Watch Kit の通知に関するドキュメント](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
