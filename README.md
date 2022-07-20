# Rails6 + Vue  

## Railsセットアップ

### 1.Railsインストール  
```
docker-compose run rails rails new . --force --database=mysql --skip-turbolinks --api

```

### 2.Gemfileの修正・CORSの設定  
```./rails/Gemfile
... 省略  
# Use Rack CORS for handling Cross-Origin Resource Sharing (CORS), making cross-origin AJAX possible  
// ↓コメントアウトを外す
gem 'rack-cors'
```
修正したら下記コマンドを実行
```
docker-compose run rails bundle install
```
次に、「cors.rb」を修正する
```/rails/src/config/initializers/cors.rb
Rails.application.config.middleware.insert_before 0, Rack::Cors do
    allow do
        origins '*'

        resource '*',
        headers: :any,
        expose: ['access-token', 'expiry', 'token-type', 'uid', 'client'],
        methods: [:get, :post, :put, :patch, :delete, :options, :head]
    end
end
```

### 3.database.ymlの修正  
```/rails/src/config/database.yml
... 省略
default: &default
  adapter: mysql2
  encoding: utf8mb4
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  username: root
  password: password
  host: db
... 省略
```

### 4. Dockerイメージの作成  
```
docker-compose build
```

### 5. DBの作成  
```
docker-compose run rails rails db:create
```

### 6. Railsの動作確認  
http://localhost:3000/  


## Vueセットアップ  
### 1. Vue3プロジェクトのインストール  
```
docker exec -it vue /bin/bash
```
コンテナに入れたらVue3をインストール  
```
vue create .
```

### 2. Vueの動作確認  
```
npm run serve
```
成功したら以下のURLにアクセス  
http://localhost:8080/  