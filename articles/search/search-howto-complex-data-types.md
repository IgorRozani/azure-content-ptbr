<properties
    pageTitle="Como modelar os tipos de dados complexos no Azure Search | Microsoft Azure Search"
    description="As estruturas de dados aninhadas e hierárquicas podem ser modeladas em um índice do Azure Search usando o conjunto de linhas nivelado e o tipo de dados Coleções."
    services="search"
    documentationCenter=""
	authors="LiamCa"
	manager="pablocas"
	editor=""
    tags="complex data types; compound data types; aggregate data types"
/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.date="09/07/2016"
    ms.author="liamca"
/>

# Como modelar os tipos de dados complexos no Azure Search

Os conjuntos de dados externos usados para preencher um índice do Azure Search, às vezes, incluem subestruturas hierárquicas ou aninhadas que não se dividem de modo organizado em um conjunto de linhas da tabela. Os exemplos dessas estruturas podem incluir vários locais e números de telefone para um único cliente, vários tamanhos e cores para um único SKU, vários autores de um único livro e assim por diante. Em termos de modelagem, você pode ver essas estruturas referidas como *tipos de dados complexos*, *tipos de dados compostos*, *tipos de dados combinados* ou *tipos de dados de agregação*, para citar alguns.

Os tipos de dados complexos não são suportados nativamente no Azure Search, mas uma solução comprovada inclui um processo de duas etapas de nivelamento da estrutura e uso de um tipo de dados **Coleção** para reconstituir a estrutura interna. Seguir a técnica descrita neste artigo permite que o conteúdo seja pesquisado, lapidado, filtrado e classificado.

## Exemplo de uma estrutura de dados complexos

Normalmente, os dados em questão residem como um conjunto de documentos XML ou JSON, ou como itens em um armazenamento NoSQL, como o DocumentDB. Estruturalmente, o desafio é ter vários itens-filho que precisam ser pesquisados e filtrados. Como ponto de partida para ilustrar a solução alternativa, veja o seguinte documento JSON que lista um conjunto de contatos como um exemplo:

~~~~~
[
  {
    "id": "1",
    "name": "John Smith",
    "company": "Adventureworks",
    "locations": [
      {
        "id": "1",
        "description": "Adventureworks Headquarters"
      },
      {
        "id": "2",
        "description": "Home Office"
      }
    ]
  }, 
  {
    "id": "2",
    "name": "Jen Campbell",
    "company": "Northwind",
    "locations": [
      {
        "id": "3",
        "description": "Northwind Headquarter"
      },
      {
        "id": "4",
        "description": "Home Office"
      }
    ]
}]
~~~~~

Embora os campos denominados 'id', 'name' e 'empresa' possam ser mapeados facilmente um a um como campos em um índice do Azure Search, o campo 'locais' contém uma matriz de locais, com um conjunto de IDs de local, bem como descrições do local. Considerando que o Azure Search não tem um tipo de dados que oferece suporte, precisamos de uma maneira diferente de modelar isso no Azure Search.

