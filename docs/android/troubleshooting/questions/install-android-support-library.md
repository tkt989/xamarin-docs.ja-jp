---
title: Xamarin.Android.Support パッケージに必要な Android サポート ライブラリを手動でインストールする方法を教えてください
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A9CB8CA8-8A6D-405E-B84C-A16CE452C0F7
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 99571e0b62592597bb1fffdc8d3ed8336fe050b2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026924"
---
# <a name="how-can-i-manually-install-the-android-support-libraries-required-by-the-xamarinandroidsupport-packages"></a>Xamarin.Android.Support パッケージに必要な Android サポート ライブラリを手動でインストールする方法を教えてください

## <a name="example-steps-for-xamarinandroidsupportv4"></a>Xamarin. Android. Support. v4 の手順例 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

必要な Xamarin. Android. サポート NuGet パッケージをダウンロードします (たとえば、NuGet パッケージマネージャーを使用してインストールします)。

`ildasm` を使用して、NuGet パッケージに必要な**android_m2repository**のバージョンを確認します。

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr SourceUrl
```

出力例:

```cmd
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
property string 'SourceUrl' = string('https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip')
```

**Ildasm.exe**から返された URL を使用して、 **android\_m2repository**を Google からダウンロードします。 または、Android SDK Manager に現在インストールされている_Android サポートリポジトリ_のバージョンを確認することもできます。

!["Android サポートリポジトリバージョン32がインストールされている Android SDK Manager"](install-android-support-library-images/sdk-extras.png)

バージョンが NuGet パッケージに必要なバージョンと一致する場合、新しいものをダウンロードする必要はありません。 代わりに、[すべてのユーザー **\\** ] の下にある既存の**m2repository**ディレクトリを (Android SDK Manager ウィンドウの上部に表示されているように) _SDK パス_に再 zip することができます。

**Ildasm**から返された URL の MD5 ハッシュを計算します。 結果の文字列の書式を設定して、大文字とスペースを使用しないようにします。 たとえば、必要に応じて `$url` 変数を調整し、PowerShell で次の2行 ( [Xamarin. C# Android からの元のコード](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)に基づく) を実行します。

```powershell
$url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"
(([System.Security.Cryptography.MD5]::Create()).ComputeHash([System.Text.Encoding]::UTF8.GetBytes($url)) | %{ $_.ToString("X02") }) -join ""
```

出力例:

```powershell
F16A3455987DBAE5783F058F19F7FCDF
```

**Android\_m2repository**を **% localappdata%\\Xamarin\\zip\\** フォルダーにコピーします。 前の MD5 ハッシュ計算手順の MD5 ハッシュを使用するように、ファイルの名前を変更します。 (例:

**% LOCALAPPDATA%\\Xamarin\\zip\\F16A3455987DBAE5783F058F19F7FCDF**

Optionalファイルを **% Localappdata%\\xamarin\\\\23.4.0.0\\コンテンツ\\** (**コンテンツ\\m2repository**サブディレクトリの作成) に解凍します。 この手順を省略した場合、この手順を完了する必要があるため、ライブラリを使用する最初のビルドは少し時間がかかります。
サブディレクトリのバージョン番号 (この例では**23.4.0.0** ) は、NuGet パッケージのバージョンとはまったく同じではありません。 `ildasm` を使用して、正しいバージョン番号を見つけることができます。

```cmd
ildasm /caverbal /text /item:Xamarin.Android.Support.v4 packages\Xamarin.Android.Support.v4.23.4.0.1\lib\MonoAndroid403\Xamarin.Android.Support.v4.dll | findstr /C:"string 'Version'"
```

出力例:

```cmd
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
property string 'Version' = string('23.4.0.0')}
```

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio for Mac](#tab/macos)

必要な Xamarin. Android. サポート NuGet パッケージをダウンロードします (たとえば、NuGet パッケージマネージャーを使用してインストールします)。

Visual Studio for Mac で、Android プロジェクトの [_参照_] セクションの下にある_Xamarin. Android. サポート_アセンブリをダブルクリックして、アセンブリブラウザーでアセンブリを開きます。 [_言語_] ドロップダウンがに_C#_ 設定されていることを確認し、アセンブリブラウザーのナビゲーションツリーから最上位レベルの Xamarin. _Android. サポート_アセンブリを選択します。 `IncludeAndroidResourcesFrom` または `JavaLibraryReference` の属性のいずれかで、`SourceUrl` プロパティを見つけます。

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

**Ildasm.exe**から返された `SourceUrl` を使用して、 **android\_m2repository**を Google からダウンロードします。 または、Android SDK Manager に現在インストールされている_Android サポートリポジトリ_のバージョンを確認することもできます。

!["Android サポートリポジトリバージョン32がインストールされている Android SDK Manager"](install-android-support-library-images/sdk-extras.png)

バージョンが NuGet パッケージに必要なバージョンと一致する場合、新しいものをダウンロードする必要はありません。 代わりに、(Android SDK Manager ウィンドウの上部に表示されているように) _SDK パス_にある **[エクストラ/android]** の下にある既存の**m2repository**ディレクトリを再 zip することができます。

**Ildasm**から返された URL の MD5 ハッシュを計算します。 結果の文字列の書式を設定して、大文字とスペースを使用しないようにします。 たとえば、必要に応じて URL 文字列を調整してから、ターミナルで次のコマンドを実行し**ます。 app**コマンドプロンプト:

```bash
echo -n "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip" | md5 | tr '[:lower:]' '[:upper:]'
```

別の方法として、`csharp` インタープリターを使用して、 [Xamarin と同じC#コード](https://github.com/xamarin/xamarin-android/blob/8e8a4dd90f26eb39172876cc52181b6639e20524/src/Xamarin.Android.Build.Tasks/Tasks/GetAdditionalResourcesFromAssemblies.cs#L208)を実行する方法もあります。
これを行うには、必要に応じて `url` 変数を調整してから、ターミナルで次のコマンドを実行し**ます。 app**コマンドプロンプト:

```bash
csharp -e 'var url = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip"; string.Concat((System.Security.Cryptography.MD5.Create().ComputeHash(System.Text.Encoding.UTF8.GetBytes(url))).Select(b => b.ToString("X02")))'
```

出力例:

```bash
F16A3455987DBAE5783F058F19F7FCDF
```

**Android\_m2repository**を **$HOME/.local/share/xamarin/zips/** フォルダーにコピーします。 前の MD5 ハッシュ計算手順の MD5 ハッシュを使用するように、ファイルの名前を変更します。 (例:

**$HOME/.local/share/Xamarin/zips/F16A3455987DBAE5783F058F19F7FCDF.zip**

Optionalファイルをに解凍します。 

**$HOME/.local/share/Xamarin/Xamarin.Android.Support.v4/23.4.0.0/content/**

( **content/m2repository**サブディレクトリを作成します)。 この手順を省略した場合、この手順を完了する必要があるため、ライブラリを使用する最初のビルドは少し時間がかかります。

サブディレクトリのバージョン番号 (この例では**23.4.0.0** ) は、NuGet パッケージのバージョンとはまったく同じではありません。 前の**ildasm.exe**手順と同様に、Visual Studio for Mac のアセンブリブラウザーを使用して、正しいバージョン番号を見つけることができます。 `IncludeAndroidResourcesFrom` または `JavaLibraryReference` の属性のいずれかで、`Version` プロパティを探します。

```csharp
[assembly: IncludeAndroidResourcesFrom ("./", PackageName = "Xamarin.Android.Support.v4", SourceUrl = "https://dl-ssl.google.com/android/repository/android_m2repository_r32.zip", EmbeddedArchive = "m2repository/com/android/support/support-v4/23.4.0/support-v4-23.4.0.aar", Version = "23.4.0.0")]
```

-----

## <a name="additional-references"></a>その他の参照情報

- [バグ 43245](https://bugzilla.xamarin.com/show_bug.cgi?id=43245) -正しくない "ダウンロードに失敗しました。 {0} をダウンロードし、{1} ディレクトリに配置してください。 " 「パッケージ '{0}' をインストールしてください。 SDK インストーラーに関連するエラーメッセージ」を参照してください。

### <a name="next-steps"></a>次のステップ

このドキュメントでは、2016年8月時点の現在の動作について説明します。 このドキュメントで説明されている手法は、Xamarin の安定したテストスイートの一部ではないため、将来中断する可能性があります。

詳細については、お問い合わせください。または、上記の情報を利用した後もこの問題が発生する場合は、「 [Xamarin で使用できるサポートオプション](~/cross-platform/troubleshooting/support-options.md)」を参照してください。連絡先オプション、提案、および必要に応じて新しいバグをファイルに登録する方法については、こちらを参照してください.
