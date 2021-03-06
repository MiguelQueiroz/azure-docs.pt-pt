---
title: "Controlo de Acesso à Base de Dados SQL do Azure | Microsoft Docs"
description: "Saiba como controlar o acesso à Base de Dados SQL do Microsoft Azure."
services: sql-database
documentationcenter: 
author: BYHAM
manager: jhubbard
editor: 
tags: 
ms.assetid: 8e71b04c-bc38-4153-8f83-f2b14faa31d9
ms.service: sql-database
ms.custom: overview
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 10/18/2016
ms.author: rickbyh
translationtype: Human Translation
ms.sourcegitcommit: 356cc4c6d8e25d36880e4b12bf471326e61990c3
ms.openlocfilehash: f12ed9d76e7c6db5e14ed3c00d7d4087dbd4069c


---
# <a name="azure-sql-database-access-control"></a>Controlo de acesso à Base de Dados SQL do Azure
Para fornecer segurança, a Base de Dados SQL controla o acesso com regras de firewall que limitam a conectividade por endereço IP, mecanismos de autenticação que exigem que os utilizadores provem a sua identidade e mecanismos de autorização que limitam os utilizadores a ações e dados específicos. 

> [!IMPORTANT]
> Para obter uma descrição geral das funcionalidades de segurança da Base de Dados SQL, veja [Descrição geral da segurança de SQL](sql-database-security-overview.md). Para obter um tutorial sobre como utilizar a autenticação do SQL Server, veja [SQL Database tutorial: SQL Server authentication, logins and user accounts, database roles, permissions, server-level firewall rules, and database-level firewall rules (Tutorial da Base de Dados SQL: autenticação do SQL Server, inícios de sessão e contas de utilizador, funções de base de dados, permissões, regras de firewall ao nível do servidor e regras de firewall ao nível da base de dados)](sql-database-control-access-sql-authentication-get-started.md). Para obter um tutorial sobre como utilizar a autenticação do Azure Active Directory, veja [SQL Database tutorial: AAD authentication, logins and user accounts, database roles, permissions, server-level firewall rules, and database-level firewall rules (Tutorial da Base de Dados SQL: autenticação do AAD, inícios de sessão e contas de utilizador, funções de base de dados, permissões, regras de firewall ao nível do servidor e regras de firewall ao nível da base de dados)](sql-database-control-access-aad-authentication-get-started.md).

>

## <a name="firewall-and-firewall-rules"></a>Firewall e regras de firewall
A Base de Dados SQL do Microsoft Azure disponibiliza um serviço de bases de dados relacionais para o Azure e outras aplicações baseadas na Internet. Para ajudar a proteger os seus dados, as firewalls impedem todos os acessos ao seu servidor de base de dados enquanto não especificar que computadores têm acesso. A firewall concede acesso a bases de dados com base no endereço IP de origem de cada pedido. Para obter mais informações, veja [Descrição geral das regras de firewall da Base de Dados SQL do Azure](sql-database-firewall-configure.md).

O serviço da Base de Dados SQL do Azure só está disponível através da porta TCP 1433. Para aceder a uma Base de Dados SQL a partir do seu computador, certifique-se de que a firewall do computador de cliente permite comunicação TCP de saída na porta TCP 1433. Se não for necessário para outras aplicações, bloqueie as ligações de entrada na porta TCP 1433. 

Como parte do processo de ligação, as ligações a partir de máquinas virtuais do Azure são redirecionadas para um endereço IP diferente e a porta exclusiva para cada função de trabalho. O número de porta está no intervalo de 11000 a 11999. Para mais informações sobreportas TCP, consulte [Ports beyond 1433 for ADO.NET 4.5 and SQL Database V12 (Portas para além do 1433 para ADO.NET 4.5 e Base de Dados SQL V12)](sql-database-develop-direct-route-ports-adonet-v12.md).

## <a name="authentication"></a>Autenticação

A Base de Dados SQL suporta dois tipos de autenticação:

