---
title: 必須ステータスチェックのトラブルシューティング
intro: ステータスチェック必須を使用して、一般的なエラーを調べ、問題を解決できます。
product: '{% data reusables.gated-features.protected-branches %}'
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
topics:
  - Repositories
redirect_from:
  - /github/administering-a-repository/troubleshooting-required-status-checks
  - /github/administering-a-repository/defining-the-mergeability-of-pull-requests/troubleshooting-required-status-checks
shortTitle: Required status checks
---

同じ名前のチェックとステータスがあり、その名前をステータスチェック必須とするようにした場合、チェックとステータスはどちらも必須になります。 詳しい情報については、「[チェック](/rest/reference/checks)」を参照してください。

ステータスチェック必須を有効にした後、マージする前にブランチをベースブランチに対して最新にする必要がある場合があります。 これによって、ブランチがベースブランチからの最新のコードでテストされたことが保証されます。 ブランチが古い場合、ベースブランチをブランチにマージする必要があります。 詳しい情報については[保護されたブランチについて](/github/administering-a-repository/about-protected-branches#require-status-checks-before-merging)を参照してください。

{% note %}

**注釈:** Git リベースを使用してブランチをベースブランチに対して最新にすることもできます。 詳しい情報については、「[Git リベースについて](/github/getting-started-with-github/about-git-rebase)」を参照してください。

{% endnote %}

必須ステータスチェックにすべてパスするまでは、ローカルでの変更を保護されたブランチにプッシュすることはできません。 その代わりに、以下のようなエラーメッセージが返されます.

```shell
remote: error: GH006: Protected branch update failed for refs/heads/main.
remote: error: Required status check "ci-build" is failing
```
{% note %}

**注釈:** 最新で必須のステータスチェックをパスしたプルリクエストは、ローカルでマージしてから保護されたブランチにプッシュできます。 これはマージコミット自体でステータスチェックを実行せずに行えます。

{% endnote %}

{% ifversion fpt or ghae or ghes or ghec %}

## Conflicts between head commit and test merge commit

テストマージコミットと head コミットのステータスチェックの結果が競合する場合があります。 テストマージコミットにステータスがある場合、そのテストマージコミットは必ずパスする必要があります。 それ以外の場合、ヘッドコミットのステータスは、ブランチをマージする前にパスする必要があります。 テストマージコミットに関する詳しい情報については、「[プル](/rest/reference/pulls#get-a-pull-request)」を参照してください。

![マージコミットが競合しているブランチ](/assets/images/help/repository/req-status-check-conflicting-merge-commits.png)
{% endif %}

## Handling skipped but required checks

Sometimes a required status check is skipped on pull requests due to path filtering. For example, a Node.JS test will be skipped on a pull request that just fixes a typo in your README file and makes no changes to the JavaScript and TypeScript files in the `scripts` directory.

If this check is required and it gets skipped, then the check's status is shown as pending, because it's required. In this situation you won't be able to merge the pull request.

### サンプル

In this example you have a workflow that's required to pass.

```yaml
name: ci
on:
  pull_request:
    paths:
      - 'scripts/**'
      - 'middleware/**'
jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [12.x, 14.x, 16.x]
    steps:
    - uses: {% data reusables.actions.action-checkout %}
    - name: Use Node.js {% raw %}${{ matrix.node-version }}{% endraw %}
      uses: {% data reusables.actions.action-setup-node %}
      with:
        node-version: {% raw %}${{ matrix.node-version }}{% endraw %}
        cache: 'npm'
    - run: npm ci
    - run: npm run build --if-present
    - run: npm test
```

If someone submits a pull request that changes a markdown file in the root of the repository, then the workflow above won't run at all because of the path filtering. As a result you won't be able to merge the pull request. You would see the following status on the pull request:

![Required check skipped but shown as pending](/assets/images/help/repository/PR-required-check-skipped.png)

You can fix this by creating a generic workflow, with the same name, that will return true in any case similar to the workflow below :

```yaml
name: ci
on:
  pull_request:
    paths-ignore:
      - 'scripts/**'
      - 'middleware/**'
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: 'echo "No build required" '
```
Now the checks will always pass whenever someone sends a pull request that doesn't change the files listed under `paths` in the first workflow.

![Check skipped but passes due to generic workflow](/assets/images/help/repository/PR-required-check-passed-using-generic.png)

{% note %}

**ノート:**
* Make sure that the `name` key and required job name in both the workflow files are the same. For more information, see "[Workflow syntax for {% data variables.product.prodname_actions %}](/actions/reference/workflow-syntax-for-github-actions)".
* The example above uses {% data variables.product.prodname_actions %} but this workaround is also applicable to other CI/CD providers that integrate with {% data variables.product.company_short %}.

{% endnote %}

{% ifversion fpt or ghes > 3.3 or ghae-issue-5379 or ghec %}It's also possible for a protected branch to require a status check from a specific {% data variables.product.prodname_github_app %}. If you see a message similar to the following, then you should verify that the check listed in the merge box was set by the expected app.

```
Required status check "build" was not set by the expected {% data variables.product.prodname_github_app %}.
```
{% endif %}
