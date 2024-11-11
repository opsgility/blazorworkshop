
# エクササイズ 9: プログレッシブウェブアプリ (PWA)

このエクササイズでは、Blazor アプリをプログレッシブウェブアプリ (PWA) に変換し、オフラインで動作する能力を追加します。PWA 機能は、ユーザーがインターネット接続なしでアプリを使用したり、ネイティブアプリのようにデバイスのホーム画面にアプリを追加したりできるようにします。

## タスク 1: PWA サポートの追加

1. `BlazingPizza.Client` プロジェクトの `wwwroot` フォルダに移動し、`manifest.json` ファイルを開きます。このファイルには、アプリが PWA として動作するために必要な設定が含まれています。

2. 次のように `manifest.json` を更新します：

   ```json
   {
       "name": "Blazing Pizza",
       "short_name": "Pizza",
       "start_url": "/",
       "display": "standalone",
       "background_color": "#FFFFFF",
       "theme_color": "#FF5733",
       "description": "Blazing Pizza - A progressive web app sample",
       "icons": [
           {
               "src": "icons/icon-192.png",
               "sizes": "192x192",
               "type": "image/png"
           },
           {
               "src": "icons/icon-512.png",
               "sizes": "512x512",
               "type": "image/png"
           }
       ]
   }
   ```

![PWA Manifest](images/PWAManifest.png)

## タスク 2: サービスワーカーの設定

サービスワーカーは、ネットワーク接続がなくてもアプリを動作させるために必要なキャッシュとリソースの管理を担当します。Blazor PWA テンプレートには、デフォルトのサービスワーカー設定が含まれていますが、`service-worker.js` ファイルをカスタマイズすることでキャッシュ戦略を調整できます。

1. `wwwroot` フォルダの `service-worker.published.js` ファイルを開き、必要に応じてキャッシュルールを変更します。

2. `self.assetsInclude` リストに、オフラインで利用可能にするファイルや URL を追加します。

   ```javascript
   self.assetsInclude = [
       '/',
       'index.html',
       'css/app.css',
       'js/app.js'
   ];
   ```

![Service Worker Config](images/ServiceWorkerConfig.png)

## タスク 3: PWA のテスト

アプリをビルドし、`dotnet run` コマンドでローカルサーバーを起動します。ブラウザでアプリにアクセスし、DevTools の「Application」タブで PWA のインストールやオフラインキャッシュが有効になっていることを確認します。

![PWA Test](images/PWATest.png)

PWA 化された Blazing Pizza アプリをインストールし、オフラインでも使用できることを確認します。
