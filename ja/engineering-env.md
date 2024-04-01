# 情報・通信工学系でエンジニアリング (研究やソフトウェア開発) にまず必要そうなツールについて (2022年度版)

この文書では、情報・通信・計算機工学系 [^1] のソフトウェアエンジニアリングを行うにあたって、まずは入れておく・使えるようになっておくと良さそうなソフトウェアを一覧しておく。細かい使い方、チューニングについては言及しない。また、なるべくエンジニア方言 [^2] を避け、ある程度回りくどい言い回しを使っている。用語を全て網羅することはできないため、よくわからない単語は検索して調べてみてほしい。

[^1]: 物理現象を計算機を使って解析する「計算科学」ではないことを強調しておく。計算科学は、きっともっと特殊なソフトウェアがたくさん必要になる。

[^2]: たとえば「PATHを通す」とかを「環境変数PATHに値を追加しておく」と記載するなど。

あくまで、[弊研究室](https://secarchlab.github.io)や筆者自身の環境を対象としている。どうしてもUnixコマンドを多用する関係上、macOSかDebian GNU/Linux系統のLinuxをOSとして想定している。**Windowsの利用は想定していない**。Windowsでの互換性等、仮想マシン・WSL2などでどうにかする方法は掲載しない。どうにかする方法はきっとあるので、興味があれば調べてほしい。

## パッケージマネージャ

環境を整えるにあたって、何を置いてもまず必要となる。コマンドライン（ターミナル）からソフトウェアのインストール・アンインストール等が効率的にできる。

### macOS

[homebrew](https://brew.sh/)を利用すれば良い。まず速攻で以下でインストールしておく。

```shell
% /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

この記事では、以下のような基本的なコマンドしか利用しない。

* ソフトウェアパッケージ情報の更新: `brew update && brew update --cask`
* ソフトウェアのインストール: `brew install <package_name>`
* インストール済みのソフトウェアのアップデート: `brew upgrade`
* キャッシュの削除: `brew cleanup -s && rm -rf "$(brew --cache)"`

homebrewの細かい使い方は、公式サイトを参照するか、以下のような記事を参照するといい。

> * [【完全版】Homebrewとはなんぞや](https://zenn.dev/sawao/articles/e7e90d43f2c7f9)
> * [【Mac】今さら聞けないHomebrew](https://zenn.dev/nekoniki/articles/8d9a6474c6bc14)
> * [Homebrew使い方まとめ](https://qiita.com/vintersnow/items/fca0be79cdc28bd2f5e4)
> * [Apple SiliconにおけるHomebrewのベストプラクティス](https://qiita.com/yujiod/items/56002a7cef5b5a3be3fb)

### Debian系Linux

OS標準の`apt`を利用すれば良い。`snap`等でもよいが、snapdのソフトウェアはOSから隔絶されるため、知識がある人向け。`apt`の基本的な使い方は以下の通り。

* ソフトウェアパッケージ情報の更新: `sudo apt update`
* ソフトウェアのインストール: `sudo apt install <package_name>`
* インストール済みの全ソフトウェアのアップデート: `sudo apt dist-upgrade`
* アップデート後、古いバージョンのソフトウェアの自動削除: `sudo apt autoremove`

## SSH

SSH (Secure SHell) は、遠隔地のコンピュータと暗号化された通信を行うためのプロトコルである。ターミナルから遠隔地のサーバにログインして操作、などというときは基本的にSSHを利用することになる。

また、後述するGitおよびGitHubを利用するには、SSHの導入が必須となる。そのため、まずはSSHを利用可能とする。ターミナルログインのやり方には今回は触れないので、以下のようなサイトを参照しておくと良い。

> * [SSHでリモートホストに接続する前にやっておくと便利なことは？ ssh-keygenコマンド](https://atmarkit.itmedia.co.jp/ait/articles/1503/20/news007.html)
> * [SSHサーバーにログインするには？ sshコマンド](https://atmarkit.itmedia.co.jp/ait/articles/1503/23/news004.html)

### 環境構築

macOSに元々入っているSSHコマンドは、凝ったことをやろうとすると機能が足りないことがある。このため、[OpenSSH](https://www.openssh.com/)を導入する。

```shell
% brew install openssh
```

Debian系Linuxでは、元から入っていることが多い。そうでなければ`apt`より導入する。

```shell
% sudo apt update && sudo apt install ssh
```

### GitHub向けSSH鍵の設定

後述するGitHubへの登録のために、SSH公開鍵・秘密鍵ペアを作成する。せっかくなのでRSAやECDSAではなく、EdDSA (ED25519) の鍵ペアを生成する。

```shell
% ssh-keygen -t ed25519

Generating public/private ed25519 key pair.
Enter file in which to save the key (~/.ssh/id_ed25519): ~/.ssh/id_ed25519_github
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in ~/.ssh/id_ed25519_github
Your public key has been saved in ~/.ssh/id_ed25519_github.pub
......
```

これにより、GitHub向けに、

* EdDSAの秘密鍵`~/.ssh/id_ed25519_github`
* EdDSAの公開鍵`~/.ssh/id_ed25519_github.pub`

が生成される。これらをGitHubへの接続に利用するため、`~/.ssh/config`に以下を追記する。

```shell:.ssh/config
Host github.com
  HostName      github.com
  IdentityFile  ~/.ssh/id_ed25519_github
  User          git
```

これにより、`github.com`というドメイン名へのSSH接続は、「上記で作成した鍵を用い、ユーザ名`git`による接続」と設定される。

## バージョン管理システムとクラウドサービス

[Wikipedia](https://ja.wikipedia.org/wiki/%E3%83%90%E3%83%BC%E3%82%B8%E3%83%A7%E3%83%B3%E7%AE%A1%E7%90%86%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0)曰く、「バージョン管理システム」とは以下のようなものを指す。

> ソフトウェアソースコード・ドキュメント・画像・音楽など、様々な電子ファイルは段階を経て編集される。編集の過程で履歴を保存しておけば、何度も変更を加えたファイルであっても過去の状態や変更内容を確認したり変更前の状態を復元したりできる。バージョン管理システムの基本的な機能は、このファイルの変更内容・作成変更日時の履歴保管である。
>
> また編集は複数人により同時並行でおこなわれる場合もある（例: 商業的なソフトウェア開発、オープンソースプロジェクト）。複数の人間が複数のファイルを各々編集すると、それぞれのファイルの最新の状態がどれであるか分からなくなったり、同一ファイルに対する変更が競合（コンフリクト）したりするなどの問題が生じやすい。バージョン管理システムはこのような問題を解決する仕組みを提供する。

弊研究室、弊社では、および筆者個人も、**ソフトウェアやLaTeX文書などのあらゆるソースコードについて、Gitを使ってバージョン管理を行い、その作業履歴を記録する**。その目的は、上記の通りである。

### Git

デファクトスタンダードのバージョン管理ソフトウェア。

macOSには標準で入っているが、バージョンが古いためhomebrewよりインストールする。

```shell
% brew install git
```

Gitの使い方は、たとえば以下を参照する。

> *[サル先生のGit入門](https://backlog.com/ja/git-tutorial/)

「リモートリポジトリ」「ローカルリポジトリ」というGitを構成する基本的な概念を理解した後、まずは以下のコマンドが使えるようになることを目指せば良い。

* `git add`: ファイルをバージョン管理対象として登録
* `git commit`: 編集内容のローカルリポジトリへの反映
* `git push`: コミットしたローカルリポジトリの内容を、リモートリポジトリへ反映
* `git pull`: リモートリポジトリから最新の状態を取得し、ローカルリポジトリへ反映
* `git clone`: リモートリポジトリをコピーして、ローカルリポジトリを作成

「ブランチ」などの概念は、上記コマンドが使えるようになった後、チュートリアルサイトなどを読みながら学ぶ。

### GitHub

GitHubは、Gitのバージョン管理情報を保存、Web上で一覧表示可能とするクラウドサービスである。Gitにおけるリモートリポジトリをクラウドサービス化したもの、と言い換えることができる。弊研究室、弊社では、**論文やソフトウェアのソースコードのほとんどをGitHubにて管理し、そこをハブとして共同開発する**。

そのため、アカウントを作成した後は、ターミナルからSSH経由でGitHubから`git clone`、GitHubへ`git push`できるようにする。そのために、前述のように作成したSSH公開鍵`~/.ssh/id_ed25519_github.pub`をGitHubへ登録する。

`~/.ssh/id_ed25519_github.pub`を以下と仮定する。

```shell:~/.ssh/id_ed25519_github.pub
ssh-ed25519 XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX user@localhost
```

[GitHubのSSH公開鍵登録画面](https://github.com/settings/ssh/new)より、上記をコピペしてGitHubへ登録する。登録後は、以下のSSH経由でのテストコマンドが通るかどうかを確かめる。

```shell
% ssh -T git@github.com

Hi XXXX! You’ve successfully authenticated, but GitHub does not provide shell access.
```

## エディタ

### emacs/vim

筆者はテキストエディタとして、emacs/vimをよく利用するが、基本的には好きなものを利用すれば良い。ただし、**vim (vi) の操作はサーバ管理では必須となるため、普段使いはせずとも操作を学んでおくと良い**。

macOSであれば、vimは標準で入っており、homebrewからGUI込みのemacsが導入できる。

```shell
% brew install --cask emacs
```

Debian系Linuxの場合、vim/emacs共々に標準で入っていることが多い。入っていない場合には、両者とも`apt`より取得可能。

```shell
% sudo apt update && sudo apt install vim emacs
```

emacs/vim共々、以下のようなサイトを参考にし、操作を学びつつ自身が使いやすいようにカスタマイズすると良い。

> * [2020年代のEmacs入門
](https://emacs-jp.github.io/tips/emacs-in-2020)
> * [viエディタ入門](https://vim.jp.net/)
> * [筆者のvim設定 (.vimrc)](https://github.com/junkurihara/dotfiles/blob/master/.vimrc)
> * [筆者のemacs設定 (init.el)](https://github.com/junkurihara/dotfiles/blob/master/.emacs.d/init.el)

### 統合開発環境 (Integrated Development Environment; IDE)

以下のどちらかを導入しておけば良いだろう。筆者は、元々はIntelliJ派だったが、VSCodeを使う方が多くなった。

#### Visual Studio Code (VSCode)

macOSであれば、以下でインストールできる。

```shell
% brew install visual-studio-code
```

Debian系Linuxであれば[公式サイト](https://code.visualstudio.com/docs/setup/linux)からdebファイルをダウンロード、以下のように指示通りにインストールを行えばよい。

```shell
% sudo apt install ./<file>.deb
```

VSCodeは、プラグインを使って対応言語や便利機能を拡張していく。たとえばRust ([`rust-analyzer`](https://marketplace.visualstudio.com/items?itemName=matklad.rust-analyzer))やLaTeX対応プラグイン([`LaTeX Workshop`](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop))は非常に出来が良いため、うまく使って効率的な記述を行うと良い。また、環境をGitHubアカウントを経由して複数環境で同期することができる。

#### JetBrains IntelliJ IDEA (PyCharm, WebStorm, GoLand等)

VSCodeと同様のIDE。プラグインで言語対応を拡張していくVSCodeとは違い、言語ごとに別のソフトウェアを利用するのが基本。基本的に有償だが、学生・教職員であれば[教育ライセンス](https://www.jetbrains.com/ja-jp/community/education/#students)が無償で利用できる。

[JetBrains Toolbox](https://www.jetbrains.com/ja-jp/toolbox-app/)をインストールし、そこからWebStorm等を導入する。

macOSはhomebrew経由で入れれば良い。

```shell
% brew install jetbrains-toolbox
```

Linuxは[公式サイト](https://www.jetbrains.com/ja-jp/toolbox-app/)より`tar.gz`をダウンロード、`/opt`あたりへ直接解凍してインストール。(参考: [Install IntelliJ IDEA (公式)](https://www.jetbrains.com/help/idea/installation-guide.html))

```shell
% sudo tar -xzf jetbrains-toolbox-<version_number>.tar.gz -C /opt
```

## コンパイラ・インタプリタ

デバッガやLinterはIDEのプラグインとして入れていくこと。なるべくモダンよりの言語を利用し、CやC++などはよほど必要でない限りは避ける。筆者は、最近はRustをメインで書いており、次点でNode.js(TypeScript/JavaScript)、たまにGo、稀にPython、という感じ。

ここでは、需要が多いであろうRustとNode.js、Pythonの環境構築について触れる。[^3]

[^3]: Goもそのうち追加したい

### Rust

C++代替を目指し、Mozillaが開発を進めるコンパイル言語。高速に動作、かつメモリセーフ[^4]なコードが書きやすい。筆者は最近はネットワークのパイプライン処理を行うコードをよく書くため、Rustをメイン言語として採用している。

[^4]: メモリリークが起きづらい

#### Rustの環境構築

macOS、Linux共々、[公式サイト](https://www.rust-lang.org/ja/tools/install)にある通りに以下を実行する。

```shell
% curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Linuxにおいて`curl`が入っていない場合は`sudo apt update && sudo apt install curl`を実行すること。

Rustのバージョンが上がった場合、以下を実行することで最新バージョンに更新される。

```shell
% rustup update
```

#### Rustの依存パッケージ管理

Rustの開発プロジェクトでは、`cargo`というソフトウェアを使って、依存パッケージの管理を行う。`cargo`はRustの環境を構築すれば同時に導入される。`cargo`で導入されるパッケージは、デフォルトでは[crates.io](https://crates.io/)というリポジトリから取得されるが、GitHubのリポジトリや、ローカルのディレクトリなども指定できる。

以下のようにすると、新規プロジェクトが作成される。

```shell
% pwd
~/some_project

# 新規プロジェクト作成
% cargo init
cargo init
     Created binary (application) package

% ls
Cargo.toml  src
```

ここで、`Cargo.toml`は、依存パッケージを含めた詳細なプロジェクトの情報が記載されたtomlファイル、`src`はソースコードのディレクトリである。依存パッケージを追加するには`Cargo.toml`を手で編集する必要がある。Rust製の外部ツール`cargo-edit`を使うことでコマンドラインから依存パッケージを追加できる。

```shell
# `cargo-edit`をインストール、cargo機能の拡張。
% cargo install cargo-edit

# `clap`という依存パッケージを当該プロジェクトに導入
% cargo add clap
    Updating 'https://github.com/rust-lang/crates.io-index' index
      Adding clap v3.0.14 to dependencies
```

結果、`Cargo.toml`は以下のようになる。

```toml:Cargo.toml
[package]
name = "some_project"
version = "0.1.0"
edition = "2021"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]
clap = "3.0.14"
```

`cargo install`でインストールされたパッケージは、プロジェクト単位ではなく、`apt`や`homebrew`で入れたソフトウェア同様に環境全体で利用可能となる。

またRustでは、`Cargo.toml`に記載された依存パッケージについて、プロジェクトのコンパイル時に基本的にはそのソースコードを取得、全てをコンパイルする。取得したソースコード、コンパイルの中間ファイル、実行ファイルなどは`./target`にキャッシュされる。**`./target`はGitに追加しないように`.gitignore`などに記載しておくこと。**

### Node.js

[Node.js](https://nodejs.org/)は、本来はフロントエンド (Webブラウザ) 上のスクリプト言語であるJavaScriptを、バックエンド・サーバサイドでも実行可能とするために作られた「JavaScript実行エンジン」である。しかしながら現在のWeb開発では、むしろフロントエンドの開発のために必須となっている。というよりも、Node.js標準のパッケージマネージャ`npm`とその標準パッケージリポジトリ[`npmjs.com`](https://npmjs.com)が、膨大な資産として必要不可欠である。もちろんそれらを用いてバックエンド構築もできる。

#### Node.jsの環境構築

Node.jsでは、LTS (Long Term Support;長期サポート版) と、latest (最新版) の2種類のリリースをおこなっている。LTSは30ヶ月のバグフィックスが保証されている。どちらを選んでも構わないが、最新版はたまに破壊的変更が入ることもあるので、ひとまずLTSを使っておく方が無難。

以下、macOS/Debian系Linux共々に`n`というソフトウェアを利用して、Node.js LTSを導入する手順である。

* macOS

  ```shell
  % brew install n
  % sudo n lts # Node.jsの安定版がインストールされる
  ```

* Debian系Linux

  ```shell
  % sudo apt -y nodejs npm
  % sudo npm i -g n # npmを経由してnをインストール
  % sudo n lts # Node.jsの安定版がインストールされる
  % sudo apt purge -y nodejs npm # aptで入れたものはもう使わないので削除
  ```

#### パッケージマネージャは`npm`代替の`yarn`を導入

Node.js標準の`npm`でもいいのだが、互換となる[`yarn`](https://yarnpkg.com/)の方がパッケージマネージャとして高速なため、`yarn`を追加導入する。

```shell
% sudo npm -i g yarn
```

`npm`/`yarn`共々に、開発プロジェクトのディレクトリ直下にパッケージをインストールし、OSの環境全体には影響しない。以下のようにすると、「新規プロジェクトの作成」「プロジェクトへの依存パッケージの導入」が可能になる。このとき、依存パッケージは[npmjs.com](https://npmjs.com/)から取得される。

```shell
% pwd
~/some_project

# 新規プロジェクト作成
% yarn init
question name (test): xxx
question version (1.0.0): xxx
......

# `webpack`というパッケージの追加
% yarn add webpack

% ls
node_modules  package.json  yarn.lock
```

ここで、`node_modules`は依存パッケージがインストールされたディレクトリ、`package.json`は依存パッケージを含めた詳細なプロジェクトの情報が記載されたjsonファイル、`yarn.lock`は依存パッケージのバージョン情報を記載する。**`node_modules`はGitに追加しないように`.gitignore`などに記載しておくこと。**

`yarn`で環境全体に影響させたパッケージインストールは以下。

```shell
% yarn global add <package_name>
```

のようにすると`~/.yarn/bin`以下などにパッケージがインストールされ、プロジェクトに関係なく利用可能となる。利用時は環境変数`$PATH`にインストール場所を追加すること。Lintingのパッケージ(`eslint`)など、プロジェクト非依存に利用しそうなものなどは、グローバルに入れても良いかもしれない。筆者はあまり利用しない。

`yarn`の細かいコマンドは[公式サイト](https://yarnpkg.com/)を参照すること。

### Python

ネットワークスタックを弄ったり、速度を出す演算処理を求めたりしなければ、豊富なライブラリが揃っているのでおそらく一番使いやすい言語。macOS、Ubuntu共々に元々入っていると思うが、OS標準のPythonインタプリタは古いことが多い。パッケージマネージャを利用して新しいものを導入すると良い。

また、**jupyter notebookやGoogle Colaboratoryはデータの中身を解析を目的としてPythonを用いるものであり、Pythonをエンジンとして動作するソフトウェアを開発するためのものでは無い**。ノートブックファイルの先頭から実行した場合と途中から実行した場合にデータが変化すること[^5]、巨大なパッケージのインストールが必要なこと、などが理由である。データ解析や、コードの動作をとりあえず確認するために用いるのには、とてもお手軽で良い。しかし、**ソフトウェアを開発する場合は、開発環境としてjupyter notebookを使ってはいけない**。外部の依存パッケージなどの管理を行うことを含めて、環境から整える。

[^5]: 冪等性が保証されない。

#### Pythonの環境構築

* macOS

  OS標準でもpythonが利用可能だが、なるべく新しいものを利用するためhomebrewを利用する。`python`パッケージをインストールすると、2022年2月8日現在ではpython 3.9.10がインストールされる。

  ```shell
  % brew install python

  % python --version
  Python 3.9.10
  ```

  python 3.10系を入れるなら以下の通り。

  ```shell
  % brew install python@3.10
  ```

  デフォルトのままだと`python3`や`pip3`コマンドでないとインストールしたバイナリが呼べないため、`~/.zshenv`か`~/.zshrc`あたりにエイリアスを記述して`python`と`pip`コマンドで呼べるようにする。

  ```shell:~/.zshenv
  alias pip='pip3'
  alias python='python3'
  ```

* Debian系Linux

  Ubuntu 20.04LTSだと`python`コマンドは python 2系を参照しているか、パスが通っていない。以下を実行してpythonコマンドからpython 3系を利用する。

  ```shell:
  % python --version
  Python 2.7.18

  % sudo apt install python-is-python3

  % python --version
  Python 3.8.10
  ```

#### `venv`を用いたPythonコードの開発

Pythonによるソフトウェア開発では、パッケージマネージャ`pip`を用いて、依存パッケージを管理する。`pip`は、基本[pypi.org](https://pypi.org/)から取得される。

ただし、何も考えず`pip`を利用すると、ユーザの環境全体に影響する箇所に依存パッケージがインストールされてしまうため、**開発ソフトウェア単位で依存パッケージを管理する**`venv`を利用する。

以下で、開発プロジェクトのディレクトリ以下に`venv`を導入する。`./venv`ディレクトリが作成される。

```shell
% pwd
/path/to/development_project_dir

% python -m venv venv

% ls
venv
```

`venv`を有効化する。この状態で`pip`を用いてパッケージをインストールすると、`./venv`以下にインストールされ、当該の開発プロジェクトのみで利用可能となる。

```shell
% source ./venv/bin/activate

(venv) % pip install --upgrade pip numpy
```

インストール済みの依存パッケージは、以下でそのバージョンも含めて記録しておく。

```shell
(venv) % pip freeze > requirements.txt
```

```shell:requirements.txt
numpy==1.22.2
```

`requirements.txt`を利用することで、別のPCに移動して作業する場合や、共同開発プロジェクトで他メンバに追加導入されたパッケージの反映が楽になる。

```shell
(venv) $ pip install -r requirements.txt
```

一方、このままだと、他プロジェクトのディレクトリに移動して`pip`で依存パッケージを導入すると、元のプロジェクトの`venv`ディレクトリに記録されてしまう。以下のように`venv`を無効化し、開発プロジェクトを移ってから再度そのディレクトリで`venv`を有効化する。あるいは別のIDEやシェルを立ち上げて、移動先プロジェクトの`venv`を有効化する。

```shell
(venv) % deactivate
```

また、**`./venv`ディレクトリはGitに登録しないこと。** `.gitignore`に`venv/`と記載しておくと良い。

## 文書作成

**基本LaTeXかMarkdown**。

### LaTeX

何も考えず[TeXLive](https://www.tug.org/texlive/)を利用する。

#### 環境構築

* macOS

  homebrew経由でTeXLiveのmacOS向けパッケージ「MacTeX」を導入する。筆者はGUI環境が不要なので`mactex-no-gui`を導入している。BibTex向けに`BibDesk`あたりも導入しておくと良い。

  ```shell
  % brew install mactex-no-gui bibdesk
  ```

* Debian系Linux

  `apt`で入れると古いTeXLiveディストリビューションが導入されてしまう。このため、[公式サイト](https://www.tug.org/texlive/quickinstall.html)に従ってインストールする。[`install-tl.zip`](https://www.tug.org/texlive/acquire-netinstall.html)をダウンロード、解凍したあと、以下を実行する。

  ```shell
  % cd <path_to_unpacked_dir>

  % perl install-tl
  ```

  ないとは思うが`perl`が入ってない場合は`apt`から入れること。

#### TeXのエディタ

エディタに以下のようなプラグインを導入して、効率的なTeXコンパイル (タイプセット) などを可能にする。

* emacsを使うならば [YaTeX](https://www.yatex.org/)
* VSCodeを使うならば [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop)

以下など参考にすれば良さそう。

> * [Emacsで快適な研究環境を整えよう3〜YaTeX + RefTeX 編〜
](https://qiita.com/Maizu/items/7d8f420817dfeccf4477)
> * [TeXはVSCodeとLaTeX Workshopで書け
](https://kajindowsxp.com/texworkshop/)
> * [筆者のinit.el (YaTeXの設定)](https://github.com/junkurihara/dotfiles/blob/master/.emacs.d/init.el)

#### スライド作成

`beamer`を利用する。TeXLiveに導入されているので、以下のテンプレをクローン、自分でデザインなどをカスタマイズして利用すると良い。

* [筆者作成日本語スライドテンプレ](https://github.com/junkurihara/latex-skeleton/tree/master/slides-ja)
* [筆者作成英語スライドテンプレ](https://github.com/junkurihara/latex-skeleton/tree/master/slides)

数式がまともに書けるならKeynoteなどを使っても良い。

### Markdown

Markdownについては、VSCodeのMarkdownプラグインをうまく利用する。あるいはMarkdownエディタを探して利用する。参考までに筆者が入れているVSCodeのMarkdown関連プラグインは以下の通り。

* [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one): キーボードショートカットやTOCの表示など必要機能を大体網羅してくれる
* [GitHub Markdown Preview](https://marketplace.visualstudio.com/items?itemName=bierner.markdown-preview-github-styles): GitHubのMarkdown方言に合わせて表示
* [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint): 文法ミスや記述の汚さを指摘してくれる

## 情報管理・検索

### 文書管理

紙で印刷してクリアファイルで管理しても良いが、[Mendeley](https://www.mendeley.com/)や[Zotero](https://www.zotero.org/)などを使う。先人の知恵を探し、Dropboxなどを組み合わせた文献管理手法など、自分に合った効率的な手法を考えるとよい。

### 情報検索

以下のようなサービスを利用するとよい。

#### 論文

* [Google Scholar](https://scholar.google.com/)
* [Mendeley](https://www.mendeley.com/)
* [ResearchGate](https://www.researchgate.net/)
* [Arxiv](https://arxiv.org/)
* [IEEE Xplore](https://ieeexplore.ieee.org/Xplore/home.jsp)
* [ACM Digital Library](https://dl.acm.org/)

文献が探せても、PDFがDLできるとは限らないことに注意。

#### 技術情報

**ネット上の情報は玉石混交なので、なるべくまずは公式ドキュメントを読むこと**。特に、Google検索すると出てくる「〇〇エンジ〇ア塾」とか煽り文句が「現役エンジニアが教える〇〇」のところ、とかも信用してはいけない。たとえば以下のようなものもあくまでまずは参考情報とし、裏取りなどをするとより良い。

* [Qiita](https://qiita.com)
* [Zenn](https://zenn.dev/)
* [StackOverflow](https://stackoverflow.com/)

その他、[検索エン](https://www.google.com)[ジンを](https://duckduckgo.com/)[使いこ](https://www.startpage.com/)[なす](https://brave.com/search/)のが重要。
