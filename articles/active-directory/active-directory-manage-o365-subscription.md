---
title: "Gerir o diretório da subscrição do Office 365 no Azure | Microsoft Docs"
description: "Gerir um diretório de subscrição do Office 365 com o Azure Active Directory e o Portal Clássico do Azure"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 746987b7-2dfd-4b35-819d-37c1b65c5c81
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/13/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: a4a78b92c8bb9e8aff25fd724ed78283de8f2fd8
ms.openlocfilehash: 2fda7dc07d01bcd0629d1fa80ec9262e80af4c5f


---
# <a name="manage-the-directory-for-your-office-365-subscription-in-azure"></a>Gerir o diretório da subscrição do Office 365 no Azure
Este artigo descreve como gerir um diretório criado para uma subscrição do Office 365 utilizando o portal clássico do Azure. Tem de ser Administrador de Serviço ou coadministrador de uma subscrição do Azure para iniciar sessão no portal clássico do Azure. Se ainda não tiver uma subscrição do Azure, pode inscrever-se para obter uma [avaliação de 30 dias gratuita](https://azure.microsoft.com/trial/get-started-active-directory/) hoje e implementar a sua primeira solução em nuvem em menos de 5 minutos, utilizando a seguinte ligação. Certifique-se de que usa a conta de estudante ou profissional que utiliza para iniciar sessão no Office 365.

Depois de concluir a subscrição do Azure, pode iniciar sessão no Portal Clássico do Azure e aceder aos serviços do Azure. Clique na extensão do Active Directory para gerir o mesmo diretório que autentica os utilizadores do Office 365.

Se já tiver uma subscrição do Azure, o processo para gerir um diretório adicional também é simples. Por exemplo, Miguel Cardoso poderá ter uma subscrição do Office 365 para a Contoso.com. Ele também tem uma subscrição do Azure para a qual se inscreveu utilizando a conta Microsoft, msmith@hotmail.com. Neste caso, ele gere dois diretórios.

| Subscrição | Office 365 | Azure |
| --- | --- | --- |
|   Nome a apresentar |Contoso |Diretório do Azure Active Directory (Azure AD) predefinido |
|   Nome de domínio |contoso.com |mcardosohotmail.onmicrosoft.com |

Ele pretende gerir as identidades de utilizador no diretório Contoso enquanto tem sessão iniciada no Azure com a conta Microsoft, para poder ativar funcionalidades do Azure AD como a autenticação multifator. O diagrama seguinte pode ajudar a ilustrar o processo.

![Diagrama para gerir dois diretórios independentes](./media/active-directory-manage-o365-subscription/AAD_O365_03.png)

Neste caso, os dois diretórios são independentes entre si.

## <a name="to-manage-two-independent-directories"></a>Para gerir dois diretórios independentes
Para Miguel Cardoso gerir os dois diretórios enquanto tiver sessão iniciada no Azure como msmith@hotmail.com,, tem de concluir os seguintes passos:

> [!NOTE]
> Estes passos só podem ser concluídos quando o utilizador tiver iniciado sessão com uma conta Microsoft. Se o utilizador tiver iniciado sessão com uma conta de estudante ou profissional, a opção **Utilizar diretório existente** não está disponível. Uma conta de estudante ou profissional só pode ser autenticada pelo respetivo diretório raiz (ou seja, o diretório onde está armazenada a conta escolar ou profissional e que é propriedade da empresa ou escola).
>
>

1. Inicie sessão no [Portal Clássico do Azure](https://manage.windowsazure.com) como msmith@hotmail.com.
2. Clique em **Novo** > **Serviços de aplicações** > **Active Directory** > **Diretório** > **Criação Personalizada**.
3. Clique em Utilizar diretório existente e marque a caixa de verificação **Estou pronto para terminar sessão agora**.
4. Inicie sessão no Portal Clássico do Azure como administrador global da Contoso.onmicrosoft.com (por exemplo, msmith@contoso.com)).
5. Quando aparecer **Utilizar o diretório da Contoso com o Azure?**, clique em **Continuar**.
6. Clique em **Terminar sessão agora**.
7. Inicie sessão no Portal Clássico do Azure como msmith@hotmail.com. O diretório da Contoso e o diretório predefinido aparecem na extensão do Active Directory.

Depois de concluir estes passos, msmith@hotmail.com é um administrador global no diretório da Contoso.

## <a name="to-administer-resources-as-the-global-admin"></a>Para administrar recursos como administrador global
Agora vamos imaginar que Júlia Martins necessita de administrar Websites e recursos da base de dados associados à subscrição do Azure de msmith@hotmail.com. Para o poder fazer, Miguel Cardoso precisa de concluir estes passos adicionais:

1. Iniciar sessão no [Portal Clássico do Azure](https://manage.windowsazure.com) utilizando a conta de Administrador de Serviços da subscrição do Azure (neste exemplo, msmith@hotmail.com).
2. Transferir a subscrição para o diretório da Contoso: clicar em **Definições** > **Subscrições** > selecionar a subscrição > **Editar Diretório** > selecionar **Contoso (Contoso.com)**. Como parte da transferência, será removida qualquer conta de estudante ou profissional pertencente a coadministradores da subscrição.
3. Adicionar a Júlia Martins como coadministradora da subscrição: clicar em **Definições** > **administradores** > selecionar a subscrição > **Adicionar** > introduzir **JohnDoe@Contoso.com**.

## <a name="next-steps"></a>Passos seguintes
Para mais informações sobre a relação entre subscrições e diretórios, consulte [Como associar uma subscrição a um diretório](active-directory-how-subscriptions-associated-directory.md).



<!--HONumber=Feb17_HO2-->


