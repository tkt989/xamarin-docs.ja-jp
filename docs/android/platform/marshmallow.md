---
title: Marshmallow の機能
description: この記事は、android 6.0 Marshmallow 用アプリを開発するために Xamarin を使用してを使用する場合に役立ちます。
ms.prod: xamarin
ms.assetid: E4D6F183-98D2-460A-9D65-937639A899E0
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: fb1ba92be9527d490b3d34bd4c0e454b0a750837
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73019984"
---
# <a name="marshmallow-features"></a>Marshmallow の機能

_この記事は、android 6.0 Marshmallow 用アプリを開発するために Xamarin を使用してを使用する場合に役立ちます。_

この記事では、Android 6.0 Marshmallow の新機能の概要、android Marshmallow 開発用に Xamarin を準備する方法、および新しい Android Marshmallow を使用する方法を示すサンプルアプリケーションへのリンクを示します。Xamarin Android アプリの機能。 

## <a name="overview"></a>概要

Android [6.0 Marshmallow](https://developer.android.com/about/versions/marshmallow/index.html)は、android ロリポップの後の次の主要な android リリースです。
Xamarin Android は Android Marshmallow をサポートし、次のものが含まれます。

- **API 23/android 6.0 のバインド**&ndash; android 6.0 では、以下に示す新機能用に多くの新しい api が追加されています。これらの Api は、API レベル23を対象とする場合に Xamarin Android アプリで使用できます。 Android 6.0 Api の詳細については、「 [android 6.0 api](https://developer.android.com/preview/api-overview.html)」を参照してください。 

[Marshmallow を実行しているタブレットや携帯電話のヒーローイメージを![](marshmallow-images/android-m-hero-sml.png)](marshmallow-images/android-m-hero.png#lightbox)

Marshmallow リリースは主に "ポーランドと品質" に重点を置いていますが、Xamarin Android 開発者にとって興味深い多くの新機能も提供しています。 これには次の機能があります。 

- この拡張機能 &ndash;**ランタイムのアクセス許可**により、ユーザーは実行時に1回限りのセキュリティアクセス許可を承認することができます。 

- **認証の改善**&ndash; Android Marshmallow 以降では、アプリは指紋センサーを使用してユーザーを認証できるようになりました。また、新しい*資格情報の確認*機能を使用すると、パスワードを入力する必要が最小限に抑えられます。 

- アプリ**リンク**&ndash; この機能を使用すると、アプリを web ドメインに自動的に関連付けることで、**アプリの選択**をポップアップ表示する必要がなくなります。 

- **ダイレクト共有**&ndash; ユーザーに対して共有を迅速かつ直感的に行うための*ダイレクト共有ターゲット*を定義できます。この機能を使用すると、他のアプリとコンテンツを共有できます。 

- この新しい API を使用して**音声**通話を行うことで、アプリに会話音声機能を組み込むことができ &ndash; ます。 

- **4K 表示モード**&ndash; Android Marshmallow では、アプリは、それをサポートするハードウェアで4k 解像度の解像度を要求できます。 

- **新しいオーディオ機能**&ndash; Marshmallow 以降では、ANDROID は MIDI プロトコルをサポートするようになりました。 また、デジタルオーディオキャプチャオブジェクトと再生オブジェクトを作成するための新しいクラスも用意されています。また、オーディオデバイスと入力デバイスを関連付けるための新しい API フックが用意されています。 

- **新しいビデオ機能**&ndash; Marshmallow は、アプリがオーディオストリームとビデオストリームを同期してレンダリングするのに役立つ新しいクラスを提供します。このクラスは、動的な再生速度もサポートします。 

- **Android For Work** &ndash; Marshmallow には、企業所有のシングルユーザーデバイス向けの強化されたコントロールが含まれています。 デバイスの所有者によるアプリのサイレントインストールとアンインストール、システム更新の自動受け入れ、証明書管理の向上、データ使用状況の追跡、アクセス許可の管理、および作業状態の通知をサポートしています。 

- **素材デザインサポートライブラリ**&ndash; 新しい*デザインサポートライブラリ*では、デザインコンポーネントとパターンが提供されます。これにより、アプリに素材のデザインの外観と外観を簡単に組み込むことができます。 

さらに、android M では多くの主要な Android ライブラリの更新がリリースされており、android M とそれ以前のバージョンの Android では新機能が提供されています。

さらに、android Marshmallow では、多くの主要な Android ライブラリの更新がリリースされており、android Marshmallow とそれ以前のバージョンの Android の新機能が提供されています。 この記事では、Android Marshmallow でアプリの構築を開始する方法について説明します。また、Android 6.0 の新機能の概要についても説明します。 

## <a name="requirements"></a>［要件］

Xamarin ベースのアプリで新しい Android Marshmallow 機能を使用するには、次のものが必要です。 

- **Xamarin &ndash; 5.1.7.12**以降をインストールし、Visual Studio または Xamarin Studio で構成する必要があります。

- **Visual Studio for Mac**または**Visual Studio** &ndash; Visual Studio for Mac を使用している場合は、バージョン5.9.7.22 以降が必要です。 Visual Studio を使用している場合は、Xamarin tools for Visual Studio のバージョン3.11.1537 以降が必要です。 

- Android SDK マネージャーを使用して**Android SDK** &ndash; Android SDK 6.0 (API 23) 以降をインストールする必要があります。

- **Java Developer Kit** &ndash; Xamarin. Android では、API レベル24以上を開発している場合は[jdk 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)以降が必要です (jdk 1.8 では、Marshmallow を含む、24より前の api レベルもサポートされています)。 カスタムコントロールまたはフォームプレビューアーを使用する場合は、64ビットバージョンの JDK 1.8 が必要です。

特に API レベル23以前を開発している場合は、 [JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html)を使用し続けることができます。 

## <a name="getting-started"></a>作業の開始

Android Marshmallow と Xamarin android の使用を開始するには、Android Marshmallow プロジェクトを作成する前に、最新のツールと SDK パッケージをダウンロードしてインストールする必要があります。 

1. **安定**チャネルから最新の Xamarin 更新プログラムをインストールします。 

2. Android 6.0 Marshmallow SDK パッケージとツールをインストールします。

3. Android 6.0 Marshmallow (API レベル 23) を対象とする新しい Xamarin Android プロジェクトを作成します。 

4. Android Marshmallow 用のエミュレーターまたはデバイスを構成します。

これらの各手順については、次のセクションで説明します。

### <a name="install-xamarin-updates"></a>Xamarin の更新プログラムのインストール

Android 6.0 Marshmallow のサポートが含まれるように Xamarin を更新するには、更新チャネルを**安定**させ、すべての更新プログラムをインストールします。 更新プログラムのチャネルから更新プログラムをインストールする方法の詳細については、「[更新プログラムのチャネルを変更](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/change_updates_channel)する」を参照してください。 

### <a name="install-the-android-60-sdk"></a>Android 6.0 SDK のインストール

Android Marshmallow 用の Xamarin Android プロジェクトを作成するには、まず Android SDK マネージャーを使用して Android 6.0 SDK をインストールする必要があります。

- Android SDK Manager を起動します (Visual Studio for Mac では、**ツール > SDK マネージャー**を使用します。 Visual Studio では、**ツール > Android > Android SDK manager**) を使用して、最新の Android SDK Tools をインストールします。

    [Android SDK マネージャーで Android SDK ツールを選択![には](marshmallow-images/mnc-preview-tools.png)](marshmallow-images/mnc-preview-tools.png#lightbox)

- また、最新の**Android 6.0** SDK パッケージをインストールします。

    [Android SDK マネージャーで Android 6.0 SDK パッケージを選択![には](marshmallow-images/mnc-preview-packages.png)](marshmallow-images/mnc-preview-packages.png#lightbox)

Android SDK Tools revision 24.3.4 以降をインストールする必要があります。
Android SDK Manager を使用した Android 6.0 SDK のインストールの詳細については、「 [SDK Manager](https://developer.android.com/tools/help/sdk-manager.html)」を参照してください。

### <a name="start-a-xamarinandroid-project"></a>Xamarin. Android プロジェクトを開始する

新しい Xamarin. Android プロジェクトを作成します。 Xamarin を使用した Android 開発を初めて使用する場合は、「 [Hello, android](~/android/get-started/hello-android/index.md) 」を参照して、android プロジェクトの作成について学習してください。 

Android プロジェクトを作成するときは、バージョン設定を Android 6.0 MarshMallow を対象とするように構成する必要があります。 Marshmallow のプロジェクトを対象にするには、 **API レベル 23 (Xamarin Android v2.0 のサポート)** 用にプロジェクトを構成する必要があります。 Android API レベルレベルの構成の詳細については、「 [ANDROID Api レベルについ](~/android/app-fundamentals/android-api-levels.md)て」を参照してください。

### <a name="configure-an-emulator-or-device"></a>エミュレーターまたはデバイスを構成する

エミュレーターを使用している場合は、Android AVD マネージャーを起動し、次の設定を使用して新しいデバイスを作成します。

- デバイス:、5、6、または9。
- ターゲット: Android 6.0-API レベル23
- ABI: x86

たとえば、この仮想デバイスは、次のように、この仮想デバイスを構成して、次のようにします。

[[![の構成] で、[の接続5デバイス、Android 6.0 ターゲット、および Intel Atom (x86) を使用して AVD を構成する](marshmallow-images/android-m-avd.png)](marshmallow-images/android-m-avd.png#lightbox)

1つの物理デバイス (たとえば、Marshmallow)、6、または9を使用している場合は、Android のプレビューイメージをインストールできます。 デバイスを Android Marshmallow に更新する方法の詳細については、「[ハードウェアシステムイメージ](https://developer.android.com/preview/download.html#images)」を参照してください。

## <a name="new-features"></a>新機能

Android Marshmallow で導入された変更の多くは、Android ユーザーエクスペリエンスの向上、パフォーマンスの向上、およびバグの修正に重点を置いています。 ただし、Marshmallow では、Android プラットフォームの基本に対するいくつかの広範な変更も導入されました。 以下のセクションでは、これらの機能強化について説明し、アプリで新しい Android Marshmallow 機能の使用を開始する際に役立つリンクを示します。 

### <a name="runtime-permissions"></a>実行時のアクセス許可

Android のアクセス許可システムは、Android のロリポップから大幅に最適化され、簡素化されています。 Android Marshmallow では、ユーザーはインストール時ではなく、実行時にケースごとにアクセス許可を付与します。 Android Marshmallow 以降でこの機能をサポートするには、実行時にユーザーにアクセス許可を要求するようにアプリを設計します (アクセス許可が必要な場所のコンテキスト内)。 この変更により、アプリのインストールとアップグレードのプロセスが合理化されるため、ユーザーはすぐにアプリを使い始めることができます。 

Xamarin Android アプリでのランタイムアクセス許可の実装に関する詳細 (コード例を含む) については、「 [Android Marshmallow でランタイムアクセス許可を要求](https://blog.xamarin.com/requesting-runtime-permissions-in-android-marshmallow/)する」を参照してください。
Xamarin には、Android Marshmallow (およびそれ以降) で実行時のアクセス許可がどのように動作するかを示すサンプルアプリも用意されています: [Runtimepermissions](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-runtimepermissions)。

このサンプルアプリでは、次のことを示します。

- 実行時にアクセス許可を確認および要求する方法。
- Android M デバイスのアクセス許可を宣言する方法。

このサンプルアプリを使用するには:

1. **カメラ**または**連絡先**のボタンをタップすると、アクセス許可の要求ダイアログボックスが表示されます。
2. カメラまたは連絡先のフラグメントを表示するアクセス許可を付与します。

Android Marshmallow の新しいランタイムアクセス許可機能の詳細については、「[システムアクセス許可の操作](https://developer.android.com/preview/features/runtime-permissions.html)」を参照してください。

### <a name="authentication-enhancements"></a>認証の機能強化

Android Marshmallow には、パスワードの必要性をなくすために役立つ2つの認証拡張機能が用意されています。

- **指紋認証**&ndash; は、指紋スキャンを使用してユーザーを認証します。

- デバイスのロックが解除された期間に基づいてユーザーを認証 &ndash;**資格情報を確認**します。

次に説明するリンクとサンプルアプリは、これらの新機能を理解するのに役立ちます。

#### <a name="fingerprint-authentication"></a>指紋認証

指紋スキャンハードウェアをサポートするデバイスでは、新しい `FingerPrintManager` クラスを使用してユーザーを認証できます。
Android Marshmallow の指紋認証機能の詳細については、「[指紋認証](https://developer.android.com/preview/api-overview.html#fingerprint-authentication)」を参照してください。

Xamarin には、アプリでユーザーを認証するために登録された指紋を使用する方法を示すサンプルアプリが用意されています: [FingerprintDialog](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)。

このサンプルアプリを使用するには:

1. **[購入]** ボタンをタッチして、指紋認証ダイアログを開きます。
2. 登録した指紋をスキャンして認証します。

このサンプルアプリでは、指紋リーダーを備えたデバイスが必要であることに注意してください。
このアプリには、指紋 (またはパスワード) は保存されません。

#### <a name="voice-interactions"></a>音声操作

Android Marshmallow で導入された新しい音声操作機能を使用すると、アプリのユーザーは、音声を使用してアクションを確認し、オプションの一覧から選択できます。 音声操作の詳細については、「[音声対話 API の概要](https://developers.google.com/voice-actions/interaction/)」を参照してください。 

Xamarin Android アプリでの音声操作の実装に関する詳細 (コード例を含む) については、「[音声対話による Android アプリへのメッセージ交換の追加](https://blog.xamarin.com/add-a-conversation-to-your-android-app-with-voice-interactions/)」を参照してください。
サンプルアプリを使用して、Xamarin Android アプリで音声対話 API を使用する方法について[説明します](https://github.com/jamesmontemagno/MarshmallowSamples/tree/master/VoiceInteractions)。

#### <a name="confirm-credential"></a>資格情報の確認

Android Marshmallow の新しい [*資格情報の確認*] 機能を使用すると、デバイスのロックが解除された期間に基づいてユーザーを認証することで、アプリ固有のパスワードを覚えて入力する必要がなくなります。
これを行うには、`KeyGenerator`の新しい `SetUserAuthenticationValidityDurationSeconds` メソッドを使用します。 `KeyGuardManager`の `CreateConfirmDeviceCredentialIntent` メソッドを使用して、アプリ内からユーザーを再認証します。 Android Marshmallow のこの新機能の詳細については、「[資格情報の確認](https://developer.android.com/preview/api-overview.html#confirm-credential)」を参照してください。

Xamarin には、アプリでデバイスの資格情報 (PIN、パターン、パスワードなど) を使用する方法を示すサンプルアプリとして、 [Confirmcredential](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-confirmcredential)が用意されています。

このサンプルアプリを使用するには:

1. デバイスにセキュリティで保護されたロック画面を設定します (セキュリティで保護された **> セキュリティ > Screenlock**)。
2. **購入** ボタンをタップし、セキュリティで保護されたロック画面 の資格情報を確認します。

### <a name="chrome-custom-tabs"></a>Chrome カスタムタブ

アプリ開発者は、ユーザーが URL をタップしたときに選択肢に直面します。アプリは、ブラウザーを起動するか、`WebView`に基づいてアプリ内ブラウザーを使用できます。 どちらの方法でも、ブラウザーの起動 &ndash; は、カスタマイズできない高負荷なコンテキストスイッチであり、`WebView`はブラウザーと状態を共有しません。 また `WebView`s を使用すると、追加のメンテナンスのオーバーヘッドが発生する可能性があります。 

*Chrome カスタムタブ*を使用すると、ユーザーがアプリを離れることなく、chrome の機能で web サイトを簡単かつ洗練された方法で表示できます。 この機能により、アプリはユーザーの web エクスペリエンスをより細かく制御できます。これにより、`WebView`に頼ることなく、ネイティブコンテンツと web コンテンツの間でシームレスな移行を行うことができます。 また、次の項目をカスタマイズすると、Chrome の外観にも影響があります。 

- ツールバーの色

- アニメーションの開始と終了

- Chrome ツールバーのカスタムアクションとオーバーフローメニュー

- Chrome プリスタートおよびコンテンツプリフェッチ (高速読み込みの場合)

Xamarin Android アプリでこの機能を利用するには、 [Android Support カスタムタブライブラリ](https://www.nuget.org/packages/Xamarin.Android.Support.CustomTabs/)をダウンロードしてインストールします。
この機能の詳細については、「 [Chrome カスタムタブ](https://developer.chrome.com/multidevice/android/customtabs)」を参照してください。

### <a name="material-design-support-library"></a>マテリアルデザインサポートライブラリ

Android ロリポップは、Android エクスペリエンスを更新するための新しいデザイン言語として[マテリアルデザイン](https://www.google.com/design/spec/material-design/introduction.html)を導入しました (Xamarin android アプリでのマテリアルデザインの使用に関する情報については、「[マテリアルのテーマ](~/android/user-interface/material-theme.md)」を参照してください)。 Android Marshmallow を使用すると、アプリの開発者が素材のデザインのルックアンドフィールを簡単に採用できるように、 *android のデザインサポートライブラリ*が導入されました。 このライブラリには、次のコンポーネントが含まれています。

- **CoordinatorLayout** &ndash; 新しい `CoordinatorLayout` ウィジェットは、`FrameLayout`よりも強力ですが、より強力です。 `CoordinatorLayout` は、子ビューのコンテナーとして、または最上位のレイアウトとして使用できます。また、ビューを他のビューとの間で固定するために使用できる `layout_anchor` 属性を提供します。

- 新しい `CollapsingToolbarLayout` &ndash;**ツールバーの折りたたみ**は、`Toolbar`のラッパーである折りたたみアプリバーです。 (*アプリバー*は、以前は*操作バー*と呼ばれていたものです)。

- **フローティング動作ボタン**&ndash;、アプリのインターフェイスの主要なアクションを示す丸いボタンです。

- **テキストを編集するための浮動ラベル**&ndash; は、ユーザーがテキストを入力したときにヒントが非表示になったときに、新しい `TextInputLayout` ウィジェット (`EditText`をラップする) を使用してフローティングラベルを表示します。

- **ナビゲーションビュー** &ndash; 新しい `NavigationView` ウィジェットを使用すると、ユーザーが簡単に移動できるようにナビゲーションドロワーを使用できます。

- **Snackbar** &ndash; 新しい `SnackBar` ウィジェットは、画面上の他のすべての要素の上に表示される簡単なメッセージを画面の下部に表示する軽量のフィードバックメカニズム (トーストに似ています) です。

- 新しい `TabLayout` ウィジェット &ndash;**素材のタブ**では、アプリにトップレベルのナビゲーションを実装する方法としてタブを表示するための水平レイアウトが提供されます。

Xamarin Android アプリで[デザインサポートライブラリ](https://developer.android.com/tools/support-library/features.html#design)を利用するには、Xamarin [Xamarin Support library Design](https://www.nuget.org/packages/Xamarin.Android.Support.Design/) NuGet パッケージをダウンロードしてインストールします。

Xamarin Android アプリでのマテリアルデザインサポートライブラリの使用に関する詳細 (コード例を含む) については[、「Android サポートデザインライブラリを使用した美しいマテリアル設計](https://blog.xamarin.com/add-beautiful-material-design-with-the-android-support-design-library/)」を参照してください。
Xamarin には、Cheesesquare の新しい Android デザインライブラリをデモするサンプルアプリが用意されています。 Android &ndash; [](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-cheesesquare)
このサンプルでは、デザインライブラリの次の機能を示します。

- 折りたたみ (ツールバーを)
- 浮動アクションボタン
- 表示アンカー
- NavigationView
- Snackbar

デザインライブラリの詳細については、Android 開発者ブログの「 [Android Design Support library](https://android-developers.googleblog.com/2015/05/android-design-support-library.html) 」を参照してください。

### <a name="additional-library-updates"></a>追加のライブラリの更新

Android Marshmallow に加えて、Google はいくつかのコア Android ライブラリに関連する更新を発表しました。 Xamarin では、プレビューリリースの NuGet パッケージを使用して、これらの更新プログラムに対する Xamarin をサポートしています。 

- [Google Play 開発者サービス](https://www.nuget.org/packages?q=Xamarin+Google+Play+Services)&ndash; 最新バージョンの Google Play 開発者サービスには、新しい*アプリの招待*機能が含まれています。これにより、ユーザーはアプリを友人と共有できます。 この機能の詳細については、「 [Google のアプリの招待でアプリのリーチを拡大する](https://blog.xamarin.com/expand-your-apps-reach-with-googles-app-invites/)」を参照してください。 

- [Android サポート &ndash; ライブラリ](https://www.nuget.org/packages?q=xamarin+support+library)は、ライブラリ api でのみ使用できる機能を提供しますが、nuget は、Android framework api の下位互換性のあるバージョンを提供します。 

- [Android ウェアラブルライブラリ](https://www.nuget.org/packages/Xamarin.Android.Wear)&ndash; この NuGet には Google Play 開発者サービスバインドが含まれています。 最新バージョンのウェアラブルライブラリは、Android の磨耗プラットフォームへの新しい機能 (カスタムアプリの簡単なナビゲーションを含む) を提供します。 

## <a name="summary"></a>まとめ

この記事では、Android Marshmallow を導入し、Marshmallow で Xamarin Android 開発用の最新のツールとパッケージをインストールして構成する方法について説明しました。 また、Xamarin Android の開発で最も魅力的な新しい Android Marshmallow 機能の概要についても説明しました。

## <a name="related-links"></a>関連リンク

- [Android 6.0 Marshmallow](https://developer.android.com/about/versions/marshmallow/index.html)
- [Android SDK を取得する](https://developer.android.com/sdk/index.html#Other)
- [機能の概要](https://developer.android.com/preview/api-overview.html)
- [リリース ノート](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_5/xamarin.android_5.1.99/index.md)
- [RuntimePermissions (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-runtimepermissions)
- [ConfirmCredential (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-confirmcredential)
- [FingerprintDialog (サンプル)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
