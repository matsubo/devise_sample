devise sample
============================

- deviseの公式の手順書が分かりづらいので、シンプルな認証を実現するための要点を整理してみました！
- 細かい設定とか、自分のサイトにあった設定はこれを元にオプションを変更していただければと思います。
- 1手順ごとにコミットを分けてあるので、変更点が明確に分かるようになっています。
  - 一部、手順が前後しても大丈夫なところがあるので間あり神経質にならなくても大丈夫です。


railsアプリケーション作成
-------------------

```
% rails new devise_smaple
```

scaffoldで適当なページを作る
-------------------
```
% rails generate scaffold Post name:string title:string content:text
```

テーブルを作成しておく
-------------------
```
% rake db:migrate
```

deviseモジュールをインストール
-------------------
```
% echo "gem 'devise'" >> Gemfile
% bundle install
```


deviseの設定をインストール
-------------------
```
% rails generate devise:install
```
これを実行すると、いろいろ設定する事項が表示されるので次のように1つずつ設定していく。

デフォルトのリダイレクト先を追加
-------------------

```
vi config/environments/development.rb
```

以下の行を追加
```diff
  + config.action_mailer.default_url_options = { :host => 'localhost:3000' }
```


トップページがPostの一覧になるように設定
-------------------

rootを以下のように設定
```diff
  + root :to => 'posts#index'
```


メッセージ表示領域を追加
-------------------

```
% vi app/views/layouts/application.html.erb
```


```diff
+    <p class="notice"><%= notice %></p>
+    <p class="alert"><%= alert %></p>
```




herokuを使っている場合にはこの設定を行う
-------------------

```
% vi config/application.rb
```


```diff
     +    # devise setting for heroku
     +    config.assets.initialize_on_precompile = false
```


deviseで追加されるページのviewだけを作成
-------------------
```
% rails g devise:views users
```


デフォルトのトップページを削除
-------------------
```
% rm public/index.html
```

deviseのmodelやrouteを追加する
-------------------

```
% rails generate devise users
```


devise用のテーブルを作成する
-------------------
```
% rake db:migrate
```


アクセスを制限したいコントローラに以下を追加する
-------------------

```
% vi app/controllers/posts_controller.rb
```

```diff
+ before_filter :authenticate_user!
```


動作テスト
-------------------
- 開いてみると、ログイン画面が表示されるはず。
  - http://127.0.0.1/

![screen shot](https://www.evernote.com/shard/s23/sh/80f9d0b0-ef20-4c0f-87be-c71e43791287/aee3d51c1d96cc2959bef0c7fea4a3c8/res/e7a47a82-dc1d-4ccd-9f98-4426fdd3e086/skitch.png?resizeSmall&width=832 "Screenshot")



