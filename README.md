Vimmanbot + Vagrant (for Vimmanbot development environment)
===========================================================


これは Vimmanbot の開発のために必要なツールを標準で搭載した Vagrant セットアップです。


1. 含まれるパッケージ
---------------------

- CentOS 6.5
- MySQL 5.6
- Nginx 1.7
- uWSGI
- Python 2.7
- Flask 


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


### 3.1. ソースをクローンする

    $ git clone xxxxxxxx
    $ cd xxxxxxxx


### 3.2. 必要な Gem をインストールする

    $ bundle install --path vendor/bundle


### 3.3. 仮想環境を構築する

    $ source .env && bundle exec vagrant up


4. 環境の資格情報
-----------------

- Host Address: .env で設定したアドレス
- SSH: vagrant / vagrant
- MySQL: root / (none)


5. ホスト側での開発
-------------------

ホスト側でソースを修正し、ゲスト側に反映するには、反映する度に手動で修正した対象ファイルをゲスト側に配置するか、修正したことを自動で検知し、検知したタイミングでゲスト側に同期する方法があります。

自動でゲスト側に同期するには下記のコマンドを実行してください。

    $ bundle exec vagrant rsync-auto


6. その他 Tips
--------------

タスク単体で構成管理に反映したい場合

    $ bundle exec ansible-playbook -i local roles/****/tasks/main.yml


