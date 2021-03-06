---
title: 認証と承認
description: この章では、eShopOnContainers モバイルアプリがコンテナー化されたマイクロサービスに対して認証と承認を実行する方法について説明します。
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: 7ced28a5ed379b761ffcc9841f5d0e923ec747e7
ms.sourcegitcommit: 6de849e2feca928ce5d91a3897e7d4049301081c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/06/2020
ms.locfileid: "75667053"
---
# <a name="authentication-and-authorization"></a>認証と承認

認証とは、ユーザーからの名前やパスワードなどの id 資格情報を取得し、それらの資格情報を機関に対して検証するプロセスです。 資格情報が有効な場合、資格情報を送信したエンティティは認証済み id と見なされます。 Id が認証されると、その id が特定のリソースにアクセスできるかどうかが承認プロセスによって判断されます。

認証と承認を ASP.NET MVC web アプリケーションと通信する Xamarin. Forms アプリに統合するには、さまざまな方法があります。たとえば、ASP.NET Core Id、外部認証プロバイダー (Microsoft、Google など) を使用します。Facebook、Twitter、および認証ミドルウェア。 EShopOnContainers モバイルアプリは、IdentityServer 4 を使用するコンテナー化された id マイクロサービスで認証と承認を実行します。 モバイルアプリは、ユーザーを認証したり、リソースにアクセスしたりするために、IdentityServer のセキュリティトークンを要求します。 ユーザーの代わりに IdentityServer がトークンを発行するようにするには、ユーザーが IdentityServer にサインインする必要があります。 ただし、IdentityServer は認証用のユーザーインターフェイスまたはデータベースを提供しません。 そのため、eShopOnContainers 参照アプリケーションでは、ASP.NET Core Id がこの目的に使用されます。

## <a name="authentication"></a>認証

アプリケーションが現在のユーザーの id を認識している必要がある場合は、認証が必要です。 ユーザーを識別するための主なメカニズムは、ASP.NET Core Id メンバーシップシステムで、開発者が構成したデータストアにユーザー情報が格納されます。これには、ASP.NET Core ます。 通常、このデータストアは EntityFramework ストアになりますが、カスタムストアまたはサードパーティのパッケージを使用して、Azure storage、Azure Cosmos DB、またはその他の場所に id 情報を格納することができます。

ローカルユーザーデータストアを使用する認証シナリオでは、ASP.NET MVC web アプリケーションの一般的なように、cookie を介して要求間で id 情報を保持する認証シナリオでは、ASP.NET Core Id が適切なソリューションです。 ただし、cookie は常にデータを永続化および転送するための自然な手段ではありません。 たとえば、モバイルアプリからアクセスされる RESTful エンドポイントを公開する ASP.NET Core web アプリケーションは、通常、ベアラートークン認証を使用する必要があります。これは、このシナリオでは cookie を使用できないためです。 ただし、ベアラートークンは、モバイルアプリから作成された web 要求の authorization ヘッダーに簡単に取得して含めることができます。

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>IdentityServer 4 を使用したベアラートークンの発行

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4)は、オープンソースの OpenID Connect および OAuth 2.0 フレームワーク ASP.NET Core 用です。ローカル ASP.NET Core id ユーザーのセキュリティトークンを発行するなど、多くの認証と承認のシナリオで使用できます。

> [!NOTE]
> OpenID Connect と OAuth 2.0 はよく似ていますが、さまざまな役割があります。

OpenID Connect は、OAuth 2.0 プロトコル上の認証レイヤーです。 OAuth 2 は、アプリケーションが Security Token Service からアクセストークンを要求し、Api との通信に使用できるようにするプロトコルです。 この委任により、認証と承認を一元化できるため、クライアントアプリケーションと Api の両方の複雑さが軽減されます。

OpenID Connect と OAuth 2.0 の組み合わせによって、認証と API アクセスの2つの基本的なセキュリティ上の懸念が組み合わされています。また、このプロトコルの実装は、IdentityServer 4 です。

EShopOnContainers 参照アプリケーションなどのクライアントからマイクロサービスへの直接通信を使用するアプリケーションでは、図に示すように、セキュリティトークンサービス (STS) として機能する専用の認証マイクロサービスを使用して、ユーザーを認証できます。9-1。 クライアントからマイクロサービスへの直接通信の詳細については、「[クライアントとマイクロサービス間の通信](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices)」を参照してください。

