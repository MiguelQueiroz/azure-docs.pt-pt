---
title: "Criar uma função de processamento de eventos | Microsoft Docs"
description: "Utilize funções do Azure para criar uma função de C# que seja executada com base num temporizador de eventos."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 84bd0373-65e2-4022-bcca-2b9cd9e696f5
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/25/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 44e397c7521ba8f0ba11893c364f51177561bee4
ms.openlocfilehash: df3d303ee10fcc982552ea9756eb59198c87b650


---
# <a name="create-an-event-processing-azure-function"></a>Criar uma Função do Azure de processamento de eventos
As Funções do Azure são uma experiência baseada em eventos, de cálculo a pedido que lhe permite criar unidades agendadas ou acionadas de código implementado em várias linguagens de programação. Para saber mais acerca das Funções do Azure, veja a [Descrição Geral das Funções do Azure](functions-overview.md).

Este tópico mostra-lhe como criar uma nova função em C# que é executada com base num temporizador de eventos para adicionar mensagens a uma fila de armazenamento. 

## <a name="prerequisites"></a>Pré-requisitos
Uma aplicação de função aloja a execução das suas funções no Azure. Se ainda não tiver uma conta do Azure, veja a experiência [Experimentar Funções](https://functions.azure.com/try) ou [criar uma conta gratuita do Azure](https://azure.microsoft.com/free/). 

## <a name="create-a-timer-triggered-function-from-the-template"></a>Criar uma função acionada por temporizador a partir do modelo
Uma aplicação de função aloja a execução das suas funções no Azure. Antes de poder criar uma função, tem de ter uma conta do Azure ativa. Se ainda não tiver uma conta do Azure, [estão disponíveis contas gratuitas](https://azure.microsoft.com/free/). 

1. Aceda ao [portal das Funções do Azure](https://functions.azure.com/signin) e inicie sessão com a sua conta do Azure.
2. Se tiver uma aplicação de função existente para utilizar, selecione-a partir de **As suas aplicações de função** e, em seguida, clique em **Abrir**. Para criar uma nova aplicação de função, escreva um **Nome** exclusivo para a nova aplicação de função ou aceite a gerada, selecione a **Região** preferencial e, em seguida, clique em **Criar + começar**. 
3. Na sua aplicação de função, clique em **+ Nova Função** > **TimerTrigger - C#** > **Criar**. Isto cria uma função com um nome predefinido que é executado na agenda predefinida de uma vez a cada minuto. 
   
    ![Criar uma nova função de acionada por temporizador](./media/functions-create-an-event-processing-function/functions-create-new-timer-trigger.png)
4. Na sua nova função, clique no separador **Integrar** > **Nova Saída** > **Fila de Armazenamento do Azure** > **Selecionar**.
   
    ![Criar uma nova função de acionada por temporizador](./media/functions-create-an-event-processing-function/functions-create-storage-queue-output-binding.png)
5. Em **saída da Fila de Armazenamento do Azure**, selecione uma **ligação de conta de Armazenamento** existente ou crie uma nova e clique em **Guardar**. 
   
    ![Criar uma nova função de acionada por temporizador](./media/functions-create-an-event-processing-function/functions-create-storage-queue-output-binding-2.png)
6. De novo no separador **Desenvolver**, substitua o script do C# existente na janela **Código** com o código seguinte:
    ```cs   
    using System;

    public static void Run(TimerInfo myTimer, out string outputQueueItem, TraceWriter log)
    {
        // Add a new scheduled message to the queue.
        outputQueueItem = $"Ping message added to the queue at: {DateTime.Now}.";

        // Also write the message to the logs.
        log.Info(outputQueueItem);
    }
    ```
   
    Este código adiciona uma nova mensagem à fila com a data e hora atuais quando a função for executada.
7. Clique em **Guardar** e veja as janelas **Registos** para a execução de função seguinte.
8. (Opcional) Navegue para a conta de armazenamento e certifique-se de que as mensagens estão a ser adicionadas à fila.
9. Volte para o separador **Integrar** e altere o campo de agenda para `0 0 * * * *`. A função é agora executada uma vez a cada hora. 

Isto é um exemplo muito simplificado de um acionador de temporizador e de um vínculo de saída da fila de armazenamento. Para obter mais informações, veja os tópicos [Acionador do temporizador de Funções do Azure](functions-bindings-timer.md) e [Acionadores de Funções do Azure e vínculos para Armazenamento do Azure](functions-bindings-storage.md).

## <a name="next-steps"></a>Passos seguintes
Veja os seguintes tópicos para obter mais informações sobre o Funções do Azure.

* [Referência para programadores das Funções do Azure](functions-reference.md)  
  Referência para programadores para codificar funções e definir acionadores e enlaces.
* [Testar as Funções do Azure](functions-test-a-function.md)  
  Descreve várias ferramentas e técnicas para testar as suas funções.
* [Como dimensionar as Funções do Azure](functions-scale.md)  
  Aborda os planos de serviço disponíveis com as Funções do Azure, incluindo o plano de alojamento de Consumo e como escolher o plano adequado.  

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Dec16_HO1-->


