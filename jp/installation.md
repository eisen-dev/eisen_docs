# eisenとvagrantのセットアップ(Windows)
仮想環境構築ツールのvagrantを利用して開発版eisenをセットアップする方法をご紹介します。  
##準備
* Oracle VM Virtualboxのダウンロード/セットアップ  
vagrantを利用する前提としてOracle VM Virtualboxのインストールが必要です。  
すでにインストールされている場合はそのまま利用できます。

* vagrantのダウンロード/セットアップ  
https://www.vagrantup.com/  
上記リンクからvagrant WINDOWS Universal (32 and 64-bit)をダウンロード、インストールしてください。  
また、以下に説明するvagrantコマンドの操作はWindowsのコマンドプロンプトで実行できます。

## eisen_frontのセットアップ
eisen_frontディレクトリに移動し、以下の操作を実行してください。  
1. eisen_frontの実行に必要なUbuntu12.04 64bitのboxファイルを追加する  
`vagrant box add precise64 http://files.vagrantup.com/precise64.box`  
2. 仮想マシンの起動  
`vagrant up`  
初回のみvagrantfileを元にeisen_frontを仮想マシン上に自動でセットアップします。
終了後仮想マシンが利用できます。  

## eisen_agentのセットアップ
eisen_agentディレクトリに移動し、以下の操作を実行してください。  
1. eisen_frontの実行に必要なUbuntu12.04 64bitのboxファイルを追加する  
`vagrant box add precise64 http://files.vagrantup.com/precise64.box`   
2. 仮想マシンの起動  
`vagrant up`  
eisen_frontendと同様に仮想マシンのセットアップが始まります。  
終了後仮想マシンが利用できます。  

## トラブルシューティング
* eisen_frontをvagrant upでポート3306が閉じていると言われ起動できない  
MySQLのデフォルトポート3306と競合している可能性があります。  
Mysqlを一時的に停止してから再試行してみてください。

## その他
* 仮想マシンの起動状態やログはOracle VM VirtualboxのGUIで確認できます。

* vagrantのヘルプ  
`vagrant --help`

* vagrantで作成した仮想マシンを操作する場合はTeratermやputtyなどのSSHクライアントソフトを利用できます。

## 各マシンへの接続情報
デフォルトでは以下の設定で仮想マシンが起動します。  
* eisen_front  
SSH address: 127.0.0.1:2222  
SSH username: vagrant  
SSH password: vagrant  
SSH auth method: private key  

* eisen_agent
SSH address: 127.0.0.1:2200  
SSH username: vagrant  
SSH password: vagrant  
SSH auth method: private key  

## eisenの初期設定
http://localhost:8080/init-setup1.phpへ移動し、初期設定を行ってください。
* 接続設定  
データベース設定  
IPアドレス： localhost  
ユーザー名：root  
パスワード：password  
データベース名：eisen  
・ユーザー設定  
お好きなユーザーを作成してください  

