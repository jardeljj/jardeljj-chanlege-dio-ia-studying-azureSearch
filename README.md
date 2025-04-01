# Azure AI Search - Guia de Utilização

## Introdução
Azure AI Search é um serviço gerenciado de pesquisa em nuvem que permite criar experiências de busca rápidas e eficientes. Este guia aborda a configuração inicial, indexação de dados e execução de consultas.

## Pré-requisitos
Antes de começar, certifique-se de que você tem:
- Uma conta no Azure.
- Um recurso do Azure AI Search criado no portal do Azure.
- Azure CLI ou Postman para testar requisições HTTP.

## 1. Criando um Serviço de Pesquisa no Azure

1. Acesse o [Portal do Azure](https://portal.azure.com/).
2. No menu de pesquisa, digite "Azure AI Search" e selecione "Criar".
3. Escolha um nome para o serviço e selecione uma região.
4. Selecione um SKU (por exemplo, `Basic`, `Standard`, etc.).
5. Clique em "Revisar e Criar" e depois em "Criar".

## 2. Criando um Índice
O índice define a estrutura dos dados que serão pesquisados.

1. Acesse seu serviço de busca no Portal do Azure.
2. No menu lateral, clique em "Índices" e depois em "Adicionar índice".
3. Defina um nome para o índice.
4. Adicione campos com seus respectivos tipos (por exemplo, `string`, `int`, `date`).
5. Configure chaves e atributos como `searchable`, `filterable` e `sortable`.
6. Clique em "Criar".

Alternativamente, via API:
```sh
curl -X POST "https://<NOME_DO_SERVICO>.search.windows.net/indexes?api-version=2023-07-01" \
-H "Content-Type: application/json" \
-H "api-key: <SUA_CHAVE_DE_ADMINISTRACAO>" \
-d '{
  "name": "meuindice",
  "fields": [
    { "name": "id", "type": "Edm.String", "key": true },
    { "name": "titulo", "type": "Edm.String", "searchable": true },
    { "name": "descricao", "type": "Edm.String", "searchable": true }
  ]
}'
```

## 3. Indexando Dados
Adicione dados ao índice utilizando a API de upload de documentos:

```sh
curl -X POST "https://<NOME_DO_SERVICO>.search.windows.net/indexes/meuindice/docs/index?api-version=2023-07-01" \
-H "Content-Type: application/json" \
-H "api-key: <SUA_CHAVE_DE_ADMINISTRACAO>" \
-d '{
  "value": [
    {
      "@search.action": "upload",
      "id": "1",
      "titulo": "Azure AI Search",
      "descricao": "Serviço de busca gerenciado pela Microsoft."
    }
  ]
}'
```

## 4. Realizando Buscas
Para buscar documentos, utilize a API de pesquisa:

```sh
curl -X GET "https://<NOME_DO_SERVICO>.search.windows.net/indexes/meuindice/docs?search=Azure&api-version=2023-07-01" \
-H "api-key: <SUA_CHAVE_DE_ADMINISTRACAO>"
```

Isso retornará resultados que contêm a palavra "Azure" no campo `titulo` ou `descricao`.

## 5. Refinando Pesquisas
- **Filtros:**
  ```sh
  curl -X GET "https://<NOME_DO_SERVICO>.search.windows.net/indexes/meuindice/docs?search=*&$filter=startswith(titulo,'Azure')&api-version=2023-07-01" \
  -H "api-key: <SUA_CHAVE_DE_ADMINISTRACAO>"
  ```
- **Ordenação:**
  ```sh
  curl -X GET "https://<NOME_DO_SERVICO>.search.windows.net/indexes/meuindice/docs?search=*&$orderby=descricao desc&api-version=2023-07-01" \
  -H "api-key: <SUA_CHAVE_DE_ADMINISTRACAO>"
  ```

## Conclusão
Este guia apresentou os passos básicos para configurar e utilizar o Azure AI Search. Para mais funcionalidades, consulte a [documentação oficial](https://learn.microsoft.com/en-us/azure/search/).

