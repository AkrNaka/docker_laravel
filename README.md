## 手順

### 0. 最終的な階層構造
- `docker_laravel/`
    - `source/ ←別リポジトリからクローン`
        - `app/`
        - `bootstrap/`
        - `config/`
        - `database 等laravelのプロジェクトファイル類`
    - `docker/`
      - `nginx`
        - `nginx.conf`
      - `php`
        - `Dockerfile`
    - `docker-compose.yml`
    - `README.md`

### 1. リポジトリのクローン

ソースのリポジトリ　https://github.com/AkrNaka/source_laravel.git<br>
クローン時のディレクトリ名は　source

```sh
git clone https://github.com/AkrNaka/source_laravel.git source
```

### 2. コンテナのビルド

下記コマンドで、コンテナをビルドし起動する

```sh
docker-compose build
docker-compose up -d
```

### 3. composerのインストール

phpのコンテナ内で、composerのインストールを実行する
```sh
composer install
```

### 4. .envファイル生成

下記手順で.envファイルを設定<br>
(a,b:ホストマシン側で操作、c:phpコンテナ内で操作)

#### a. .env.exampleをコピー
```sh
cd source #ソースリポジトリのクローン先
cp .env.example .env
```

#### b. DB設定の書き換え

.envの22-27行目を下記に書き換え
```
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=mydatabase
DB_USERNAME=user
DB_PASSWORD=password
```

#### c. APP_KEYの設定

```sh
php artisan key:generate
```

### 5. マイグレーションの実行

phpコンテナ内で実行
```sh
php artisan migrate
```

### 6. 動作確認

下記URLにアクセスし、laravelのページが表示されることを確認<br>
http://localhost<br>
下記URLへアクセスし、todoで追加できることを確認<br>
http://localhost:3000/todo