* **Autenticação SQL**, que utiliza um nome de utilizador e palavra-passe. Quando criou o servidor lógico para a sua base de dados, especificou um início de sessão "administrador do servidor" com um nome de utilizador e palavra-passe. Com estas credenciais, pode autenticar-se em qualquer base de dados nesse servidor como o proprietário da base de dados ou "dbo". 
* **Autenticação do Azure Active Directory**, que utiliza identidades geridas pelo Azure Active Directory e é suportada para domínios geridos e integrados. Utilize a autenticação do Active Directory (segurança integrada) [sempre que possível](https://msdn.microsoft.com/library/ms144284.aspx). Se pretender utilizar a autenticação do Azure Active Directory, tem de criar outro administrador de servidor, denominado "Administrador do Azure AD", que tem permissão para administrar utilizadores e grupos do Azure AD. Este administrador também pode fazer todas as operações que um administrador de servidor normal faz. Veja [Connecting to SQL Database By Using Azure Active Directory Authentication (Utilizar a Autenticação do Azure Active Directory para Ligar à Base de Dados SQL)](sql-database-aad-authentication.md) para obter instruções sobre como criar um administrador do Azure AD e ativar a Autenticação do Azure Active Directory.

O Motor de Base de Dados fecha ligações que permanecem inativas durante mais de 30 minutos. A ligação tem de iniciar sessão novamente para que possa ser utilizada. As ligações continuamente ativas à Base de Dados SQL precisam de reautorização (realizada pelo motor de base de dados) pelo menos a cada 10 horas. O motor de base de dados tenta a reautorização com a palavra-passe originalmente submetida e sem intervenção do utilizador. Por motivos de desempenho, quando uma palavra-passe é reposta na Base de Dados SQL, a ligação não pode ser reautenticada, mesmo que a ligação seja reposta devido ao conjunto de ligações. Isto é diferente do comportamento do SQL Server no local. Se a palavra-passe tiver sido alterada porque a ligação foi inicialmente autorizada, a ligação tem de ser terminada e uma nova ligação efetuada com a nova palavra-passe. Um utilizador com permissão ELIMINAR LIGAÇÃO DE BASE DE DADOS pode terminar uma ligação explicitamente na Base de Dados SQL com o comando [ELIMINAR](https://msdn.microsoft.com/library/ms173730.aspx).

As contas de utilizador podem ser criadas na base de dados mestra e podem ser concedidas permissões às mesmas em todas as bases de dados no servidor, ou podem ser criadas na própria base de dados (chamados utilizadores contidos). Para obter informações sobre como criar e gerir inícios de sessão, veja [Gerir inícios de sessão](sql-database-manage-logins.md). Para melhorar a portabilidade e a escalabilidade, utilize utilizadores de base de dados contida. Para obter mais informações sobre utilizadores contidos, veja [Contained Database Users - Making Your Database Portable (Utilizadores de Bases de Dados Contidas - Tornar a Base de Dados Portátil)](https://msdn.microsoft.com/library/ff929188.aspx), [CREATE USER (Transact-SQL)](https://technet.microsoft.com/library/ms173463.aspx), e [Contained Databases (Bases de Dados Contidas)](https://technet.microsoft.com/library/ff929071.aspx).

Como melhor prática, a sua aplicação deve utilizar uma conta diferente para se autenticar. Desta forma, pode limitar as permissões concedidas à aplicação e reduzir os riscos de atividades maliciosas, caso o código da aplicação seja vulnerável a um ataque de injeção SQL. A abordagem recomendada consiste em criar um [utilizador de base de dados contido](https://msdn.microsoft.com/library/ff929188), que permite que a sua aplicação se autentique diretamente na base de dados. Pode criar um utilizador de base de dados contido que utilize a Autenticação SQL ao executar o comando T-SQL seguinte enquanto está ligado à sua base de dados de utilizador como o início de sessão de administrador do servidor:

## <a name="authorization"></a>Autorização

A autorização diz respeito ao que um utilizador pode fazer dentro de uma Base de Dados SQL do Azure e é controlada pelas [permissões de nível de objeto](https://msdn.microsoft.com/library/ms189121) e [associações de funções](https://msdn.microsoft.com/library/ms191291.aspx) de base de dados da sua conta de utilizador. Como melhor prática, deverá conceder aos utilizadores o mínimo de privilégios necessários. A conta de administrador do servidor que está a ligar é membro de db_owner, que tem autoridade para fazer todas as operações na base de dados. Guarde esta conta para a implementação de atualizações de esquema e outras operações de gestão. Utilize a conta "ApplicationUser" com permissões mais limitadas para se ligar da sua aplicação à base de dados com o mínimo de privilégios necessários para a sua aplicação. Para obter mais informações, veja [Gerir inícios de sessão](sql-database-manage-logins.md).

Normalmente, apenas os administradores necessitam de aceder à base de dados mestra. O acesso de rotina a cada base de dados do utilizador deve ser feito através dos utilizadores de base de dados contida não-administradores criados em cada base de dados. Quando utiliza utilizadores de base de dados contida, não precisa de criar inícios de sessão na base de dados mestra. Para obter mais informações, veja [Contained Database Users - Making Your Database Portable (Utilizadores de Base de Dados Contidos - Tornar a Sua Base de Dados Portátil)](https://msdn.microsoft.com/library/ff929188.aspx).

Além disso, estas funcionalidades podem ser utilizadas para limitar ou elevar permissões.

* A [representação](https://msdn.microsoft.com/library/vstudio/bb669087) e a [assinatura de módulo](https://msdn.microsoft.com/library/bb669102) podem ser utilizadas para elevar temporariamente permissões de forma segura.
* A [Segurança ao Nível da Linha](https://msdn.microsoft.com/library/dn765131) pode ser utilizada para limitar as linhas a que um utilizador pode aceder.
* A [Máscara de Dados](sql-database-dynamic-data-masking-get-started.md) pode ser utilizada para limitar a exposição de dados confidenciais.
* Os [Procedimentos armazenados](https://msdn.microsoft.com/library/ms190782) podem ser utilizados para limitar as ações que podem ser realizadas na base de dados.

## <a name="next-steps"></a>Passos seguintes

- Para obter uma descrição geral de todas as funcionalidades de segurança da Base de Dados SQL, veja [Descrição geral da segurança de SQL](sql-database-security-overview.md).
- Para saber mais sobre regras de firewall, veja [Firewall da Base de Dados SQL do Azure](sql-database-firewall-configure.md).
- Para saber mais sobre utilizadores e inícios de sessão, veja [Gerir inícios de sessão](sql-database-manage-logins.md). 
- Para ver um debate da utilização das funcionalidades de proteção de dados na Base de Dados SQL, veja [Proteção de dados e segurança](sql-database-protect-data.md).
- Para ver um debate sobre a monitorização proativa, veja [Get started with SQL Database Auditing (Introdução à Auditoria da Base de Dados SQL)](sql-database-auditing-get-started.md) e [Get started with SQL Database Threat Detection (Introdução à Deteção de Ameaças da Base de Dados SQL)](sql-database-threat-detection-get-started.md).
- Para obter um tutorial sobre como utilizar a autenticação do SQL Server, veja [SQL Database tutorial: SQL Server authentication, logins and user accounts, database roles, permissions, server-level firewall rules, and database-level firewall rules (Tutorial da Base de Dados SQL: autenticação do SQL Server, inícios de sessão e contas de utilizador, funções de base de dados, permissões, regras de firewall ao nível do servidor e regras de firewall ao nível da base de dados)](sql-database-control-access-sql-authentication-get-started.md).
- Para obter um tutorial sobre como utilizar a autenticação do Azure Active Directory, veja [SQL Database tutorial: AAD authentication, logins and user accounts, database roles, permissions, server-level firewall rules, and database-level firewall rules (Tutorial da Base de Dados SQL: autenticação do AAD, inícios de sessão e contas de utilizador, funções de base de dados, permissões, regras de firewall ao nível do servidor e regras de firewall ao nível da base de dados)](sql-database-control-access-aad-authentication-get-started.md).



<!--HONumber=Jan17_HO3-->


