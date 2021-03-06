---
title: "Consultar o Índice da Azure Search utilizando o Portal do Azure | Microsoft Docs"
description: Emita uma consulta de pesquisa no Explorador de Pesquisa do Portal do Azure.
services: search
manager: jhubbard
documentationcenter: 
author: ashmaka
ms.assetid: 8e524188-73a7-44db-9e64-ae8bf66b05d3
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.date: 08/29/2016
ms.author: ashmaka
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: a23372112e17703a3399e1bdc9eaf73b85a1f80d


---
# <a name="query-your-azure-search-index-using-the-azure-portal"></a>Consultar o índice da Azure Search utilizando o Portal do Azure
> [!div class="op_single_selector"]
> * [Descrição geral](search-query-overview.md)
> * [Portal](search-explorer.md)
> * [.NET](search-query-dotnet.md)
> * [REST](search-query-rest-api.md)
> 
> 

Este guia irá mostrar como consultar o índice da Azure Search no Portal do Azure.

Antes de iniciar estas instruções, já deverá ter [criado um índice da Azure Search](search-what-is-an-index.md) e [preenchido este com dados](search-what-is-data-import.md).

## <a name="i-go-to-your-azure-search-blade"></a>I. Aceda ao painel da Azure Search
1. Clique em "Todos os recursos" no menu no lado esquerdo do [Portal do Azure](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices)
2. Selecione o seu serviço do Azure Search

## <a name="ii-select-the-index-you-would-like-to-search"></a>II. Selecione o índice que pretende pesquisar
1. Selecione o índice que pretende pesquisar a partir do mosaico "Índices".

![](./media/search-explorer/pick-index.png)

## <a name="iii-click-on-the-search-explorer-tile"></a>III. Clique no mosaico "Explorador de Pesquisa"
![](./media/search-explorer/search-explorer-tile.png)

## <a name="iii-start-searching"></a>III. Inicie a pesquisa
1. Para pesquisar o índice da Azure Search, comece a digitar no campo "*Cadeia de consulta*" e, em seguida, prima "**Pesquisar**".
   
   * Ao utilizar o Explorador de Pesquisa, pode especificar qualquer um dos [parâmetros de consulta](https://msdn.microsoft.com/library/dn798927.aspx)
2. Na secção "*Resultados*", os resultados da consulta serão apresentados no JSON não processado que receberia num Corpo de Resposta HTTP ao emitir pedidos de pesquisa na API REST da Azure Search.
3. A cadeia de consulta é analisada automaticamente para o URL de pedido adequado para submeter um pedido HTTP na API REST da Azure Search

![](./media/search-explorer/search-bar.png)




<!--HONumber=Nov16_HO2-->