![](authentication-and-authorization-images/authentication.png "Authentication by a dedicated authentication microservice")

**図 9-1:** 専用認証マイクロサービスによる認証

EShopOnContainers モバイルアプリは、id マイクロサービスと通信します。これは、認証を実行するためにユーザー id を使用し、Api のアクセス制御を行います。 そのため、モバイルアプリは、ユーザーを認証したり、リソースにアクセスしたりするために、ユーザーのトークンを要求します。

- IdentityServer のユーザーの認証は、認証プロセスの結果を表す*id*トークンを要求するモバイルアプリによって実現されます。 最低でも、ユーザーの識別子と、ユーザーが認証された方法とタイミングに関する情報が含まれています。 また、追加の id データを含めることもできます。
- IdentityServer を使用してリソースにアクセスするには、*アクセス*トークンを要求するモバイルアプリを使用します。これにより、API リソースにアクセスできるようになります。 クライアントはアクセストークンを要求し、API に転送します。 アクセストークンには、クライアントとユーザーに関する情報 (存在する場合) が含まれます。 Api は、その情報を使用して、データへのアクセスを承認します。

> [!NOTE]
> クライアントは、トークンを要求できるようにする前に、クライアントを IdentityServer に登録する必要があります。

### <a name="adding-identityserver-to-a-web-application"></a>Web アプリケーションへの IdentityServer の追加

ASP.NET Core web アプリケーションで、IdentityServer 4 を使用するには、web アプリケーションの Visual Studio ソリューションに追加する必要があります。 詳細については、「IdentityServer のドキュメント」の「[概要](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html)」を参照してください。

Web アプリケーションの Visual Studio ソリューションに提供された IdentityServer は、web アプリケーションの HTTP 要求処理パイプラインに追加して、OpenID Connect および OAuth 2.0 エンドポイントへの要求を処理できるようにする必要があります。 これは、次のコード例に示すように、web アプリケーションの `Startup` クラスの `Configure` メソッドで実現されます。

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Web アプリケーションの HTTP 要求処理パイプラインでは順序が重要です。 そのため、ログイン画面を実装する UI フレームワークの前に、IdentityServer がパイプラインに追加されている必要があります。

### <a name="configuring-identityserver"></a>IdentityServer の構成

EShopOnContainers reference アプリケーションの次のコード`ConfigureServices`例に示すように、 `Startup` `services.AddIdentityServer`メソッドを呼び出すことによって、web アプリケーションのクラスのメソッドで、IdentityServer を構成する必要があります。

```csharp
public void ConfigureServices(IServiceCollection services)  
{  
    ...  
    services.AddIdentityServer(x => x.IssuerUri = "null")  
        .AddSigningCredential(Certificate.Get())                 
        .AddAspNetIdentity<ApplicationUser>()  
        .AddConfigurationStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .AddOperationalStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .Services.AddTransient<IProfileService, ProfileService>();  
}
```

`services.AddIdentityServer` メソッドを呼び出した後、次を構成するために、追加の fluent Api が呼び出されます。

- 署名に使用される資格情報。
- ユーザーがアクセスを要求する API および id リソース。
- 要求トークンに接続するクライアント。
- ASP.NET Core Id。

> [!TIP]
> IdentityServer 4 の構成を動的に読み込みます。 IdentityServer 4 の Api を使用すると、構成オブジェクトのメモリ内の一覧から、IdentityServer を構成できます。 EShopOnContainers reference アプリケーションでは、これらのメモリ内コレクションはアプリケーションにハードコーディングされています。 ただし、運用環境のシナリオでは、構成ファイルまたはデータベースから動的に読み込むことができます。

