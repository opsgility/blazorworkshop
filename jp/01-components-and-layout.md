# エクササイズ 1: コンポーネントとレイアウト

このエクササイズでは、Blazor を使用してピザ注文アプリを作成します。このアプリを使って、ユーザーがピザを注文したりカスタマイズしたり、配達を追跡できるようになります。

## タスク 1: サンプルアプリのダウンロードと実行

1. コンピューターに「BlazingPizza」という名前の新しいフォルダーを作成し、そのフォルダーに[サンプルアプリ](https://github.com/opsgility/blazorworkshop/releases/latest)をダウンロードして解凍します。
2. 「Visual Studio 2022」でソリューションを開きます。このソリューションには次の 4 つのプロジェクトが含まれています。
   - `BlazingPizza.Client`: Blazor プロジェクトで、アプリの UI コンポーネントが含まれます。
   - `BlazingPizza.Server`: Blazor アプリをホストする ASP.NET Core プロジェクトで、アプリのバックエンドサービスも含まれます。
   - `BlazingPizza.Shared`: アプリの共通モデルタイプが含まれるプロジェクトです。
   - `BlazingPizza.ComponentsLibrary`: これは後のセッションで使用されるコンポーネントとヘルパーコードのライブラリです。
3. ソリューションを実行し、Webブラウザで開くと、次のようなシンプルなホームページが表示されます。

   ![アプリの初期画面](images/initial-app-screen.png)

## タスク 2: ピザスペシャルのリストを表示する

次に、ホームページに利用可能なピザスペシャルのリストを表示するように更新します。

1. `BlazingPizza.Client/Pages` フォルダ内の `Index.razor` ファイルを開きます。
2. `@code` ブロックに、利用可能なスペシャルを追跡するリストフィールドを追加します。

   ```razor
   @code {
       List<PizzaSpecial> specials;
   }
Blazor では、依存性注入によって構成済みの HttpClient が提供されており、適切なベースアドレスが設定されています。@inject ディレクティブを使用して、HttpClient を Index コンポーネントに注入します。

razor
Copy code
@inject HttpClient HttpClient
コンポーネントが初期化されると、OnInitializedAsync メソッドをオーバーライドして、ピザスペシャルのリストを取得します。

razor
Copy code
@code {
    List<PizzaSpecial> specials;

    protected override async Task OnInitializedAsync()
    {
        specials = await HttpClient.GetFromJsonAsync<List<PizzaSpecial>>("specials");
    }
}
ピザスペシャルのリストが読み込まれた後、それを Index.razor ファイルのマークアップに表示するように更新します。

razor
Copy code
@if (specials == null)
{
    <p><em>Loading...</em></p>
}
else
{
    <ul>
        @foreach (var special in specials)
        {
            <li>@special.Name - @special.BasePrice</li>
        }
    </ul>
}
アプリを保存して再起動すると、利用可能なピザのリストがホームページに表示されるはずです。



タスク 3: ピザのカードを作成する
データリストの表示は機能しますが、もう少し魅力的な表示にしてみましょう。利用可能な各ピザをカードとして表示します。

BlazingPizza.Client/Shared フォルダに PizzaCard.razor という新しいファイルを作成します。

PizzaCard コンポーネントのマークアップに、ピザ情報の表示レイアウトを作成します。

razor
Copy code
@typeparam TItem

<div class="card">
    <img src="@item.ImageUrl" class="card-img-top" alt="@item.Name" />
    <div class="card-body">
        <h5 class="card-title">@item.Name</h5>
        <p class="card-text">@item.Description</p>
        <p class="card-price">@item.BasePrice</p>
    </div>
</div>


これで、各ピザがカードとして表示され、より視覚的に魅力的になります。