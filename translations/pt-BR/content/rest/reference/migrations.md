---
title: Migrações
intro: 'A API de migração permite que você faça a migração dos repositórios e usuários da sua organização de {% data variables.product.prodname_dotcom_the_website %} para {% data variables.product.prodname_ghe_server %}.'
redirect_from:
  - /v3/migrations
  - /v3/migration
  - /v3/migration/migrations
versions:
  fpt: '*'
  ghec: '*'
topics:
  - API
miniTocMaxHeadingLevel: 3
---

{% for operation in currentRestOperations %}
  {% unless operation.subcategory %}{% include rest_operation %}{% endunless %}
{% endfor %}

## organização

A API de migrações só está disponível para os proprietários de organizações autenticadas. Para obter mais informações, consulte "[Funções em organização](/organizations/managing-peoples-access-to-your-organization-with-roles/roles-in-an-organization#permission-levels-for-an-organization)" e "[Outros métodos de autenticação](/rest/overview/other-authentication-methods)".

{% data variables.migrations.organization_migrations_intro %}

{% for operation in currentRestOperations %}
  {% if operation.subcategory == 'orgs' %}{% include rest_operation %}{% endif %}
{% endfor %}

## Importações de código-fonte

{% data variables.migrations.source_imports_intro %}

Uma importação de código-fonte típica iniciaria a importação e, em seguida, (opcionalmente) atualizaria os autores e/ou atualizaria a preferência pelo uso do LFS do Git se existirem arquivos grandes na importação. Também é possível criar um webhook que ouve o [`ReposityImportEvent`](/developers/webhooks-and-events/webhook-events-and-payloads#repository_import) para descobrir o status da importação.

Um exemplo mais detalhado pode ser visto neste diagrama:

```
+---------+                     +--------+                              +---------------------+
| Tooling |                     | GitHub |                              | Original Repository |
+---------+                     +--------+                              +---------------------+
     |                              |                                              |
     |  Start import                |                                              |
     |----------------------------->|                                              |
     |                              |                                              |
     |                              |  Download source data                        |
     |                              |--------------------------------------------->|
     |                              |                        Begin streaming data  |
     |                              |<---------------------------------------------|
     |                              |                                              |
     |  Get import progress         |                                              |
     |----------------------------->|                                              |
     |       "status": "importing"  |                                              |
     |<-----------------------------|                                              |
     |                              |                                              |
     |  Get commit authors          |                                              |
     |----------------------------->|                                              |
     |                              |                                              |
     |  Map a commit author         |                                              |
     |----------------------------->|                                              |
     |                              |                                              |
     |                              |                                              |
     |                              |                       Finish streaming data  |
     |                              |<---------------------------------------------|
     |                              |                                              |
     |                              |  Rewrite commits with mapped authors         |
     |                              |------+                                       |
     |                              |      |                                       |
     |                              |<-----+                                       |
     |                              |                                              |
     |                              |  Update repository on GitHub                 |
     |                              |------+                                       |
     |                              |      |                                       |
     |                              |<-----+                                       |
     |                              |                                              |
     |  Map a commit author         |                                              |
     |----------------------------->|                                              |
     |                              |  Rewrite commits with mapped authors         |
     |                              |------+                                       |
     |                              |      |                                       |
     |                              |<-----+                                       |
     |                              |                                              |
     |                              |  Update repository on GitHub                 |
     |                              |------+                                       |
     |                              |      |                                       |
     |                              |<-----+                                       |
     |                              |                                              |
     |  Get large files             |                                              |
     |----------------------------->|                                              |
     |                              |                                              |
     |  opt_in to Git LFS           |                                              |
     |----------------------------->|                                              |
     |                              |  Rewrite commits for large files             |
     |                              |------+                                       |
     |                              |      |                                       |
     |                              |<-----+                                       |
     |                              |                                              |
     |                              |  Update repository on GitHub                 |
     |                              |------+                                       |
     |                              |      |                                       |
     |                              |<-----+                                       |
     |                              |                                              |
     |  Get import progress         |                                              |
     |----------------------------->|                                              |
     |        "status": "complete"  |                                              |
     |<-----------------------------|                                              |
     |                              |                                              |
     |                              |                                              |
```

{% for operation in currentRestOperations %}
  {% if operation.subcategory == 'source-imports' %}{% include rest_operation %}{% endif %}
{% endfor %}

## Usuário

A API de migrações do usuário só está disponível para proprietários de contas autenticadas. Para obter mais informações, consulte "[Outros métodos de autenticação](/rest/overview/other-authentication-methods)".

{% data variables.migrations.user_migrations_intro %} Para obter uma lista dos dados de migração que você pode baixar, consulte "[Fazer download de um arquivo de migração do usuário](#download-a-user-migration-archive)".

Para fazer o download de um arquivo, você deverá iniciar uma migração de usuário primeiro. Uma vez que o status da migração é `exportado`, você pode fazer o download da migração.

Ao criar um arquivo de migração, ele ficará disponível para download por sete dias. No entanto, você pode excluir o arquivo de migração do usuário mais cedo, se desejar. Você pode desbloquear o repositório quando a migração for `exportada` para começar a usar seu repositório novamente ou excluir o repositório se não precisar mais dos dados do código-fonte.

{% for operation in currentRestOperations %}
  {% if operation.subcategory == 'users' %}{% include rest_operation %}{% endif %}
{% endfor %}
