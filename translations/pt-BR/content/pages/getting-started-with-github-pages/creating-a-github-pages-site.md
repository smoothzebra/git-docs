---
title: Criar um site do GitHub Pages
intro: 'É possível criar um site do {% data variables.product.prodname_pages %} em um repositório novo ou existente.'
redirect_from:
  - /articles/creating-pages-manually
  - /articles/creating-project-pages-manually
  - /articles/creating-project-pages-from-the-command-line
  - /articles/creating-project-pages-using-the-command-line
  - /articles/creating-a-github-pages-site
  - /github/working-with-github-pages/creating-a-github-pages-site
product: '{% data reusables.gated-features.pages %}'
versions:
  fpt: '*'
  ghes: '*'
  ghae: '*'
  ghec: '*'
topics:
  - Pages
shortTitle: Criar um site do GitHub Pages
---

{% data reusables.pages.org-owners-can-restrict-pages-creation %}

## Criar um repositório para seu site

{% data reusables.pages.new-or-existing-repo %}

{% data reusables.repositories.create_new %}
{% data reusables.repositories.owner-drop-down %}
{% data reusables.pages.create-repo-name %}
{% data reusables.repositories.choose-repo-visibility %}
{% data reusables.repositories.initialize-with-readme %}
{% data reusables.repositories.create-repo %}

## Criar seu site

{% data reusables.pages.must-have-repo-first %}

{% data reusables.pages.private_pages_are_public_warning %}

{% data reusables.pages.navigate-site-repo %}
{% data reusables.pages.decide-publishing-source %}
3. Se a fonte de publicação que você escolheu já existe, navegue até ela. Caso contrário, crie a fonte de publicação.
4. Na raiz da fonte de publicação, crie um novo arquivo chamado `index.md` com o conteúdo que você deseja exibir na página principal do seu site.

  {% tip %}

  **Dica:** se `index.html` estiver presente, ele será usado ao invés de `index.md`. Se nem `index.html` nem `index.md` estiverem presentes, será usado `README.md`.

  {% endtip %}
{% data reusables.pages.configure-publishing-source %}
{% data reusables.repositories.sidebar-settings %}
{% data reusables.pages.sidebar-pages %}
{% data reusables.pages.choose-visibility %}
{% data reusables.pages.visit-site %}
{% data reusables.pages.check-workflow-run %}

{% data reusables.pages.admin-must-push %}

## Próximas etapas

Você pode adicionar mais páginas ao seu site criando novos arquivos. Cada arquivo ficará disponível no site na mesma estrutura de diretórios que a fonte de publicação. Por exemplo, se a fonte de publicação do site de projeto for o branch `gh-pages` e você criar um arquivo chamado `/about/contact-us.md` no branch `gh-pages`, o arquivo novo ficará disponível em {% ifversion fpt or ghec %}`https://<user>.github.io/<repository>/{% else %}`http(s)://<hostname>/pages/<username>/<repository>/{% endif %}about/contact-us.html`.

Também é possível adicionar um tema para personalizar a aparência do site. Para obter mais informações, consulte {% ifversion fpt or ghec %}"[Adicionar um tema ao site do {% data variables.product.prodname_pages %} com o seletor de temas](/articles/adding-a-theme-to-your-github-pages-site-with-the-theme-chooser){% else %}"[Adicionar um tema ao site do {% data variables.product.prodname_pages %} usando o Jekyll](/articles/adding-a-theme-to-your-github-pages-site-using-jekyll){% endif %}".

Para personalizar seu site ainda mais, você pode usar o Jekyll, um gerador de site estático com suporte integrado para o {% data variables.product.prodname_pages %}. Para obter mais informações, consulte "[Sobre o {% data variables.product.prodname_pages %} e o JJekyll](/articles/about-github-pages-and-jekyll)".

## Leia mais

- "[Solucionar problemas de erros de criação do Jekyll para sites do {% data variables.product.prodname_pages %}](/articles/troubleshooting-jekyll-build-errors-for-github-pages-sites)"
- "[Criar e excluir branches no repositório](/articles/creating-and-deleting-branches-within-your-repository)"
- "[Criar arquivos](/articles/creating-new-files)"
