# Introdução

Este [schema](#sobre-json-schema) oferece a função de _autocomplete_ de componentes VTEX IO no Visual Studio Code.

A vantagem é listar dentro do VS Code todos os possíveis componentes VTEX, assim como suas descrições e referência à documentação.

Isso reduz o tempo de trabalho e a necessidade de consultar constantemente a documentação, assim como evitar erros de digitação.

> A seguinte issue em repositório oficial da VTEX apresenta um levantamento dos problemas que este schema tenta solucionar: [Precisamos melhorar a experiência de desenvolvimento #643 ](https://github.com/vtex-apps/store-discussion/issues/643)

## Configuração

Deve-se configurar o schema manualmente. Acesse as configurações em JSON do Visual Studio Code pelo _Command Pallette_ (`Ctrl+Shift+P`) em _Preferences: Open Settings (JSON)_. (Para outras maneiras de acessar, [visite a documentação](https://code.visualstudio.com/docs/getstarted/settings))

Dentro do `settings.json`, insira no array `json.schemas` os tipos de arquivos afetados e o local do schema. No exemplo a seguir, o schema está na pasta raiz do projeto e afeta todos os arquivos `.jsonc`.

```json
{
  "json.schemas": [
    {
      "fileMatch": ["*.jsonc"],
      "url": "./schema.json"
    }
  ]
}
```

## Como usar?

Dentro de um arquivo `.jsonc`, use o comando `Ctrl + Espaço` para abrir uma lista com todas as opções possíveis. Você pode fazer isso em cada nível dos blocos, inclusive dentro das props.

Além disso, as opções também serão sugeridas conforme se digita dentro do JSON.

> Sâo proibidos blocos duplicados.

> Lembre-se de nomear seus blocos com identificadores únicos sempre que necessário. Ex.: `flex-layout.row#meu-nome`

---

# Contribuindo

## 1. Principal

O schema tem 3 campos principais:

| Campo               | Função                                                                                                                                                                                   |
| ------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `definitions`       | É onde são declarados efetivamente todos os blocos com suas props. Essas definições serão chamadas pelos outros dois objetos a seguir.                                                   |
| `properties`        | Habilita o **autocomplete** de blocos. Precisa de um para cada definição de bloco.                                                                                                       |
| `patternProperties` | Expressão regular que permite que os blocos sejam nomeados com identificadores, sem perder o autocomplete (Ex.: `flex-layout.row#meu-nome`). Precisa de um para cada definição de bloco. |

## 2. Criando blocos

#### Definindo blocos em `definitions`:

É importante que os blocos e suas props contenham o `title` e um `markdownDescription` com **descrição** e os **tipos de valores** possíveis, assim como é mostrado na [documentação da VTEX](https://developers.vtex.com/).

No campo `markdownDescription` dos blocos é importante colocar o link da documentação.

Também é importante colocar o **nome/versão major** do app relativo ao bloco no fim da descrição. Dessa maneira é possível instalá-lo no manifest mais facilmente apenas copiando da descrição, caso precise.

E caso seja necessária alguma configuração extra para instalação, pode ser interessante colocá-la na descrição também.

> É usado `markdownDescription` em vez de `description` para permitir uso de Markdown. Essa prop é exclusiva do VS Code e não tem a ver com o JSON Schema. Em outras palavras, é um `description` melhorado.

Exemplo de bloco:

```json
{
  "disclosure-layout": {
    "title": "Disclosure Layout",
    "markdownDescription": "https://developers.vtex.com/vtex-developer-docs/docs/vtex-disclosure-layout\n\n![Mandatory](https://img.shields.io/badge/-Mandatory-red.png) Parent block that enables you to build the disclosure layout using its 3 children blocks: `disclosure-trigger`, `disclosure-content`, and `disclosure-state-indicator`.\n\n`\"vtex.disclosure-layout@1.x\"`"
  }
}
```

Resultado ao posicionar o cursor no bloco:

![Resultado ao posicionar o cursor no bloco](https://i.imgur.com/2c0V7dn.png)

Exemplo de uma prop do mesmo bloco acima:

```json
{
  "initialVisibility": {
    "type": "string",
    "markdownDescription": "enum\n\nDefines the initial visibility of the layout content. Possible values are: `visible` (content initially open) or `hidden` (content is only displayed with user interaction).",
    "default": "visible",
    "oneOf": [
      {
        "const": "visible"
      },
      {
        "const": "hidden"
      }
    ]
  }
}
```

Resultado ao posicionar o cursor na prop:

![Resultado ao posicionar o cursor na prop](https://i.imgur.com/RB0Bjto.png)

#### Configurando campo `properties`:

Para configurar, basta criar um objeto com o nome do bloco e referenciar a ele sua respectiva definição. Exemplo:

```json
    "flex-layout.row": {
      "$ref": "#/definitions/flex-layout.row"
    },
```

Esse campo é responsável por ativar o autocomplete, então é importante que seu nome seja **exatamente igual** ao nome do bloco VTEX.

#### Configurando campo `patternProperties`:

O nome desse campo é uma **expressão regular** que permite que os blocos tenham identificadores únicos. A expressão é do tipo `^nome-do-bloco#`. Isso significa que o schema valida um bloco que termine com um identificador depois do símbolo `#`.

```json
    "^flex-layout.row#": {
      "$ref": "#/definitions/flex-layout.row"
    },
```

## 3. Git Flow e Padrão de Commits

#### Git Flow

Esse projeto foi preparado com **Git Flow**. Recomenda-se que uma branch seja criada a partir da branch `develop`, com o padrão `feature/nome-do-recurso`. Para isso, pode-se usar o seguinte comando no terminal:

```
git flow feature start nome-do-recurso
```

Quando concluir o trabalho de desenvolvimento no recurso, a próxima etapa é mesclar a ramificação de recurso na branch de desenvolvimento.

```
git flow feature finish nome-do-recurso
```

Para mais informações sobre o fluxo e **branches de hotfix**: https://www.atlassian.com/br/git/tutorials/comparing-workflows/gitflow-workflow

#### Git Commit Linter

As mensagens de commits seguem o padrão [Conventional Commit](https://www.conventionalcommits.org), e são verificadas automaticamente pelo `git-commit-msg-linter`. Os tipos de commit possíveis são: **feat**, **fix**, **docs**, **style**, **refactor**, **test**, **chore**, **perf**, **ci**, **build** e **temp**. Exemplos de uso:

❌ Não aceito:

    Correct spelling of CHANGELOG.

✅ Aceito:

    docs: correct spelling of CHANGELOG

✅ Aceito (recomendado, mensagem com escopo):

    docs(CHANGELOG): correct spelling

Para mais informações sobre Conventional Commit:

- https://www.conventionalcommits.org
- https://medium.com/linkapi-solutions/conventional-commits-pattern-3778d1a1e657

Repositório oficial de `git-commit-msg-linter`:

- https://www.npmjs.com/package/git-commit-msg-linter

---

# Evolução

## Problemas Encontrados

> Os problemas resolvidos serão estilizados com ~~um risco~~ e permanecem abaixo para manter histórico.

- O campo `url` do `json.schemas` precisa selecionar apenas os jsonc/json de dentro da pasta da vtex (/store). Caso contrário todos os
- Não está funcionando corretamente com workspaces, só com folders (tem a ver com o item acima)
- ~~O autocomplete não identifica o bloco depois de nomeado. Ex.: `flex-layout.row#nome`~~
- Blocos `login` e `login-content` possuem uma prop com grafia errada chamada `acessCodePlaceholder`. Verificar se está funcionando na prática com _acess_ ou _access_, pois não encontrei informação no [repositório do app](https://github.com/vtex-apps/login/).
- Em `sku-selector` faltou testar a prop `visibleVariations` (entender se ela funciona na prática como string|array ou string|object). [Referência](https://github.com/vtex-apps/store-components/blob/master/docs/SKUSelector.md)
- Em `shelf.relatedProduct` faltou testar a prop `summary` e entender se as suas opções estão corretas. Não há documentação suficiente. [Referência](https://developers.vtex.com/vtex-developer-docs/docs/vtex-shelf#related-products-shelf) (Ver o _ProductListSchema_)

## Problemas da VTEX

- Algumas referências em json da vtex trazem o valor booleano `"true"` como string, em vez de `true`. Isso não é aceito pelas props, que entendem qualquer string como true, e podem acabar "bugando" algumas props, como as de borda e posicionamento. [Exemplo de uma referência com esse suposto equívoco](https://developers.vtex.com/vtex-developer-docs/docs/vtex-product-list#advanced-configuration).

## Ideias/Sugestões de Melhoria

> As ideias implementadas serão estilizados com ~~um risco~~ e permanecem abaixo para manter histórico. Se necessário, virão acompanhadas da _hash_ de seu commit/PR.

- Transformar este schema em uma extensão pra não ser necessário lidar com o `settings.json`
- Acessar a documentação por uma aba no VS Code, assim como a extensão [DevDocs](https://marketplace.visualstudio.com/items?itemName=Anan.devdocstab)
- Criar também snippets com os blocos principais, além do schema, para agilizar ainda mais o desenvolvimento. Exemplo na extensão [VTEX IO Snippets](https://marketplace.visualstudio.com/items?itemName=brendonguedes.vtex-snippets). Guia do VS Code de como **criar snippets dentro do schema**, usando `defaultSnippets`: https://code.visualstudio.com/docs/languages/json
- ~~Adicionar automaticamente os títulos (`title`) em blocos de layout responsivo, como `footer-layout` ou `responsive-layout`, para os blocos se organizarem no Site Editor.~~
- Separar `definitions` e `patternProperties`em diferentes arquivos para não sobrecarregar a IDE com o tamanho do json
- Separar `definitions` em blocos menores (ex.: por tipo de componente), mesmo motivo do item anterior
- Colocar nas descrições as imagens com exemplo de uso do componente, que já está presente em algumas documentações. [Exemplo aqui](https://user-images.githubusercontent.com/52087100/82367576-6d196800-99ea-11ea-9672-77fa2b90a581.gif).

## Progresso (51,2% - 105 de 205)

Lista com os componentes VTEX que podem ser customizados, e a indicação de quais já foram contemplados no schema.

| Layout Components             | Status |
| ----------------------------- | ------ |
| flex-layout.row               | ✅     |
| flex-layout.column            | ✅     |
| condition-layout.product      | ✅     |
| condition-layout.binding      | ✅     |
| modal-trigger                 | ✅     |
| modal-layout                  | ✅     |
| modal-header                  | ✅     |
| modal-content                 | ✅     |
| modal-actions                 | ✅     |
| modal-actions.close           | ✅     |
| overlay-trigger               | ✅     |
| overlay-layout                | ✅     |
| responsive-layout.desktop     | ✅     |
| responsive-layout.mobile      | ✅     |
| responsive-layout.tablet      | ✅     |
| responsive-layout.phone       | ✅     |
| slider-layout                 | ✅     |
| slider-layout-group           | ✅     |
| stack-layout                  | ✅     |
| sticky-layout                 | ✅     |
| sticky-layout.stack-container | ✅     |
| tab-layout                    | ✅     |
| tab-list                      | ✅     |
| tab-list.item                 | ✅     |
| tab-list.item.children        | ✅     |
| tab-content                   | ✅     |
| tab-content.item              | ✅     |

| Basic Components               | Status |
| ------------------------------ | ------ |
| add-to-cart-button             | ✅     |
| breadcrumb                     | ✅     |
| breadcrumb.search              | ✅     |
| disclosure-layout              | ✅     |
| disclosure-trigger             | ✅     |
| disclosure-content             | ✅     |
| disclosure-state-indicator     | ✅     |
| disclosure-layout-group        | ✅     |
| disclosure-trigger-group       | ✅     |
| footer                         | ✅     |
| footer-layout.desktop          | ✅     |
| footer-layout.mobile           | ✅     |
| accepted-payment-methods       | ✅     |
| social-networks                | ✅     |
| powered-by                     | ✅     |
| header                         | ✅     |
| header-layout.desktop          | ✅     |
| header-layout.mobile           | ✅     |
| header-row                     | ✅     |
| header-border                  | ✅     |
| header-force-center            | ✅     |
| login                          | ✅     |
| login-content                  | ✅     |
| vtex.menu@2.x:menu             | ✅     |
| menu-item                      | ✅     |
| product-assembly-options       | ✅     |
| assembly-option-input-values   | ✅     |
| assembly-option-item-customize | ✅     |
| product-list                   | ✅     |
| product-list-content-desktop   | ✅     |
| product-list-content-mobile    | ✅     |
| message                        | ✅     |
| product-reference              | ✅     |
| price                          | ✅     |
| unit-price                     | ✅     |
| product-list-image             | ✅     |
| quantity-selector              | ✅     |
| remove-button                  | ✅     |
| rich-text                      | ✅     |

| Store Components    | Status |
| ------------------- | ------ |
| search-bar          | ✅     |
| back-to-top-button  | ✅     |
| image               | ✅     |
| info-card           | ✅     |
| logo                | ✅     |
| sku-selector        | ✅     |
| notification.bar    | ✅     |
| notification.inline | ✅     |
| product-brand       | ✅     |
| product-description | ✅     |
| product-images      | ✅     |
| product-name        | ✅     |
| share               | ✅     |
| shipping-simulator  | ✅     |

| Product Specifications      | Status |
| --------------------------- | ------ |
| product-specification-group | ✅     |
| product-specification       | ✅     |
| product-specification-value | ✅     |
| product-specification-text  | ✅     |

| Search Components           | Status |
| --------------------------- | ------ |
| autocomplete-result-list.v2 |        |
| search-banner               |        |
| did-you-mean                |        |
| search-suggestions          |        |

| Store Image             | Status |
| ----------------------- | ------ |
| list-context.image-list | ✅     |
| image-list              | ✅     |
| image-slider            | ✅     |
| image-new               | ✅     |

| Product Highlights                             | Status |
| ---------------------------------------------- | ------ |
| vtex.product-highlights@2.x:product-highlights | ✅     |
| product-highlight-text                         | ✅     |
| product-highlight-wrapper                      | ✅     |

| Minicart                    | Status |
| --------------------------- | ------ |
| minicart.v2                 | ✅     |
| minicart-product-list       |        |
| minicart-base-content       |        |
| minicart-empty-state        |        |
| minicart-summary            |        |
| minicart-checkout-button    |        |
| minicart-summary            |        |
| checkout-summary.compact    |        |
| summary-totalizer           |        |
| product-list                |        |
| product-list-content-mobile |        |

| Product Price                  | Status |
| ------------------------------ | ------ |
| product-list-price             | ✅     |
| product-selling-price          |        |
| product-spot-price             |        |
| product-installments           |        |
| product-installments-list      |        |
| product-installments-list-item |        |
| product-price-savings          |        |
| product-spot-price-savings     |        |
| product-list-price-range       |        |
| product-selling-price-range    |        |
| product-seller-name            |        |
| product-price-suspense         |        |
| product-quantity               | ✅     |

| Search Result App                | Status |
| -------------------------------- | ------ |
| search-result-layout             |        |
| search-result-layout.customQuery |        |
| search-result-layout.desktop     |        |
| search-result-layout.mobile      |        |
| search-layout-switcher           |        |
| search-not-found-layout          |        |
| gallery                          |        |
| gallery-layout-switcher          |        |
| gallery-layout-option            |        |
| not-found                        |        |
| search-content                   |        |
| store.not-found#search           |        |
| search-products-count-per-page   |        |
| search-products-progress-bar     |        |
| order-by.v2                      |        |
| filter-navigator.v3              |        |
| total-products.v2                |        |
| search-title.v2                  |        |
| search-fetch-more                |        |
| search-fetch-previous            |        |
| search-products-count-per-page   |        |
| sidebar-close-button             |        |
| search-title.v2                  |        |

| Product Summary                 | Status |
| ------------------------------- | ------ |
| product-summary-quantity        | ✅     |
| list-context.product-list       | ✅     |
| product-summary.shelf           | ✅     |
| shelf.relatedProducts           | ✅     |
| product-summary-attachment-list | ✅     |
| product-summary-brand           | ✅     |
| product-summary-buy-button      | ✅     |
| product-summary-description     | ✅     |
| product-summary-image           | ✅     |
| product-summary-name            | ✅     |
| product-summary-sku-name        | ✅     |
| product-summary-sku-selector    | ✅     |

| Advanced Components              | Status |
| -------------------------------- | ------ |
| mega-menu                        |        |
| category-menu                    |        |
| challenge.trade-policy-condition |        |
| iframe                           |        |
| product-availability             |        |
| product-gifts                    |        |
| product-gift-list                |        |
| gift-text                        |        |
| gift-name                        |        |
| gift-image                       |        |
| product-kit                      |        |
| product-specification-badges     |        |
| quickorder-textarea              |        |
| quickorder-upload                |        |
| quickorder-autocomplete          |        |
| quickorder-categories            |        |
| product-reviews                  |        |
| product-rating-summary           |        |
| product-rating-inline            |        |
| sandbox                          |        |
| sandbox.product                  |        |
| sandbox.order                    |        |
| link-seller                      |        |
| drawer                           |        |
| drawer-trigger                   |        |
| drawer-header                    |        |
| drawer-close-button              |        |
| form                             |        |
| form-input.checkbox              |        |
| form-input.dropdown              |        |
| form-input.radiogroup            |        |
| form-input.textarea              |        |
| form-input.text                  |        |
| form-field-group                 |        |
| form-input.upload                |        |
| form-submit                      |        |
| form-success                     |        |
| link.product                     |        |
| link                             |        |
| media                            |        |
| list-context.media-list          |        |
| newsletter-form                  |        |
| newsletter-input-email           |        |
| newsletter-input-name            |        |
| newsletter-input-phone           |        |
| newsletter-checkbox-confirmation |        |
| newsletter-hidden-field          |        |
| newsletter-submit                |        |
| video                            |        |
| telemarketing                    |        |
| sku-list                         |        |

---

## Sobre JSON Schema

Para mais informações e fundamentos de JSON Schema, [acesse o site oficial](https://json-schema.org/learn/getting-started-step-by-step.html#intro).