> [AZURE.NOTE] Essa técnica também é descrita por Kirk Evans em uma postagem de blog [Indexando o DocumentDB com o Azure Search](https://blogs.msdn.microsoft.com/kaevans/2015/03/09/indexing-documentdb-with-azure-seach/), que mostra uma técnica denominada "nivelar os dados", por meio da qual você teria os campos chamados `locationsID` e `locationsDescription` que são [coleções](https://msdn.microsoft.com/library/azure/dn798938.aspx) (ou uma matriz de cadeias de caracteres).

## Parte 1: Nivelar a matriz em campos individuais

Para criar um índice do Azure Search que aceita esse conjunto de dados, crie campos individuais para a subestrutura aninhada: `locationsID` e `locationsDescription` com um tipo de dados de [coleções](https://msdn.microsoft.com/library/azure/dn798938.aspx) (ou uma matriz de cadeias de caracteres). Nesses campos, você deve indexar os valores '1' e '2' no campo `locationsID` para John Smith e os valores '3' e '4' no campo `locationsID` para Jen Campbell.

Os dados no Azure Search ficariam assim:

![dados de exemplo, 2 linhas](./media/search-howto-complex-data-types/sample-data.png)


## Parte 2: Adicionar um campo de coleção na definição do índice

No esquema do índice, as definições do campo podem ser semelhantes a este exemplo.

~~~~
var index = new Index()
{
    Name = indexName,
    Fields = new[]
    {
        new Field("id", DataType.String) { IsKey = true },
        new Field("name", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("company", DataType.String) { IsSearchable = true, IsFilterable = false, IsSortable = false, IsFacetable = false },
        new Field("locationsId", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true },
        new Field("locationsDescription", DataType.Collection(DataType.String)) { IsSearchable = true, IsFilterable = true, IsFacetable = true }
    }
};
~~~~

## Validar os comportamentos da pesquisa e, opcionalmente, estender o índice

Supondo que você criou o índice e carregou os dados, agora você pode testar a solução para verificar a execução da consulta de pesquisa no conjunto de dados. Cada campo da **coleção** deve ser **pesquisável**, **filtrável** e **lapidável**. Você deve ser capaz de executar consultas como:

* Localize todas as pessoas que trabalham na 'Matriz Adventureworks'.
* Obtenha uma contagem do número de pessoas que trabalham em um ‘Escritório Central’.
* Das pessoas que trabalham em um ‘Escritório Central', mostre em quais outros escritórios elas trabalham, junto com uma contagem de pessoas em cada local.

Onde essa técnica falha é quando você precisa fazer uma pesquisa que combina a identificação do local, bem como a descrição do local. Por exemplo:

* Localize todas as pessoas onde elas têm um Escritório Central E uma Identificação de local 4.

Se você se lembra, o conteúdo original era assim:

~~~~
   {
        id: '4',
        description: 'Home Office'
   }
~~~~

No entanto, agora que dividimos os dados em campos separados, não temos nenhuma maneira de saber se o Escritório Central para Jen Campbell relaciona-se a `locationsID 3` ou `locationsID 4`.

Para lidar com isso, defina outro campo no índice que combina todos os dados em uma única coleção. Para nosso exemplo, chamaremos esse campo de `locationsCombined` e iremos separar o conteúdo com `||`, embora seja possível escolher qualquer separador que você acha que seria um conjunto exclusivo de caracteres para seu conteúdo. Por exemplo:

![dados de exemplo, 2 linhas com separador](./media/search-howto-complex-data-types/sample-data-2.png)

Usando esse campo `locationsCombined`, agora podemos aceitar ainda mais consultas, como:

* Mostre uma contagem de pessoas que trabalham em um 'Escritório Central' com uma Id de local '4'.
* Procure as pessoas que trabalham em um ‘Escritório Central' com uma Id de local '4'.

## Limitações

Essa técnica é útil para vários cenários, mas ela não é aplicável em todos os casos. Por exemplo:

1. Se você não tem um conjunto estático de campos em seu tipo de dados complexos e não foi possível mapear todos os tipos possíveis para um único campo.
2. Atualizar os objetos aninhados requer um trabalho extra para determinar exatamente o que precisa ser atualizado no índice do Azure Search

## Exemplo de código

Você pode ver um exemplo sobre como indexar um conjunto de dados JSON complexo no Azure Search e executar várias consultas nesse conjunto de dados no [repositório GitHub](https://github.com/liamca/AzureSearchComplexTypes).

## Próxima etapa

[Escolha o suporte nativo para os tipos de dados complexos](https://feedback.azure.com/forums/263029-azure-search) na página UserVoice do Azure Search e forneça qualquer entrada adicional que você gostaria de considerar em relação à implementação do recurso. Você também pode me acessar diretamente no Twitter em @liamca.


 

<!---HONumber=AcomDC_0914_2016-->