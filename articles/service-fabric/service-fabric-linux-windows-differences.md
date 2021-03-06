---
title: "Diferenças do Azure Service Fabric entre o Linux e o Windows | Microsoft Docs"
description: "Diferenças entre a Pré-visualização do Azure Service Fabric no Linux e no Windows."
services: service-fabric
documentationcenter: .net
author: mani-ramaswamy
manager: timlt
editor: 
ms.assetid: d552c8cd-67d1-45e8-91dc-871853f44fc6
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 03/23/2017
ms.author: subramar
translationtype: Human Translation
ms.sourcegitcommit: 503f5151047870aaf87e9bb7ebf2c7e4afa27b83
ms.openlocfilehash: 5faf7dc0b544f6fe2f83565cc368e218c6df35af
ms.lasthandoff: 03/28/2017


---
# <a name="differences-between-service-fabric-on-linux-preview-and-windows-generally-available"></a>Diferenças entre o Service Fabric no Linux (pré-visualização) e no Windows (disponível em geral)

Uma vez que o Service Fabric no Linux é uma pré-visualização, existem algumas funcionalidades que são suportadas no Windows, mas não no Linux. Eventualmente, os conjuntos de funcionalidades ficarão em paridade quando o Service Fabric no Linux se torna disponível em geral.

* As coleções fiáveis (e Serviços com Estado Fiável) não são suportadas no Linux.
* O ReverseProxy não está disponível no Linux.
* O instalador autónomo não está disponível no Linux.
* A validação do esquema XML para os ficheiros de manifesto não é realizada no Linux. 
* O redirecionamento de consola não é suportado no Linux. 
* O Serviço de Análise de Falhas (FAS) não está disponível no Linux.
* O suporte do Azure Active Directory não está disponível no Linux.
* Alguns equivalentes do comando CLI de comandos do Powershell não estão disponíveis.
* Apenas um subconjunto de comandos do Powershell pode ser executado comparativamente a um cluster do Linux (conforme expandido na secção seguinte).

>[!NOTE]
>O redirecionamento da consola não é suportado em clusters de produção, mesmo no Windows.

As ferramentas de desenvolvimento são diferentes com o VisualStudio, o Powershell, o VSTS e o ETW a serem utilizados no Windows e no Yeoman, no Eclipse, no Jenkins e no LTTng utilizados no Linux.

## <a name="powershell-cmdlets-that-do-not-work-against-a-linux-service-fabric-cluster"></a>Cmdlets do PowerShell que não funcionam num cluster do Service Fabric do Linux

* Invoke-ServiceFabricChaosTestScenario
* Invoke-ServiceFabricFailoverTestScenario
* Invoke-ServiceFabricPartitionDataLoss
* Invoke-ServiceFabricPartitionQuorumLoss
* Restart-ServiceFabricPartition
* Start-ServiceFabricNode
* Stop-ServiceFabricNode
* Get-ServiceFabricImageStoreContent
* Get-ServiceFabricChaosReport
* Get-ServiceFabricNodeTransitionProgress
* Get-ServiceFabricPartitionDataLossProgress
* Get-ServiceFabricPartitionQuorumLossProgress
* Get-ServiceFabricPartitionRestartProgress
* Get-ServiceFabricTestCommandStatusList
* Remove-ServiceFabricTestState
* Start-ServiceFabricChaos
* Start-ServiceFabricNodeTransition
* Start-ServiceFabricPartitionDataLoss
* Start-ServiceFabricPartitionQuorumLoss
* Start-ServiceFabricPartitionRestart
* Stop-ServiceFabricChaos
* Stop-ServiceFabricTestCommand
* Cmd
* Get-ServiceFabricNodeConfiguration
* Get-ServiceFabricClusterConfiguration
* Get-ServiceFabricClusterConfigurationUpgradeStatus
* Get-ServiceFabricPackageDebugParameters
* New-ServiceFabricPackageDebugParameter
* New-ServiceFabricPackageSharingPolicy
* Add-ServiceFabricNode
* Copy-ServiceFabricClusterPackage
* Get-ServiceFabricRuntimeSupportedVersion
* Get-ServiceFabricRuntimeUpgradeVersion
* New-ServiceFabricCluster
* New-ServiceFabricNodeConfiguration
* Remove-ServiceFabricCluster
* Remove-ServiceFabricClusterPackage
* Remove-ServiceFabricNodeConfiguration
* Test-ServiceFabricClusterManifest
* Test-ServiceFabricConfiguration
* Update-ServiceFabricNodeConfiguration
* Approve-ServiceFabricRepairTask
* Complete-ServiceFabricRepairTask
* Get-ServiceFabricRepairTask
* Invoke-ServiceFabricDecryptText
* Invoke-ServiceFabricEncryptSecret
* Invoke-ServiceFabricEncryptText
* Invoke-ServiceFabricInfrastructureCommand
* Invoke-ServiceFabricInfrastructureQuery
* Remove-ServiceFabricRepairTask
* Start-ServiceFabricRepairTask
* Stop-ServiceFabricRepairTask
* Update-ServiceFabricRepairTaskHealthPolicy



## <a name="next-steps"></a>Passos seguintes
* [Preparar o ambiente de desenvolvimento no Linux](service-fabric-get-started-linux.md)
* [Prepare your development environment on OSX (Preparar o ambiente de desenvolvimento no OSX)](service-fabric-get-started-mac.md)
* [Criar e implementar a sua primeira aplicação Java do Service Fabric no Linux com o Yeoman](service-fabric-create-your-first-linux-application-with-java.md)
* [Criar e implementar a sua primeira aplicação Java do Service Fabric no Linux com o Plug-in do Service Fabric para Eclipse](service-fabric-get-started-eclipse.md)
* [Create your first CSharp application on Linux (Criar a sua primeira aplicação CSharp no Linux)](service-fabric-create-your-first-linux-application-with-csharp.md)
* [Use the Azure CLI to manage your Service Fabric applications (Utilizar a CLI do Azure para gerir as aplicações do Service Fabric)](service-fabric-azure-cli.md)

