
# エクササイズ 7: JavaScript インターオペレーション

Blazor アプリは多くのタスクを単独で実行できますが、時にはブラウザ内で既に存在する JavaScript 機能を活用する必要があります。このエクササイズでは、Blazor の JavaScript インターオペレーション（JS インターオプ）機能を使用して、JavaScript コードとやり取りする方法を学びます。

## タスク 1: JavaScript 関数の呼び出し

Blazor から JavaScript 関数を呼び出すには、`IJSRuntime` サービスを使用します。このサービスは、JavaScript 関数を呼び出すためのメソッド `InvokeAsync` を提供します。

まず、JavaScript ファイルを `wwwroot` フォルダに追加します。これを行うには、`wwwroot` フォルダを右クリックし、「新しいファイル」を選択します。ファイル名を `interop.js` に設定し、次の内容を追加します：

```javascript
window.showAlert = (message) => {
    alert(message);
};
```

![Add JS file](images/AddJSFile.png)

次に、`Pages/Index.razor` で `IJSRuntime` をインジェクトし、ボタンのクリック時に JavaScript 関数を呼び出すコードを追加します。

```razor
@inject IJSRuntime JS

<h3>JavaScript Interop Example</h3>

<button @onclick="ShowAlert">Show Alert</button>

@code {
    async Task ShowAlert() {
        await JS.InvokeAsync<object>("showAlert", "Hello from Blazor!");
    }
}
```

アプリを実行し、「Show Alert」ボタンをクリックすると、JavaScript アラートが表示されます。

![Alert example](images/AlertExample.png)

## タスク 2: JavaScript から .NET メソッドを呼び出す

JavaScript から Blazor の .NET メソッドを呼び出すことも可能です。この機能は、Blazor コンポーネントがブラウザ内のイベントに応答する必要がある場合に便利です。

1. `interop.js` に次の関数を追加します：

    ```javascript
    window.callDotNetMethod = () => {
        DotNet.invokeMethodAsync('BlazingPizza.Client', 'ShowDotNetAlert');
    };
    ```

2. 次に、`Index.razor` ファイルで `ShowDotNetAlert` メソッドを静的メソッドとして定義し、`JSInvokable` 属性を付与します：

    ```razor
    @code {
        [JSInvokable]
        public static Task ShowDotNetAlert() {
            Console.WriteLine("Hello from .NET!");
            return Task.CompletedTask;
        }
    }
    ```

![JS to .NET Example](images/JSInvokeExample.png)

これで、JavaScript コードから `.NET` メソッドを呼び出せるようになり、コンソールにメッセージが表示されます。
