# getstart-ansible
- この サンプル Ansible プレイブック は、以下のドキュメントを参考に作成したものです。
  - https://learn.microsoft.com/ja-jp/azure/developer/ansible/vm-configure-windows?tabs=ansible#complete-sample-ansible-playbook
- これはデモ用のコードであり、実環境での利用を想定していません。


## 説明
2つのプレイブックはそれぞれ以下の処理を行います。

- create_azure_vm.yml
リソースグループ、仮想ネットワーク、仮想マシン等 の Azure リソースを作成します。
仮想マシン は Windows Server 2019 Datacenter で、Ansible で管理するために WinRM を有効化します。

- domain.yml
ドメインコントローラー の役割をインストールし、新規フォレストを作成します。


## 使い方
Azure CLI (Bash) を起動します。
このリポジトリをクローンします。

> git clone https://github.com/uedait/getstart-ansible.git

getstart-ansible ディレクトリへ移動し、Azure リソース作成用のプレイブックを実行して仮想マシン等を作成します。

> $ ansible-playbook create_azure_vm.yml

プレイブックを実行すると、仮想マシンのローカル管理者のパスワードを設定するよう求められますので、任意の値を設定してください。なお、ロカール管理者ユーザーは azureuser です。
プレイブックの実行中、パブリックIPアドレスのリソースが作成され、パブリックIPアドレスが表示されますので、これを控えます。

> PLAY [provision new azure host]
> .... 略 ....
> TASK [Output public IP]
> ok: [localhost] => {
>   "msg": "The public IP is 20.89.105.16"
> }
> .... 略 ....

仮想マシンが作成され、WinRM の構成が終了したら、控えたパブリックIPアドレスを指定して、ドメインコントローラ構成用のプレイブックを実行します。

> $ ansible-playbook -i 20.89.105.16, domain.yml

ドメインコントローラとして構成されたら、RDP で接続して確認してください。
ユーザーは azureuser、パスワードは設定を頂いたものです。
