---
title: Using GitHub Copilot in Codespaces
intro: You can use Copilot in Codespaces by adding the extension.
versions:
  fpt: '*'
  ghec: '*'
type: reference
topics:
  - Codespaces
  - Copilot
  - Visual Studio Code
product: '{% data reusables.gated-features.codespaces %}'
shortTitle: Copilot in Codespaces
redirect_from:
  - /codespaces/codespaces-reference/using-copilot-in-codespaces
---

## {% data variables.product.prodname_copilot %}を使用する

[{% data variables.product.prodname_copilot %}](https://copilot.github.com/), an AI pair programmer, can be used in any codespace. To start using {% data variables.product.prodname_copilot_short %} in {% data variables.product.prodname_codespaces %}, install the [{% data variables.product.prodname_copilot_short %} extension from the {% data variables.product.prodname_vscode %} marketplace](https://marketplace.visualstudio.com/items?itemName=GitHub.copilot).

To include {% data variables.product.prodname_copilot_short %}, or other extensions, in all of your codespaces, enable Settings Sync. 詳しい情報については、「[アカウントの {% data variables.product.prodname_codespaces %} をパーソナライズする](/codespaces/customizing-your-codespace/personalizing-codespaces-for-your-account#settings-sync)」を参照してください。 Additionally, to include {% data variables.product.prodname_copilot_short %} in a given project for all users, you can specify `GitHub.copilot` as an extension in your `devcontainer.json` file. For information about configuring a `devcontainer.json` file, see "[Introduction to dev containers](/codespaces/customizing-your-codespace/configuring-codespaces-for-your-project#creating-a-custom-dev-container-configuration)."

