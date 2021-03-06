---
title: "Identidade híbrida: comparação das ferramentas de integração de diretórios | Microsoft Docs"
description: "Nesta página, encontra uma tabela abrangente que compara as várias ferramentas de integração de diretórios que podem ser utilizadas para a integração de diretórios."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: curtand
ms.assetid: 1e62a4bd-4d55-4609-895e-70131dedbf52
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/14/2017
ms.author: billmath
translationtype: Human Translation
ms.sourcegitcommit: facdbff229eaf5ee20d98214c62c8b2b46acc0fd
ms.openlocfilehash: 80617b203bf5efa4cd6bed590ea937b2e68db9e1


---
# <a name="hybrid-identity-directory-integration-tools-comparison"></a>Comparação das ferramentas de integração de diretórios de identidade híbrida
Ao longo dos anos, as ferramentas de integração de diretórios cresceram e evoluíram.  Este documento tem o objetivo de ajudar a fornecer uma visão consolidada destas ferramentas e uma comparação das funcionalidades que estão disponíveis em cada uma.

<!-- The hardcoded link is a workaround for campaign ids not working in acom links-->

> [!NOTE]
> O Azure AD Connect inclui os componentes e as funcionalidades já disponibilizadas como o Dirsync e o AAD Sync. Estas ferramentas deixaram de ser disponibilizadas individualmente e todos os melhoramentos futuros serão incluídos nas atualizações do Azure AD Connect para que saiba sempre onde pode obter as funcionalidades mais recentes.
> 
> O DirSync e o Azure AD Sync foram preteridos. Pode encontrar mais informações [aqui](active-directory-aadconnect-dirsync-deprecated.md).
> 
> 

Utilize a seguinte chave para cada uma das tabelas.

● = Atualmente Disponível  
FR = Versão Futura  
PP = Pré-visualização Pública  

## <a name="on-premises-to-cloud-synchronization"></a>Local para a Sincronização de Nuvem
| Funcionalidade | Azure Active Directory Connect | Serviços de Sincronização do Azure Active Directory (AAD Sync) | Ferramenta de Sincronização do Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Ligar a uma única floresta do AD no local |● |● |● |● |● |
| Ligar a várias florestas do AD no local |● |● | |● |● |
| Ligar a várias Organizações do Exchange no local |● | | | | |
| Ligar a um único diretório LDAP no local |FR | | |● |● |
| Ligar a vários diretórios LDAP no local |FR | | |● |● |
| Ligar a diretórios AD no local e diretórios LDAP no local |FR | | |● |● |
| Ligar a sistemas personalizados (por exemplo, SQL Server, Oracle, MySQL, etc.) |FR | | |● |● |
| Sincronizar atributos definidos pelo cliente (extensões de diretório) |● | | | | |
| Ligar a HR no local (por exemplo, SAP, Oracle eBusiness, PeopleSoft) |FR | | |● |● |
| Suporta as regras de sincronização do FIM e os conectores para o aprovisionamento para sistemas no local. | | | |● |● |

## <a name="cloud-to-on-premises-synchronization"></a>Nuvem para a Sincronização no Local
| Funcionalidade | Azure Active Directory Connect | Serviços de Sincronização do Azure Active Directory | Ferramenta de Sincronização do Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Repetição de escrita de dispositivos |● | |● | | |
| Repetição de escrita de atributos (para implementação híbrida do Exchange) |● |● |● |● |● |
| Repetição de escrita de objetos de grupos |● | | | | |
| Repetição de escrita de palavras-passe (na reposição de personalizada de palavra-passe (SSPR) e alteração de palavra-passe) |● |● | | | |

## <a name="authentication-feature-support"></a>Suporte de funcionalidades de autenticação
| Funcionalidade | Azure Active Directory Connect | Serviços de Sincronização do Azure Active Directory | Ferramenta de Sincronização do Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Sincronização de Palavras-passe para a floresta única do AD no local |● |● |● | | |
| Sincronização de Palavras-passe para várias florestas do AD no local |● |● | | | |
| Início de Sessão Único com Federação |● |● |● |● |● |
| Repetição de escrita de palavras-passe (na SSPR e alteração de palavra-passe) |● |● | | | |

## <a name="set-up-and-installation"></a>Configuração e Instalação
| Funcionalidade | Azure Active Directory Connect | Serviços de Sincronização do Azure Active Directory | Ferramenta de Sincronização do Azure Active Directory (DirSync) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|
| Suporta a instalação num Controlador de Domínio |● |● |● | |
| Suporta a instalação utilizando o SQL Express |● |● |● | |
| Atualização fácil a partir do DirSync |● | | | |
| Localização do Admin UX para idiomas do servidor do Windows |● |● |● | |
| Localização do UX do utilizador final para idiomas do Windows Server | | | |● |
| Suporte para Windows Server 2008 e Windows Server 2008 R2 |● para sincronização, não para a federação |● |● |● |
| Suporte para Windows Server 2012 e Windows Server 2012 R2 |● |● |● |● |

## <a name="filtering-and-configuration"></a>Filtragem e Configuração
| Funcionalidade | Azure Active Directory Connect | Serviços de Sincronização do Azure Active Directory | Ferramenta de Sincronização do Azure Active Directory (DirSync) | Forefront Identity Manager 2010 R2 (FIM) | Microsoft Identity Manager 2016 (MIM) |
|:--- |:---:|:---:|:---:|:---:|:---:|
| Filtrar domínios e unidades organizacionais |● |● |● |● |● |
| Filtrar valores de atributo dos objetos |● |● |● |● |● |
| Permitir que um conjunto mínimo de atributos seja sincronizado (MinSync) |● |● | | | |
| Permitir que diferentes modelos de serviço sejam aplicados a fluxos de atributos |● |● | | | |
| Permitir a remoção de atributos no sentido do AD para o Azure AD |● |● | | | |
| Permitir a personalização avançada de fluxos de atributos |● |● | |● |● |

## <a name="next-steps"></a>Passos seguintes
Saiba mais sobre como [Integrar as identidades no local ao Azure Active Directory](active-directory-aadconnect.md).




<!--HONumber=Jan17_HO3-->


