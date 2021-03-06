---
title: Migrar para o Azure com o Site Recovery | Microsoft Docs
description: "Este artigo disponibiliza uma descrição geral da migração de VMs e servidores físicos para o Azure com o Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: c413efcd-d750-4b22-b34b-15bcaa03934a
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/27/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: cfe4957191ad5716f1086a1a332faf6a52406770
ms.openlocfilehash: 981155c38bdc8cb54639d2271be1f3bd3036125c
ms.lasthandoff: 03/09/2017


---
# <a name="migrate-to-azure-with-site-recovery"></a>Migrar para o Azure com o Site Recovery

Leia este artigo para obter uma descrição geral de como utilizar o serviço Azure Site Recovery para migrar máquinas virtuais e servidores físicos.

O Site Recovery é um serviço do Azure que contribui para a sua estratégia de BCDR (Continuidade de Negócio e Recuperação Após Desastre) através da orquestração da replicação dos servidores físicos no local e das máquinas virtuais para a cloud (Azure) ou para um datacenter secundário. Quando ocorrem falhas na sua localização principal, faz-se a ativação pós-falha para a localização secundária para manter disponíveis as aplicações e cargas de trabalho. A localização principal é reativada quando se retomam as operações normais. Saiba mais em [O que é a Recuperação de Sites?](site-recovery-overview.md) Também pode utilizar o Site Recovery para migrar as cargas de trabalho existentes no local para o Azure, para agilizar a sua viagem na cloud e beneficiar da matriz de funcionalidades que o Azure disponibiliza.

Para uma descrição geral rápida sobre como efetuar a migração, consulte este vídeo.
>[!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/ASRHowTo-Video2-Migrate-Virtual-Machines-to-Azure/player]

Este artigo descreve a implementação no [portal do Azure](https://portal.azure.com). O [portal clássico do Azure](https://manage.windowsazure.com/) pode ser utilizado para manter cofres existentes do Site Recovery, mas não pode criar cofres novos.

Publique os seus comentários na parte inferior do artigo. Coloque questões técnicas no [Fórum de Serviços de Recuperação do Azure](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="what-do-we-mean-by-migration"></a>O que queremos dizer com migração?

Pode implementar o Site Recovery para a replicação de VMs no local e os servidores físicos para o Azure ou para um site secundário. Pode replicar máquinas, efetuar a ativação pós-falha desde o site primário quando ocorrerem falhas e reativar a ativação pós-falha para o site primário quando recuperar. Além disto, pode utilizar o Site Recovery para migrar VMs e servidores físicos para o Azure, para que os utilizadores possam aceder às mesmas a partir das VMs do Azure. A migração pressupõe a replicação e a ativação pós-falha do site primário para o Azure, e um gesto de migração completo.

## <a name="what-can-site-recovery-migrate"></a>O que pode ser migrado com o Site Recovery?

Pode:

- Migrar cargas de trabalho em execução em VMs de Hyper-V no local, VMs de VMware e servidores físicos para serem executadas em VMs do Azure. Também pode fazer a replicação completa e a reativação pós-falha neste cenário.
- Migrar [VMs de IaaS do Azure](site-recovery-migrate-azure-to-azure.md) entre regiões do Azure. Atualmente, só a migração é suportada neste cenário, o que significa que a reativação pós-falha não o é.
- Migrar [instâncias do Windows AWS](site-recovery-migrate-aws-to-azure.md) para VMs de IaaS do Azure. Atualmente, só a migração é suportada neste cenário, o que significa que a reativação pós-falha não o é.

## <a name="migrate-on-premises-vms-and-physical-servers"></a>Migrar VMs no local e servidores físicos

Para migrar VMs de Hyper-V no local, VMs VMware e servidores físicos, são seguidos quase os mesmo passos que os utilizados na replicação normal. Configura um cofre dos Serviços de Recuperação, configura os servidores de gestão necessários (consoante o que pretende migrar), adiciona-os ao cofre e especifica as definições da replicação. Ativa a replicação nas máquinas que quer migrar e executa uma ativação pós-falha de teste rápida, para garantir que tudo está a trabalhar como deve ser.

Depois de verificar se o seu ambiente de replicação está a funcionar, utiliza uma ativação pós-falha planeada ou não planeada, dependendo do que [é suportado](site-recovery-failover.md) no seu cenário. Para a migração, não precisa de consolidar uma ativação pós-falha. Em vez disso, selecione a opção **Concluir Migração** em cada máquina que pretende migrar. A ação **Concluir Migração** conclui o processo de migração, remove a replicação da máquina e interrompe a faturação do Site Recovery da mesma.

![completemigration](./media/site-recovery-hyper-v-site-to-azure/migrate.png)

## <a name="migrate-between-azure-regions"></a>Migrar entre regiões do Azure

Pode utilizar o Site Recovery para migrar VMs do Azure entre regiões. Neste cenário, só a migração é suportada. Por outras palavras, pode replicar as VMs do Azure e fazer a ativação pós-falha das mesmas para outra região, mas não a reativação pós-falha. Neste cenário, configura um cofre dos Serviços de Recuperação, implementa um servidor de configuração no local para gerir a replicação, adiciona-o ao cofre e especifica as definições da replicação. Ativa a replicação nas máquinas que quer migrar e executa uma ativação pós-falha de teste rápida. Em seguida, executa uma ativação pós-falha não planeada com a opção **Concluir Migração**.

## <a name="migrate-aws-to-azure"></a>Migrar o AWS para o Azure

Pode migrar instâncias do AWS para VMs do Azure. Neste cenário, só a migração é suportada. Por outras palavras, pode replicar as instâncias do AWS e fazer a ativação pós-falha das mesmas para o Azure, mas não a reativação pós-falha. Para fins de migração, as instâncias do AWS são tratadas da mesma forma que os servidores físicos. Configura um cofre dos Serviços de Recuperação, implementa um servidor de configuração no local para gerir a replicação, adiciona-o ao cofre e especifica as definições da replicação. Ativa a replicação nas máquinas que quer migrar e executa uma ativação pós-falha de teste rápida. Em seguida, executa uma ativação pós-falha não planeada com a opção **Concluir Migração**.




## <a name="next-steps"></a>Passos seguintes

- [Migrar VMs VMware para o Azure](site-recovery-vmware-to-azure.md)
- [Migrar VMs de Hyper-V em clouds do VMM para o Azure](site-recovery-vmm-to-azure.md)
- [Migrar VMs de Hyper-V sem o VMM para o Azure](site-recovery-hyper-v-site-to-azure.md)
- [Migrar VMs do Azure entre regiões do Azure](site-recovery-migrate-azure-to-azure.md)
- [Migrar instâncias do AWS para o Azure](site-recovery-migrate-aws-to-azure.md)

