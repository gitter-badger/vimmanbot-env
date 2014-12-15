Vimmanbot + Vagrant (for Vimmanbot development environment)
===========================================================


これは Vimmanbot の開発のために必要なツールを標準で搭載した Vagrant セットアップです。


1. 含まれるパッケージ
---------------------

- CentOS 6.5
- MySQL 5.6
- Nginx 1.7
- uWSGI 2.0.8
- Python 2.7
- Flask 0.10.1


2. サポート環境
---------------

- Mac OS X 10.9
- Mac Homebrew 0.9.5
- VirtualBox 4.3.12
- Vagrant 1.6.5
- Bundler 1.6.9


3. インストール方法
-------------------

サポート環境を満たした上で、下記を実行してください。

構成管理のソースを設置します。  
[https://github.com/OMOSAN/vimmanbot-env](https://github.com/OMOSAN/vimmanbot-env) をフォークし、ソースをクローンします。

    $ git clone git@github.com:{YOUR GITHUB USERNAME}/vimmanbot-env.git
    $ cd vimmanbot-env

必要な Gem をインストールします。

    $ bundle install --path vendor/bundle

アプリケーションのソースを設置します。  
[https://github.com/OMOSAN/vimmanbot-app](https://github.com/OMOSAN/vimmanbot-app) をフォークし、ソースをクローンします。

    $ cd /path/to/vimmanbot-env
    $ git clone git@github.com:{YOUR GITHUB USERNAME}/vimmanbot-app.git src/vimmanbot-app

仮想環境のアドレスを設定します。  
サンプルの環境変数設定をコピーします。

    $ cp .env.sample .env

そのファイルの中の `VIMMANBOT_LOCAL_ENV_PRIVATE_IP` というアドレスを決める環境変数を適当なものに設定します。

    export VIMMANBOT_LOCAL_ENV_PRIVATE_IP='192.168.52.101'

SSH 接続を設定します。

    $ cat files/ssh/config >> ~/.ssh/config

仮想環境を構築します。

    $ source .env && bundle exec vagrant up


4. 環境の資格情報
-----------------

- Host Address: .env で設定したアドレス
- SSH: vagrant / vagrant
- MySQL: root / (none)


5. ホスト側での開発
-------------------

### ソースの自動同期

ホスト側でソースを修正し、ゲスト側に反映するには、反映する度に手動で修正した対象ファイルをゲスト側に配置するか、修正したことを自動で検知し、検知したタイミングでゲスト側に同期する方法があります。

自動でゲスト側に同期するには下記のコマンドを実行してください。

    $ bundle exec vagrant rsync-auto


### アプリのブラウザ経由での動作確認

virtualenv セットを作ります。

    $ bundle exec vagrant rsync-auto
    $ cd /vagrant/src/vimmanbot-app/
    $ mkdir /vagrant/src/vimmanbot-app/src/api/.venv/
    $ virtualenv --python=`which python2.7` /vagrant/src/vimmanbot-app/src/api/.venv/

virtualenv を参照します。

    $ source .venv/bin/activate

Pip 経由でライブラリをインストールします。

    (.venv)$ pip install -r src/api/requirements.txt

データベースを設定します。

    (.venv)$ mysql -u root < src/api/data/sql/mysql_createdb.sql
    (.venv)$ mysql -u root vimmanbot < src/api/data/sql/mysql_schema.sql
    (.venv)$ cp src/api/config/databases.py.sample src/api/config/databases.py

uWSGI 経由でアプリを起動します。

    (.venv)$ uwsgi --ini /vagrant/src/vimmanbot-app/src/api/config/uwsgi/uwsgi-local-api.ini

お使いのブラウザで `www.vimmanbot.local` にアクセスします。  
ex) `http://www.vimmanbot.local/api/questions`


6. その他 Tips
--------------

タスク単体で構成管理に反映したい場合

    $ bundle exec ansible-playbook -i local roles/****/tasks/main.yml


