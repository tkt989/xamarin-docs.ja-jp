---
title: MonoTouch.Dialog Json マークアップ
description: このドキュメントでは、Monotouch.dialog を使用して Xamarin のユーザーインターフェイスを構築するために使用できる JSON 構文について説明します。
ms.prod: xamarin
ms.assetid: 59F3E18C-3A73-69B8-DA5E-21B19B9DFB98
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: 84698ab769156726982c4d5a38d5f284bdc30328
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002222"
---
# <a name="monotouchdialog-json-markup"></a>MonoTouch.Dialog Json マークアップ

このページでは、Monotouch.dialog の[Jsonelement](xref:MonoTouch.Dialog.JsonElement)によって受け入れられる Json マークアップについて説明します。

まず、例を見てみましょう。 次に、JsonElement に渡すことができる完全な Json ファイルを示します。

```json
{     
    "title": "Json Sample",
    "sections": [ 
        {
            "header": "Booleans",
            "footer": "Slider or image-based",
            "id": "first-section",
            "elements": [
                { 
                    "type": "boolean",
                    "caption": "Demo of a Boolean",
                    "value": true
                }, {
                    "type": "boolean",
                    "caption": "Boolean using images",
                    "value": false,
                    "on": "favorite.png",
                    "off": "~/favorited.png"
                }, {
                    "type": "root",
                    "title": "Tap for nested controller",
                    "sections": [
                        {
                            "header": "Nested view!",
                            "elements": [
                                {
                                    "type": "boolean",
                                    "caption": "Just a boolean",
                                    "id": "the-boolean",
                                    "value": false
                                }, {
                                    "type": "string",
                                    "caption": "Welcome to the nested controller"
                                }
                            ]
                        }
                    ]
                }
            ]
        }, {
            "header": "Entries",
            "elements" : [
                {
                    "type": "entry",
                    "caption": "Username",
                    "value": "",
                    "placeholder": "Your account username"
                }
            ]
        }
    ]
}
```

