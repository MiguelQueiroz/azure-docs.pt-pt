---
title: "Descrição geral dos escalões de preços das Mensagens Standard e Premium do Service Bus | Microsoft Docs"
description: Mensagens Standard e Premium do Service Bus
services: service-bus-messaging
documentationcenter: .net
author: djrosanova
manager: timlt
editor: 
ms.assetid: e211774d-821c-4d79-8563-57472d746c58
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/18/2017
ms.author: darosa;sethm;jotaub
translationtype: Human Translation
ms.sourcegitcommit: f223ea7ac168d3c800b6ba664b31ab66b382d6cc
ms.openlocfilehash: 2657965ff3ee028263f9ef0c48024fe1839eee6e


---
# <a name="service-bus-premium-and-standard-messaging-tiers"></a>Escalões de mensagens Standard e Premium do Service Bus
As Mensagens do Service Bus, que incluem entidades como filas e tópicos, combinam capacidades de mensagens empresariais com semântica de publicação-subscrição avançada à escala da cloud. As Mensagens do Service Bus são utilizadas como base de comunicação para várias soluções de cloud sofisticadas.

O escalão *Premium* das Mensagens do Service Bus atende pedidos de cliente comuns relativamente a dimensionamento, desempenho e disponibilidade para aplicações fundamentais. Embora os conjuntos de funcionalidades sejam praticamente idênticos, estes dois escalões de Mensagens do Service Bus foram concebidos para serem utilizados em situações diferentes.

Na tabela seguinte, destacam-se algumas das principais diferenças.

| Premium | Standard |
| --- | --- |
| Débito elevado |Débito variável |
| Desempenho previsível |Latência variável |
| Preços fixos |Preços variáveis de utilização  |
| Possibilidade de aumentar e reduzir verticalmente a carga de trabalho |N/D |
| Tamanho da mensagem até 1 MB |Tamanho da mensagem até 256 KB |

As **Mensagens Premium do Service Bus** fornecem isolamento de recursos na camada de CPU e memória para que cada carga de trabalho do cliente seja executada de forma isolada. Este contentor de recursos é designado por *unidade de mensagens*. A cada espaço de nomes premium é atribuído, pelo menos, uma unidade de mensagens. Pode comprar 1, 2 ou 4 unidades de mensagens para cada espaço de nomes Premium do Service Bus. Uma única carga de trabalho ou entidade pode abranger várias unidades de mensagens e o número de unidades de mensagens pode ser alterado à vontade, embora a faturação seja realizada com base em tarifas diárias ou de 24 horas. O resultado é um desempenho previsível e repetível da sua solução com base no Service Bus.

Este desempenho não só é mais previsível e repetível, como também mais rápido. As Mensagens do Service Bus Premium baseiam-se no motor de armazenamento introduzido nos [Hubs de Eventos do Azure](https://azure.microsoft.com/services/event-hubs/). Com as Mensagens Premium, obter o máximo rendimento é muito mais rápido do que com o escalão Standard.

## <a name="premium-messaging-technical-differences"></a>Diferenças técnicas das Mensagens Premium
A seguir apresentam-se algumas diferenças entre as camadas de mensagens Standard e Premium.

### <a name="partitioned-queues-and-topics"></a>Filas e tópicos particionados
As filas e tópicos particionados são suportados nas Mensagens Premium, mas não funcionam da mesma forma que nos escalões de Mensagens Standard e Basic do Service Bus. As Mensagens Premium não utilizam SQL como armazém de dados e já não têm a possível concorrência de recursos associada a uma plataforma partilhada. Como resultado, não é necessário criar partições para assegurar o desempenho. Além disso, a quantidade de partições foi alterada de 16 partições nas Mensagens Standard para 2 partições nas Premium. Ter duas partições garante a disponibilidade e é um número mais adequado para o ambiente de tempo de execução Premium. Para mais informações sobre a criação de partições, consulte o artigo [Filas e tópicos particionados](service-bus-partitioning.md).

### <a name="express-entities"></a>Entidades expressas
Uma vez que as mensagens Premium se executam num ambiente de tempo de execução completamente isolado, as entidades expressas deixam de ser suportadas nos espaços de nome Premium. Para obter mais informações sobre a funcionalidade Express, veja a propriedade [QueueDescription.EnableExpress](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.queuedescription#Microsoft_ServiceBus_Messaging_QueueDescription_EnableExpress).

## <a name="get-started-with-premium-messaging"></a>Introdução às Mensagens Premium

A introdução às Mensagens Premium é simples e o processo é semelhante ao das Mensagens Standard. Comece por [criar um espaço de nomes](service-bus-create-namespace-portal.md). Certifique-se de que seleciona **Premium**, em **Escalão de preço**.

![create-premium-namespace][create-premium-namespace]

Também pode criar [espaços de nomes Premium com modelos do Azure Resource Manager](https://azure.microsoft.com/en-us/resources/templates/101-servicebus-pn-ar/).


## <a name="next-steps"></a>Passos seguintes
Para saber mais sobre Mensagens do Service Bus, veja os tópicos seguintes.

* [Introducing Azure Service Bus Premium Messaging (Introdução às Mensagens Premium do Azure Service Bus) (mensagem de blogue)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Introducing Azure Service Bus Premium Messaging (Introdução às Mensagens Premium do Azure Service Bus) (Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Descrição geral de Mensagens do Service Bus](service-bus-messaging-overview.md)
* [Como utilizar as filas do Service Bus](service-bus-dotnet-get-started-with-queues.md)

<!--Image references-->

[create-premium-namespace]: ./media/service-bus-premium-messaging/select-premium-tier.png



<!--HONumber=Jan17_HO3-->