ASP.NET Core Id を使用するように IdentityServer を構成する方法の詳細については、IdentityServer のドキュメントの「[ASP.NET Core id の使用](https://identityserver4.readthedocs.io/en/latest/quickstarts/8_aspnet_identity.html)」を参照してください。

#### <a name="configuring-api-resources"></a>API リソースの構成

API リソースを構成する場合、`AddInMemoryApiResources` メソッドには `IEnumerable<ApiResource>` コレクションが必要です。 次のコード例は、eShopOnContainers reference アプリケーションでこのコレクションを提供する `GetApis` メソッドを示しています。

```csharp
public static IEnumerable<ApiResource> GetApis()  
{  
    return new List<ApiResource>  
    {  
        new ApiResource("orders", "Orders Service"),  
        new ApiResource("basket", "Basket Service")  
    };  
}
```

このメソッドは、IdentityServer が注文とバスケットの Api を保護する必要があることを指定します。 そのため、これらの Api を呼び出す場合は、IdentityServer によって管理されるアクセストークンが必要になります。 型の`ApiResource`詳細については、「IdentityServer 4 のドキュメント」の[API リソース](https://identityserver4.readthedocs.io/en/latest/reference/api_resource.html)を参照してください。

#### <a name="configuring-identity-resources"></a>Id リソースの構成

Id リソースを構成する場合、`AddInMemoryIdentityResources` メソッドには `IEnumerable<IdentityResource>` コレクションが必要です。 Id リソースとは、ユーザー ID、名前、電子メールアドレスなどのデータのことです。 各 id リソースには一意の名前が付けられ、任意のクレームの種類を割り当てることができます。これは、ユーザーの id トークンに含まれます。 次のコード例は、eShopOnContainers reference アプリケーションでこのコレクションを提供する `GetResources` メソッドを示しています。

```csharp
public static IEnumerable<IdentityResource> GetResources()  
{  
    return new List<IdentityResource>  
    {  
        new IdentityResources.OpenId(),  
        new IdentityResources.Profile()  
    };  
}
```

OpenID Connect の仕様では、[標準の id リソース](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims)が指定されています。 最小要件は、ユーザーの一意の ID を生成するためのサポートが用意されていることです。 これは、`IdentityResources.OpenId` id リソースを公開することで実現されます。

> [!NOTE]
> `IdentityResources` クラスは、OpenID Connect 仕様で定義されているすべてのスコープ (openid、電子メール、プロファイル、電話、およびアドレス) をサポートします。

また、IdentityServer はカスタム id リソースの定義もサポートしています。 詳細については、「[カスタム id リソースの定義](http://docs.identityserver.io/en/latest/topics/resources.html#defining-custom-identity-resources)」を参照してください。 型の`IdentityResource`詳細については、「IdentityServer 4」ドキュメントの「 [Identity Resource](https://identityserver4.readthedocs.io/en/latest/reference/identity_resource.html) 」を参照してください。

#### <a name="configuring-clients"></a>クライアントの構成

クライアントは、IdentityServer からトークンを要求できるアプリケーションです。 通常、次の設定は、各クライアントに対して最小値として定義する必要があります。

- 一意のクライアント ID。
- トークンサービスとの許可された対話 (許可の種類と呼ばれます)。
- Id トークンとアクセストークンが送信される場所 (リダイレクト URI と呼ばれます)。
- クライアントがアクセスを許可されているリソースの一覧 (スコープと呼ばれます)。

クライアントを構成する場合、`AddInMemoryClients` メソッドには `IEnumerable<Client>` コレクションが必要です。 次のコード例は、このコレクションを eShopOnContainers reference アプリケーションで提供する `GetClients` メソッドでの eShopOnContainers モバイルアプリの構成を示しています。

```csharp
public static IEnumerable<Client> GetClients(Dictionary<string,string> clientsUrl)
{
    return new List<Client>
    {
        ...
        new Client
        {
            ClientId = "xamarin",
            ClientName = "eShop Xamarin OpenId Client",
            AllowedGrantTypes = GrantTypes.Hybrid,
            ClientSecrets =
            {
                new Secret("secret".Sha256())
            },
            RedirectUris = { clientsUrl["Xamarin"] },
            RequireConsent = false,
            RequirePkce = true,
            PostLogoutRedirectUris = { $"{clientsUrl["Xamarin"]}/Account/Redirecting" },
            AllowedCorsOrigins = { "http://eshopxamarin" },
            AllowedScopes = new List<string>
            {
                IdentityServerConstants.StandardScopes.OpenId,
                IdentityServerConstants.StandardScopes.Profile,
                IdentityServerConstants.StandardScopes.OfflineAccess,
                "orders",
                "basket"
            },
            AllowOfflineAccess = true,
            AllowAccessTokensViaBrowser = true
        },
        ...
    };
}
```

この構成では、次のプロパティのデータを指定します。

- `ClientId`: クライアントの一意の ID。
- `ClientName`: クライアントの表示名。ログと同意画面に使用されます。
- `AllowedGrantTypes`: クライアントがどのようにしてサーバーと通信するかを指定します。 詳細について[は、「認証フローの構成](#configuring_the_authentication_flow)」を参照してください。
- `ClientSecrets`: トークンエンドポイントからトークンを要求するときに使用されるクライアントシークレットの資格情報を指定します。
- `RedirectUris`: トークンまたは認証コードを返すために使用できる Uri を指定します。
- `RequireConsent`: 同意画面が必要かどうかを指定します。
- `RequirePkce`: 認証コードを使用するクライアントが証明キーを送信する必要があるかどうかを指定します。
- `PostLogoutRedirectUris`: ログアウト後のリダイレクト先として許可されている Uri を指定します。
- `AllowedCorsOrigins`: クライアントの配信元を指定します。これにより、サーバーが配信元からのクロスオリジン呼び出しを許可できるようになります。
- `AllowedScopes`: クライアントがアクセスできるリソースを指定します。 既定では、クライアントはリソースにアクセスできません。
- `AllowOfflineAccess`: クライアントが更新トークンを要求できるかどうかを指定します。

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>認証フローの構成

クライアントと IdentityServer 間の認証フローは、 `Client.AllowedGrantTypes`プロパティで許可の種類を指定することによって構成できます。 OpenID Connect と OAuth 2.0 仕様では、次のような多数の認証フローが定義されています。

- [Implicit] このフローは、ブラウザーベースのアプリケーション用に最適化されているため、ユーザー認証のみ、または認証とアクセストークン要求のどちらかを使用する必要があります。 すべてのトークンがブラウザーを介して送信されるため、更新トークンなどの高度な機能は使用できません。
- 認証コード。 このフローでは、クライアント認証もサポートしながら、ブラウザーのフロントチャネルとは対照的に、バックチャネルでトークンを取得する機能が提供されます。
- [ハイブリッド] : このフローは、暗黙的および承認のコード許可の種類を組み合わせたものです。 Id トークンは、ブラウザーチャネルを介して送信されます。また、認証コードなどの他の成果物と共に、署名済みのプロトコル応答が含まれています。 応答が正常に検証されたら、バックチャネルを使用してアクセストークンと更新トークンを取得する必要があります。

> [!TIP]
> ハイブリッド認証フローを使用します。 ハイブリッド認証フローは、ブラウザーチャネルに適用される多数の攻撃を軽減します。また、アクセストークン (および場合によってはトークンを更新することもあります) を取得する必要があるネイティブアプリケーションに推奨されるフローです。

認証フローの詳細については、IdentityServer のドキュメントの「[付与の種類](https://identityserver4.readthedocs.io/en/latest/topics/grant_types.html)」を参照してください。

### <a name="performing-authentication"></a>認証の実行

ユーザーの代わりに IdentityServer がトークンを発行するようにするには、ユーザーが IdentityServer にサインインする必要があります。 ただし、IdentityServer は認証用のユーザーインターフェイスまたはデータベースを提供しません。 そのため、eShopOnContainers 参照アプリケーションでは、ASP.NET Core Id がこの目的に使用されます。

EShopOnContainers モバイルアプリは、図9-2 に示すハイブリッド認証フローを使用して、IdentityServer で認証を行います。

![](authentication-and-authorization-images/sign-in.png "High-level overview of the sign-in process")

**図 9-2:** サインインプロセスの大まかな概要

`<base endpoint>:5105/connect/authorize`へのサインイン要求が行われます。 成功した認証の後、IdentityServer は認証コードと id トークンを含む認証応答を返します。 次に、認証コードが `<base endpoint>:5105/connect/token`に送信され、アクセス、id、および更新トークンを使用して応答します。

EShopOnContainers モバイルアプリは、追加のパラメーターを使用してに要求を`<base endpoint>:5105/connect/endsession`送信することによって、IdentityServer からサインアウトします。 サインアウトが発生すると、IdentityServer は、post ログアウトリダイレクト URI をモバイルアプリに送り返すことで応答します。 図9-3 はこのプロセスを示しています。

![](authentication-and-authorization-images/sign-out.png "High-level overview of the sign-out process")

**図 9-3:** サインアウトプロセスの大まかな概要

EShopOnContainers モバイルアプリでは、 `IdentityService` `IIdentityService`インターフェイスを実装するクラスによって、IdentityServer との通信が実行されます。 このインターフェイスは、実装するクラスが `CreateAuthorizationRequest`、`CreateLogoutRequest`、および `GetTokenAsync` メソッドを提供する必要があることを指定します。

#### <a name="signing-in"></a>サインイン

ユーザーが `LoginView`の **[ログイン]** ボタンをタップすると、`LoginViewModel` クラスの `SignInCommand` が実行され、さらに `SignInAsync` メソッドが実行されます。 以下のコード例はこのメソッドを示しています。

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

このメソッドは、次のコード例に示すように、`IdentityService` クラスの `CreateAuthorizationRequest` メソッドを呼び出します。

```csharp
public string CreateAuthorizationRequest()
{
    // Create URI to authorization endpoint
    var authorizeRequest = new AuthorizeRequest(GlobalSetting.Instance.IdentityEndpoint);

    // Dictionary with values for the authorize request
    var dic = new Dictionary<string, string>();
    dic.Add("client_id", GlobalSetting.Instance.ClientId);
    dic.Add("client_secret", GlobalSetting.Instance.ClientSecret); 
    dic.Add("response_type", "code id_token");
    dic.Add("scope", "openid profile basket orders locations marketing offline_access");
    dic.Add("redirect_uri", GlobalSetting.Instance.Callback);
    dic.Add("nonce", Guid.NewGuid().ToString("N"));
    dic.Add("code_challenge", CreateCodeChallenge());
    dic.Add("code_challenge_method", "S256");

    // Add CSRF token to protect against cross-site request forgery attacks.
    var currentCSRFToken = Guid.NewGuid().ToString("N");
    dic.Add("state", currentCSRFToken);

    var authorizeUri = authorizeRequest.Create(dic); 
    return authorizeUri;
}

```

このメソッドは、必要なパラメーターを使用して、IdentityServer の[承認エンドポイント](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html)の URI を作成します。 認証エンドポイントは、ユーザー設定として公開されているベースエンドポイントのポート5105で `/connect/authorize` にあります。 ユーザー設定の詳細については、「[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)」を参照してください。

> [!NOTE]
> EShopOnContainers モバイルアプリの攻撃対象領域を減らすには、OAuth にコード交換 (PKCE) 拡張の証明キーを実装します。 PKCE では、承認コードが傍受された場合に使用されるのを防ぐことができます。 これは、クライアントが秘密検証ツールを生成し、そのハッシュが承認要求で渡され、認証コードの適用時にハッシュされて表示されることによって実現されます。 PKCE の詳細については、インターネット技術標準化委員会の web サイトの「 [OAuth パブリッククライアントによるコード交換のための証明キー](https://tools.ietf.org/html/rfc7636) 」を参照してください。

返された URI は、`LoginViewModel` クラスの `LoginUrl` プロパティに格納されます。 `IsLogin` プロパティが `true`になると、`LoginView` 内の[`WebView`](xref:Xamarin.Forms.WebView)が表示されます。 データ`WebView`は、その[`Source`](xref:Xamarin.Forms.WebView.Source)プロパティ`LoginUrl``LoginUrl`をクラスのプロパティにバインドします。これにより、プロパティが "Identity Server" 認証エンドポイントに設定されている場合に、"IdentityServer にサインイン" 要求が行われます。 `LoginViewModel` ユーザーがこの要求を受信し、ユーザーが認証されていない場合、`WebView` は、図9-4 に示すように、構成されたログインページにリダイレクトされます。

![](authentication-and-authorization-images/login.png "Login page displayed by the WebView")

**図 9-4:** WebView によって表示されるログインページ

ログインが完了すると、 [`WebView`](xref:Xamarin.Forms.WebView)は戻り値の URI にリダイレクトされます。 この `WebView` のナビゲーションにより、`LoginViewModel` クラスの `NavigateAsync` メソッドが実行されます。次のコード例を参照してください。

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    var authResponse = new AuthorizeResponse(url);  
    if (!string.IsNullOrWhiteSpace(authResponse.Code))  
    {  
        var userToken = await _identityService.GetTokenAsync(authResponse.Code);  
        string accessToken = userToken.AccessToken;  

        if (!string.IsNullOrWhiteSpace(accessToken))  
        {  
            Settings.AuthAccessToken = accessToken;  
            Settings.AuthIdToken = authResponse.IdentityToken;  

            await NavigationService.NavigateToAsync<MainViewModel>();  
            await NavigationService.RemoveLastFromBackStackAsync();  
        }  
    }  
    ...  
}
```

このメソッドは、戻り URI に含まれている認証応答を解析します。有効な認証コードが存在する場合は、IdentityServer の[トークンエンドポイント](https://identityserver4.readthedocs.io/en/latest/endpoints/token.html)に対して要求を行い、認証コード、pkce シークレット検証ツール、およびその他の必須パラメーター。 トークンエンドポイントは、ユーザー設定として公開されているベースエンドポイントのポート5105で `/connect/token` にあります。 ユーザー設定の詳細については、「[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)」を参照してください。

> [!TIP]
> リターン Uri を検証します。 EShopOnContainers モバイルアプリでは、リターン URI は検証されませんが、オープンリダイレクト攻撃を防ぐために、戻り値 URI が既知の場所を参照していることを検証することをお勧めします。

トークンエンドポイントは、有効な認証コードと PKCE シークレット検証ツールを受け取ると、アクセストークン、id トークン、および更新トークンで応答します。 (API リソースへのアクセスを許可する) アクセストークンと id トークンは、アプリケーション設定として格納され、ページナビゲーションが実行されます。 そのため、eShopOnContainers モバイルアプリの全体的な影響は次のようになります。ユーザーが IdentityServer の認証に成功した場合は、 `MainView`ページに移動され[`TabbedPage`](xref:Xamarin.Forms.TabbedPage)ます。これ`CatalogView`は、選択したタブとして選択します。

ページナビゲーションの詳細については、「[ナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md)」を参照してください。 [`WebView`](xref:Xamarin.Forms.WebView)ナビゲーションによってビューモデルメソッドが実行される方法については、「[動作を使用したナビゲーションの呼び出し](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors)」を参照してください。 アプリケーション設定の詳細については、「[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)」を参照してください。

> [!NOTE]
> また、eShopOnContainers は、アプリが `SettingsView`でモックサービスを使用するように構成されている場合に、モックサインインを許可します。 このモードでは、アプリは、ユーザーが任意の資格情報を使用してサインインできるようにするために、ユーザーが IdentityServer と通信しません。

#### <a name="signing-out"></a>サインアウト

ユーザーが `ProfileView`の **[ログアウト]** ボタンをタップすると、`ProfileViewModel` クラスの `LogoutCommand` が実行され、さらに `LogoutAsync` メソッドが実行されます。 このメソッドは、`LoginView` ページへのページナビゲーションを実行し、`true` に設定された `LogoutParameter` インスタンスをパラメーターとして渡します。 ページナビゲーション中にパラメーターを渡す方法の詳細については、「[ナビゲーション中のパラメーターの引き渡し](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation)」を参照してください。

ビューを作成してナビゲートすると、ビューに関連付けられたビューモデルの `InitializeAsync` メソッドが実行されます。その後、次のコード例に示すように、`LoginViewModel` クラスの `Logout` メソッドが実行されます。

```csharp
private void Logout()  
{  
    var authIdToken = Settings.AuthIdToken;  
    var logoutRequest = _identityService.CreateLogoutRequest(authIdToken);  

    if (!string.IsNullOrEmpty(logoutRequest))  
    {  
        // Logout  
        LoginUrl = logoutRequest;  
    }  
    ...  
}
```

このメソッドは、`IdentityService` クラスの `CreateLogoutRequest` メソッドを呼び出し、アプリケーション設定から取得された id トークンをパラメーターとして渡します。 アプリケーション設定の詳細については、「[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)」を参照してください。 次のコード例は、`CreateLogoutRequest` メソッドを示しています。

```csharp
public string CreateLogoutRequest(string token)  
{  
    ...  
    return string.Format("{0}?id_token_hint={1}&post_logout_redirect_uri={2}",   
        GlobalSetting.Instance.LogoutEndpoint,  
        token,  
        GlobalSetting.Instance.LogoutCallback);  
}
```

このメソッドは、必要なパラメーターを使用して、IdentityServer の[エンドセッションエンドポイント](https://identityserver4.readthedocs.io/en/latest/endpoints/endsession.html#refendsession)への URI を作成します。 エンドセッションエンドポイントは、ユーザー設定として公開されているベースエンドポイントのポート5105で `/connect/endsession` にあります。 ユーザー設定の詳細については、「[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)」を参照してください。

返された URI は、`LoginViewModel` クラスの `LoginUrl` プロパティに格納されます。 `IsLogin` プロパティが `true`場合、`LoginView` 内の[`WebView`](xref:Xamarin.Forms.WebView)が表示されます。 `WebView` データは、その[`Source`](xref:Xamarin.Forms.WebView.Source)プロパティを `LoginViewModel` クラスの `LoginUrl` プロパティにバインドします。そのため、`LoginUrl` プロパティが [サーバーのエンドセッションエンドポイント] に設定されている場合は、[サインアウト要求] を使用します。 ユーザーがサインインしている場合、ユーザーがこの要求を受信すると、サインアウトが発生します。 認証は、ASP.NET Core からクッキー認証ミドルウェアによって管理される cookie を使用して追跡されます。 このため、サーバーからサインアウトすると、認証クッキーが削除され、ログアウト後のリダイレクト URI がクライアントに返されます。

モバイルアプリでは、 [`WebView`](xref:Xamarin.Forms.WebView)はログアウト後のリダイレクト URI にリダイレクトされます。 この `WebView` のナビゲーションにより、`LoginViewModel` クラスの `NavigateAsync` メソッドが実行されます。次のコード例を参照してください。

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    Settings.AuthAccessToken = string.Empty;  
    Settings.AuthIdToken = string.Empty;  
    IsLogin = false;  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    ...  
}
```

このメソッドは、アプリケーション設定から id トークンとアクセストークンの両方をクリアし、`IsLogin` プロパティを `false`に設定します。これにより、`LoginView` ページの[`WebView`](xref:Xamarin.Forms.WebView)が非表示になります。 最後に、 `LoginUrl`プロパティは、ユーザーが次回サインインを開始するときに備えて、必要なパラメーターを指定して、IdentityServer の[認証エンドポイント](https://identityserver4.readthedocs.io/en/latest/endpoints/authorize.html)の URI に設定されます。

ページナビゲーションの詳細については、「[ナビゲーション](~/xamarin-forms/enterprise-application-patterns/navigation.md)」を参照してください。 [`WebView`](xref:Xamarin.Forms.WebView)ナビゲーションによってビューモデルメソッドが実行される方法については、「[動作を使用したナビゲーションの呼び出し](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors)」を参照してください。 アプリケーション設定の詳細については、「[構成管理](~/xamarin-forms/enterprise-application-patterns/configuration-management.md)」を参照してください。

> [!NOTE]
> また、eShopOnContainers は、SettingsView でモックサービスを使用するようにアプリが構成されている場合に、モックサインアウトを許可します。 このモードでは、アプリは、IdentityServer と通信しません。その代わり、アプリケーション設定から格納されているトークンをクリアします。

<a name="authorization" />

## <a name="authorization"></a>認証

認証後、ASP.NET Core web Api は、多くの場合、アクセスを承認する必要があります。これにより、サービスは、認証された一部のユーザーだけが Api を使用できるようになります。

ASP.NET Core MVC ルートへのアクセスを制限するには、次のコード例に示すように、承認属性をコントローラーまたはアクションに適用します。これにより、コントローラーまたはアクションへのアクセスが認証されたユーザーに制限されます。

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

承認されていないユーザーが `Authorize` 属性でマークされたコントローラーまたはアクションにアクセスしようとすると、MVC フレームワークは 401 (未承認) HTTP 状態コードを返します。

> [!NOTE]
> `Authorize` 属性でパラメーターを指定して、API を特定のユーザーに制限することができます。 詳細については、「[承認](/aspnet/core/security/authorization/introduction/)」を参照してください。

認証ワークフローには、アクセストークンによって制御承認が提供されるように、IdentityServer を統合することができます。 このアプローチを図9-5 に示します。

![](authentication-and-authorization-images/authorization.png "Authorization by access token")

**図 9-5:** アクセストークンによる承認

EShopOnContainers モバイルアプリは、id マイクロサービスと通信し、認証プロセスの一環としてアクセストークンを要求します。 アクセストークンは、アクセス要求の一部として、順序付けおよびバスケットマイクロサービスによって公開される Api に転送されます。 アクセストークンには、クライアントとユーザーに関する情報が含まれています。 Api は、その情報を使用して、データへのアクセスを承認します。 Api を保護するように IdentityServer を構成する方法の詳細については、「 [Api リソースの構成](#configuring-api-resources)」を参照してください。

### <a name="configuring-identityserver-to-perform-authorization"></a>認証を実行するように IdentityServer を構成する

IdentityServer で承認を実行するには、その承認ミドルウェアを web アプリケーションの HTTP 要求パイプラインに追加する必要があります。 ミドルウェアは、`Configure` メソッドから呼び出される web アプリケーションの `Startup` クラスの `ConfigureAuth` メソッドに追加されます。これは、eShopOnContainers reference アプリケーションの次のコード例で示されています。

```csharp
protected virtual void ConfigureAuth(IApplicationBuilder app)  
{  
    var identityUrl = Configuration.GetValue<string>("IdentityUrl");  
    app.UseIdentityServerAuthentication(new IdentityServerAuthenticationOptions  
    {  
        Authority = identityUrl.ToString(),  
        ScopeName = "basket",  
        RequireHttpsMetadata = false  
    });  
} 
```

このメソッドは、有効なアクセストークンを使用してのみ API にアクセスできることを保証します。 ミドルウェアは、受信トークンを検証して信頼された発行者から送信されることを確認し、トークンが受信する API と共に使用できることを検証します。 したがって、注文またはバスケットコントローラーを参照すると、401 (未承認) の HTTP 状態コードが返され、アクセストークンが必要であることが示されます。

> [!NOTE]
> `app.UseMvc()` または `app.UseMvcWithDefaultRoute()` を使用して MVC を追加する前に、IdentityServer の承認ミドルウェアを web アプリケーションの HTTP 要求パイプラインに追加する必要があります。

### <a name="making-access-requests-to-apis"></a>Api へのアクセス要求の作成

注文とバスケットのマイクロサービスに要求を行うときは、認証プロセス中に、次のコード例に示すように、認証プロセス中に、IdentityServer から取得したアクセストークンを要求に含める必要があります。

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

アクセストークンはアプリケーション設定として格納され、プラットフォーム固有のストレージから取得され、`OrderService` クラスの `GetOrderAsync` メソッドの呼び出しに含まれます。

同様に、次のコード例に示すように、IdentityServer によって保護されている API にデータを送信するときに、アクセストークンを含める必要があります。

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

アクセストークンはプラットフォーム固有のストレージから取得され、`BasketService` クラスの `UpdateBasketAsync` メソッドの呼び出しに含まれます。

EShopOnContainers モバイルアプリの `RequestProvider` クラスは、`HttpClient` クラスを使用して、eShopOnContainers 参照アプリケーションによって公開される RESTful Api に要求を行います。 承認が必要な注文とバスケットの Api に対して要求を行う場合は、要求に有効なアクセストークンを含める必要があります。 これは、次のコード例に示すように、アクセストークンを `HttpClient` インスタンスのヘッダーに追加することで実現されます。

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

`HttpClient` クラスの `DefaultRequestHeaders` プロパティは、各要求と共に送信されるヘッダーを公開します。アクセストークンは、文字列 `Bearer`で始まる `Authorization` ヘッダーに追加されます。 要求が RESTful API に送信されると、`Authorization` ヘッダーの値が抽出され検証され、信頼された発行者から送信されていることが確認され、ユーザーがそれを受け取る API を呼び出すためのアクセス許可を持っているかどうかを判断するために使用されます。

EShopOnContainers モバイルアプリが web 要求を作成する方法の詳細については、「[リモートデータへのアクセス](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md)」を参照してください。

## <a name="summary"></a>要約

ASP.NET MVC web アプリケーションと通信する Xamarin. Forms アプリに認証と承認を統合するには、さまざまな方法があります。 EShopOnContainers モバイルアプリは、IdentityServer 4 を使用するコンテナー化された id マイクロサービスで認証と承認を実行します。 IdentityServer は、ASP.NET Core Id と統合してベアラートークン認証を実行する ASP.NET Core 用のオープンソース OpenID Connect および OAuth 2.0 フレームワークです。

モバイルアプリは、ユーザーを認証したり、リソースにアクセスしたりするために、IdentityServer のセキュリティトークンを要求します。 リソースにアクセスする場合は、承認を必要とする Api に対する要求にアクセストークンを含める必要があります。 信頼された発行者から送信されたことを確認するために、IdentityServer のミドルウェアが受信アクセストークンを検証し、それらのトークンを受信する API と共に使用することが有効であることを確認します。

## <a name="related-links"></a>関連リンク

- [電子ブックのダウンロード (2 Mb PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (サンプル)](https://github.com/dotnet-architecture/eShopOnContainers)