上記のマークアップでは、次の UI が生成されます。

 [![](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png "The UI created by the given markup")](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png#lightbox)

ツリー内のすべての要素には、`"id"`プロパティを含めることができます。 実行時に、JsonElement インデクサーを使用して個々のセクションまたは要素を参照することができます。 以下に例を示します。

```csharp
var jsonElement = JsonElement.FromFile ("demo.json");

var firstSection = jsonElement ["first-section"] as Section;

var theBoolean = jsonElement ["the-boolean"] as BooleanElement;
```

 <a name="Root_Element_Syntax" />

## <a name="root-element-syntax"></a>ルート要素の構文

ルート要素には、次の値が含まれます。

- `title`
- `sections` (省略可能)

ルート要素は、入れ子になったコントローラーを作成するための要素としてセクション内に表示できます。 その場合は、追加のプロパティ `"type"` をに設定する必要があり `"root"`

 <a name="url" />

### <a name="url"></a>url

`"url"` プロパティが設定されている場合、ユーザーがこの RootElement をタップすると、コードは指定された url からファイルを要求し、その内容を新しい情報に表示します。 これを使用すると、ユーザーがタップした内容に基づいて、サーバーからユーザーインターフェイスを拡張できます。

 <a name="group" />

### <a name="group"></a>グループ

設定すると、ルート要素の groupname が設定されます。 グループ名は、要素内の入れ子になった要素の1つからルート要素の値として表示される概要を選択するために使用されます。 これは、チェックボックスの値、またはラジオボタンの値のいずれかです。

 <a name="radioselected" />

### <a name="radioselected"></a>radioselected

入れ子になった要素で選択されているラジオ項目を識別します

 <a name="title" />

### <a name="title"></a>タイトル

存在する場合は、RootElement に使用されるタイトルになります。

 <a name="type" />

### <a name="type"></a>型

このがセクションに表示される場合は `"root"` に設定する必要があります (これはコントローラーを入れ子にするために使用されます)。

 <a name="sections" />

### <a name="sections"></a>sections

これは、個々のセクションを含む Json 配列です

 <a name="Section_Syntax" />

## <a name="section-syntax"></a>セクションの構文

セクションには次のものが含まれます。

- `header` (省略可能)
- `footer` (省略可能)
- `elements` 配列

 <a name="header" />

### <a name="header"></a>ヘッダー

ヘッダーテキストが存在する場合は、セクションのキャプションとして表示されます。

 <a name="footer" />

### <a name="footer"></a>フッター

存在する場合は、セクションの下部にフッターが表示されます。

 <a name="elements" />

### <a name="elements"></a>要素

これは、要素の配列です。 各要素には、作成する要素の種類を識別するために使用されるキー (`"type"` キー) が少なくとも1つ含まれている必要があります。
一部の要素は、`"caption"` や `"value"`などのいくつかの一般的なプロパティを共有しています。 サポートされている要素の一覧を次に示します。

- `string` 要素 (スタイルの有無にかかわらず)
- `entry` の行 (標準またはパスワード)
- `boolean` 値 (スイッチまたはイメージを使用)

文字列要素は、ユーザーがセルまたはアクセサリをタップしたときに呼び出すメソッドを提供することで、ボタンとして使用できます。

 <a name="Rendering_Elements" />

## <a name="rendering-elements"></a>レンダリング要素

レンダリング要素はC# stringelement と StyledStringElement に基づいており、さまざまな方法で情報を表示でき、さまざまな方法でレンダリングすることができます。 最も単純な要素は次のように作成できます。

```json
{
    "type": "string",
    "caption": "Json Serializer"
}
```

これにより、すべての既定値を含む単純な文字列が表示されます。フォント、背景、テキストの色、および装飾です。 これらの要素にアクションをフックし、`"ontap"` プロパティまたは `"onaccessorytap"` プロパティを設定して、ボタンのように動作させることができます。

```json
{
    "type": "string",
    "caption": "View Photos",
    "ontap": "Acme.PhotoLibrary.ShowPhotos"
}
```

上の例では、"PhotoLibrary" クラスの "ShowPhotos" メソッドを呼び出しています。 `"onaccessorytap"` は似ていますが、ユーザーがセルをタップする代わりにアクセサリをタップした場合にのみ呼び出されます。 これを有効にするには、アクセサリも設定する必要があります。

```json
{
    "type": "string",
    "caption": "View Photos",
    "ontap": "Acme.PhotoLibrary.ShowPhotos",
    "accessory": "detail-disclosure",
    "onaccessorytap": "Acme.PhotoLibrary.ShowStats"
}
```

レンダリング要素には、2つの文字列を一度に表示できます。1つはキャプションで、もう1つは値です。 これらの文字列がどのようにレンダリングされるかは、スタイルによって異なります。これは、`"style"` プロパティを使用して設定できます。 既定では、左側にキャプション、右側に値が表示されます。 詳細については、「style」セクションを参照してください。 色は、' # ' 記号の後に、赤、緑、青、および場合によってはアルファ値の値を表す16進数値を使用してエンコードされます。 コンテンツは、RGB 値または RGBA 値を表す短い形式 (3 桁または4桁の16進数) でエンコードできます。 または、RGB 値または RGBA 値を表す長い形式 (6 桁または8桁) です。 短いバージョンは、同じ16進数字を2回記述するための短縮形です。 このため、"#1bc" 定数は、red = 0x11、green = 0xbb、blue = 0xbb として宣言されています。 アルファ値が存在しない場合、色は不透明になります。 以下に、いくつかの例を示します。

```json
"background": "#f00"
"background": "#fa08f880"
```

 <a name="accessory" />

### <a name="accessory"></a>アクセサリ

表示要素に表示されるアクセサリの種類を決定します。有効な値は次のとおりです。

- `checkmark`
- `detail-disclosure`
- `disclosure-indicator`

値が存在しない場合、アクセサリは表示されません。

 <a name="background" />

### <a name="background"></a>バックグラウンド

Background プロパティは、セルの背景色を設定します。 値はイメージの URL (この場合は、非同期イメージダウンローダーが呼び出され、イメージがダウンロードされるとバックグラウンドが更新されます) または color 構文を使用して指定された色になります。

 <a name="caption" />

### <a name="caption"></a>キャプション

表示要素に表示されるメイン文字列。 フォントと色をカスタマイズするには、`"textcolor"` と `"font"` のプロパティを設定します。 表示スタイルは、`"style"` プロパティによって決定されます。

 <a name="color_and_detailcolor" />

### <a name="color-and-detailcolor"></a>色と色の色

メインテキストまたは詳細テキストに使用する色。

 <a name="detailfont_and_font" />

### <a name="detailfont-and-font"></a>[すべてのフォントとフォント]

キャプションまたは詳細テキストに使用するフォント。 フォント仕様の形式としては、フォント名の後にダッシュとポイントサイズを指定します。
有効なフォントの仕様は次のとおりです。

- Helvetica,
- "Helvetica,-14"

 <a name="linebreak" />

### <a name="linebreak"></a>改行

行をどのように分割するかを決定します。 次の値を指定できます。

- `character-wrap`
- `clip`
- `head-truncation`
- `middle-truncation`
- `tail-truncation`
- `word-wrap`

`character-wrap` と `word-wrap` はどちらも、0に設定された `"lines"` プロパティと共に使用して、レンダリング要素を複数行の要素に変換できます。

 <a name="ontap_and_onaccessorytap" />

### <a name="ontap-and-onaccessorytap"></a>ontap と onaccessorytap

これらのプロパティは、オブジェクトをパラメーターとして受け取るアプリケーション内の静的メソッド名を指す必要があります。 FromFile または JsonDialog. FromJson メソッドを使用して階層を作成すると、オプションのオブジェクト値を渡すことができます。 その後、このオブジェクトの値がメソッドに渡されます。 これを使用すると、一部のコンテキストを静的メソッドに渡すことができます。 次に例を示します。

```csharp
class Foo {
    Foo ()
    {
        root = JsonDialog.FromJson (myJson, this);
    }

    static void Callback (object obj)
    {
        Foo myFoo = (Foo) obj;
        obj.Callback ();
    }
}
```

 <a name="lines" />

### <a name="lines"></a>線

この値が0に設定されている場合は、含まれている文字列の内容に応じて、要素の自動サイズが設定されます。 これを機能させるには、`"linebreak"` プロパティを `"character-wrap"` または `"word-wrap"`に設定する必要もあります。

 <a name="style" />

### <a name="style"></a>style

スタイルは、コンテンツの表示に使用されるセルスタイルの種類を決定し、UITableViewCellStyle 列挙値に対応します。
次の値を指定できます。

- `"default"`
- `"value1"`
- `"value2"`
- `"subtitle"`: サブタイトルを使用したテキスト。

 <a name="subtitle" />

### <a name="subtitle"></a>サブタイトル

サブタイトルに使用する値。 これは、スタイルを `"subtitle"` に設定し、`"value"` プロパティを文字列に設定するためのショートカットです。
これには、1つのエントリがあります。

 <a name="textcolor" />

### <a name="textcolor"></a>textcolor

テキストに使用する色。

 <a name="value" />

### <a name="value"></a>値

表示要素に表示される2番目の値。 このレイアウトは、`"style"` 設定の影響を受けます。 フォントと色をカスタマイズするには、`"detailfont"` と `"detailcolor"`を設定します。

 <a name="Boolean_Elements" />

## <a name="boolean-elements"></a>ブール型の要素

ブール型の要素では、型を `"bool"`に設定し、表示する `"caption"` を含めることができます。また、`"value"` は true または false に設定されます。 `"on"` プロパティと `"off"` プロパティが設定されている場合、それらは画像と見なされます。 イメージは、アプリケーションの現在の作業ディレクトリを基準にして解決されます。 バンドル相対ファイルを参照する場合は、アプリケーションバンドルディレクトリを表すショートカットとして `"~"` を使用できます。 たとえば `"~/favorite.png"` は、バンドルファイルに格納されている .png です。 次に例を示します。

```json
{ 
    "type": "boolean",
    "caption": "Demo of a Boolean",
    "value": true
},

{
    "type": "boolean",
    "caption": "Boolean using images",
    "value": false,
    "on": "favorite.png",
    "off": "~/favorited.png"
}
```

 <a name="type" />

### <a name="type"></a>型

Type は `"boolean"` または `"checkbox"`のいずれかに設定できます。 ブール値に設定すると、UISlider またはイメージが使用されます (`"on"` と `"off"` の両方が設定されている場合)。 Checkbox に設定すると、チェックボックスが使用されます。 `"group"` プロパティを使用して、特定のグループに属するブール型の要素にタグを付けることができます。 これは、ルートが、同じグループに属するすべてのブール値 (またはチェックボックス) のカウントによって結果を要約するため、包含するルートにも `"group"` プロパティがある場合に便利です。

 <a name="Entry_Elements" />

## <a name="entry-elements"></a>Entry 要素

入力要素を使用して、ユーザーがデータを入力できるようにします。 エントリ要素の型は、`"entry"` または `"password"`のいずれかです。 `"caption"` プロパティは、右側に表示されるテキストに設定され、`"value"` にはエントリを設定するための初期値が設定されます。 `"placeholder"` は、空のエントリに対してユーザーにヒントを表示するために使用されます (グレー表示されています)。 次にいくつかの例を示します。

```json
{
    "type": "entry",
    "caption": "Username",
    "value": "",
    "placeholder": "Your account username"
}, {
    "type": "password",
    "caption": "Password",
    "value": "",
    "placeholder": "You password"
}, {
    "type": "entry",
    "caption": "Zip Code",
    "value": "01010",
    "placeholder": "your zip code",
    "keyboard": "numbers"
}, {
    "type": "entry",
    "return-key": "route",
    "caption": "Entry with 'route'",
    "placeholder": "captialization all + no corrections",
    "capitalization": "all",
    "autocorrect": "no"
}
```

 <a name="autocorrect" />

### <a name="autocorrect"></a>自動修正

エントリに使用する自動修正スタイルを決定します。 有効な値は、true または false (または文字列 `"yes"` と `"no"`) です。

 <a name="capitalization" />

### <a name="capitalization"></a>大文字使用

エントリに使用する大文字/小文字のスタイル。 次の値を指定できます。

- `all`
- `none`
- `sentences`
- `words`

 <a name="caption" />

### <a name="caption"></a>キャプション

エントリに使用するキャプション

 <a name="keyboard" />

### <a name="keyboard"></a>キーボード

データ入力に使用するキーボードの種類。 次の値を指定できます。

- `ascii`
- `decimal`
- `default`
- `email`
- `name`
- `numbers`
- `numbers-and-punctuation`
- `twitter`
- `url`

 <a name="placeholder" />

### <a name="placeholder"></a>ホルダー

エントリに空の値が含まれている場合に表示されるヒントテキスト。

 <a name="return-key" />

### <a name="return-key"></a>return キー

Return キーに使用されるラベルです。 次の値を指定できます。

- `default`
- `done`
- `emergencycall`
- `go`
- `google`
- `join`
- `next`
- `route`
- `search`
- `send`
- `yahoo`

 <a name="value" />

### <a name="value"></a>値

エントリの初期値

 <a name="Radio_Elements" />

## <a name="radio-elements"></a>ラジオ要素

Radio 要素には `"radio"`型があります。 選択された項目は、それを含むルート要素の `radioselected` プロパティによって選択されます。
また、`"group"` プロパティに値が設定されている場合、このオプションボタンはそのグループに属しています。

 <a name="Date_and_Time_Elements" />

## <a name="date-and-time-elements"></a>日付と時刻の要素

要素型 `"datetime"`、`"date"`、および `"time"` は、日時、日付、または時刻を含む日付を表示するために使用されます。 これらの要素は、キャプションと値をパラメーターとして受け取ります。 この値は、.NET の DateTime. Parse 関数でサポートされている任意の形式で記述できます。 例:

```json
"header": "Dates and Times",
"elements": [
    {
        "type": "datetime",
        "caption": "Date and Time",
        "value": "Sat, 01 Nov 2008 19:35:00 GMT"
    }, {
        "type": "date",
        "caption": "Date",
        "value": "10/10"
    }, {
        "type": "time",
        "caption": "Time",
        "value": "11:23"
    }                       
]
```

 <a name="Html/Web_Element" />

## <a name="htmlweb-element"></a>Html/Web 要素

`"html"` の種類を使用して、ローカルまたはリモートのいずれかの指定された URL のコンテンツを表示する UIWebView をタップして埋め込むと、セルを作成できます。 この要素の2つのプロパティのみが `"caption"` および `"url"`ます。

```json
{
    "type": "html",
    "caption": "Miguel's blog",
    "url": "https://tirania.org/blog" 
}
```
