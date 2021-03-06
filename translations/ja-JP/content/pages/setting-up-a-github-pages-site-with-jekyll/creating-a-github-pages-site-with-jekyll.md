---
title: Jekyll を使用して GitHub Pages サイトを作成する
intro: '新規または既存のリポジトリ内に、{% data variables.product.prodname_pages %} Jekyll を使用してサイトを作成できます。'
product: '{% data reusables.gated-features.pages %}'
redirect_from:
  - /articles/creating-a-github-pages-site-with-jekyll
  - /github/working-with-github-pages/creating-a-github-pages-site-with-jekyll
permissions: 'People with admin permissions for a repository can create a {% data variables.product.prodname_pages %} site with Jekyll.'
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
topics:
  - Pages
shortTitle: Jekyllでのサイトの作成
---

{% data reusables.pages.org-owners-can-restrict-pages-creation %}

## 必要な環境

Jekyll を使用して {% data variables.product.prodname_pages %} サイトを作成する前に、Jekyll と Git をインストールする必要があります。 詳しい情報については、Jekyll ドキュメンテーションの [Installation](https://jekyllrb.com/docs/installation/) および「[Git のセットアップ](/articles/set-up-git)」を参照してください。

{% data reusables.pages.recommend-bundler %}

{% data reusables.pages.jekyll-install-troubleshooting %}

## サイト用にリポジトリを作成する

{% data reusables.pages.new-or-existing-repo %}

{% data reusables.repositories.create_new %}
{% data reusables.repositories.owner-drop-down %}
{% data reusables.pages.create-repo-name %}
{% data reusables.repositories.choose-repo-visibility %}

## サイトを作成する

{% data reusables.pages.must-have-repo-first %}

{% data reusables.pages.private_pages_are_public_warning %}

{% data reusables.command_line.open_the_multi_os_terminal %}
1. リポジトリのローカルコピーがまだない場合、サイトのソースファイルを保存したい場所に移動します。_PARENT-FOLDER_ は、リポジトリを保存したいフォルダの名前に置き換えてください。
  ```shell
  $ cd <em>PARENT-FOLDER</em>
  ```
1. ローカルの Git リポジトリをまだ初期化していない場合は、初期化します。 _REPOSITORY-NAME_ は、リポジトリの名前に置き換えてください。
  ```shell
  $ git init <em>REPOSITORY-NAME</em>
  > Initialized empty Git repository in /Users/octocat/my-site/.git/
  # Creates a new folder on your computer, initialized as a Git repository
  ```
  4. ディレクトリをリポジトリに変更します。
  ```shell
  $ cd <em>REPOSITORY-NAME</em>
  # Changes the working directory
  ```
{% data reusables.pages.decide-publishing-source %}
{% data reusables.pages.navigate-publishing-source %}
  たとえば、デフォルトブランチの `docs` フォルダからサイトを公開することを選択した場合は、ディレクトリを作成して `docs ` フォルダに変更します。
 ```shell
 $ mkdir docs
 # Creates a new folder called docs
 $ cd docs
 ```
 サイトを `gh-pages` ブランチから公開する場合には、`gh-pages` ブランチを作成してチェックアウトします。
 ```shell
 $ git checkout --orphan gh-pages
 # 履歴やコンテンツなしでgh-pagesという新しいブランチを作成、gh-pagesブランチに切り替え
 $ git rm -rf 
 # ワーキングディレクトリでデフォルトブランチからコンテンツを削除
 ```
1. 新しい Jekyll サイトを作成するには、`jekyll new` コマンドを使用します。
   ```shell
   $ jekyll new --skip-bundle .
   # Creates a Jekyll site in the current directory
   ```
1. Jekyll が作成した Gemfile を開きます。
1. `gem "jekyll"` で始まる行の先頭に「#」を追加して行をコメントアウトします。
1. `# gem "github-pages"` で始まる行を編集して `github-pages` を追加します。 行を次のように変更します。

   ```shell
   gem "github-pages", "~> GITHUB-PAGES-VERSION", group: :jekyll_plugins
   ```

   _GITHUB-PAGES-VERSION_ をサポートされている最新バージョンの `github-pages` gem に置き換えます。 このバージョンについては、「[依存関係バージョン](https://pages.github.com/versions/)」を参照してください。

   正しいバージョンの Jekyll は、`github-pages` gem の依存関係としてインストールされます。
1. Gemfile を保存して閉じます。
1. コマンドラインから`bundle install`を実行
1. あるいは、`_config.yml`ファイルに必要な編集を加えてください。 これは、リポジトリがサブディレクトリでホストされている場合に相対パスに対して必要です。  詳しい情報については「[サブフォルダを分割して新しいリポジトリにする](/github/getting-started-with-github/using-git/splitting-a-subfolder-out-into-a-new-repository)」を参照してください。
   ```yml
   domain: my-site.github.io       # HTTPSを強制したいなら、ドメインの先頭でhttpを指定しない。例: example.com
   url: https://my-site.github.io  # サイトのベースのホスト名とプロトコル。例: http://example.com
   baseurl: /REPOSITORY-NAME/      # サイトがサブフォルダで提供されるならフォルダ名を置く
  ```
1. 必要に応じて、サイトをローカルでテストします。 詳しい情報については、「[Jekyll を使用して {% data variables.product.prodname_pages %} サイトをローカルでテストする](/articles/testing-your-github-pages-site-locally-with-jekyll)」を参照してください。
1. 作業内容を追加してコミットしてください。
```shell
git add .
git commit -m 'Initial GitHub pages site with Jekyll'
```
1. {% ifversion ghae %}{% data variables.product.product_name %}{% else %}{% data variables.product.product_location %}{% endif %}でリモートとしてリポジトリを追加してください。{% ifversion ghes or ghae %}_HOSTNAME_をEnterpriseのホスト名で、{% endif %}_USER_をリポジトリを所有するアカウントで、{% ifversion ghes or ghae %}{% endif %}_REPOSITORY_をリポジトリ名で置き換えてください。
```shell
{% ifversion fpt or ghec %}
$ git remote add origin https://github.com/<em>USER</em>/<em>REPOSITORY</em>.git
{% else %}
$ git remote add origin https://<em>HOSTNAME</em>/<em>USER</em>/<em>REPOSITORY</em>.git
{% endif %}
```
1. リポジトリを {% data variables.product.product_name %} にプッシュします。 _BRANCH_ は、作業を行なっているブランチの名前に置き換えてください。
   ```shell
   $ git push -u origin <em>BRANCH</em>
   ```
{% data reusables.pages.configure-publishing-source %}
{% data reusables.pages.navigate-site-repo %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.pages.sidebar-pages %}
{% data reusables.pages.choose-visibility %}
{% data reusables.pages.visit-site %}
{% data reusables.pages.check-workflow-run %}

{% data reusables.pages.admin-must-push %}

## 次のステップ

サイトに新しいページを追加したり、投稿したりするには、「[Jekyll を使用して {% data variables.product.prodname_pages %} サイトにコンテンツを追加する](/articles/adding-content-to-your-github-pages-site-using-jekyll)」を参照してください。

{% data reusables.pages.add-jekyll-theme %}詳しい情報については、「[Jekyll を使用して {% data variables.product.prodname_pages %} サイトにテーマを追加する](/articles/adding-a-theme-to-your-github-pages-site-using-jekyll)」を参照してください。
