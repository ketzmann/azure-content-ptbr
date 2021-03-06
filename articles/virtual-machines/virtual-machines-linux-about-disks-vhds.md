<properties
	pageTitle="Sobre discos e VHDs para VMs Linux | Microsoft Azure"
	description="Conheça o básico sobre discos e VHDs para máquinas virtuais Linux no Azure."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="cynthn"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager,azure-service-management"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/16/2016"
	ms.author="cynthn"/>

# Sobre discos e VHDs para máquinas virtuais do Azure

Assim como qualquer outro computador, as máquinas virtuais do Azure usam os discos como locais onde armazenar um sistema operacional, aplicativos e dados. Todas as máquinas virtuais do Azure têm pelo menos dois discos - um disco do sistema operacional Linux (no caso de uma VM Linux) e um disco temporário. O disco do sistema operacional é criado por meio de uma imagem, e o disco do sistema operacional e a imagem na verdade são VHDs (discos rígidos virtuais) armazenados em uma conta de armazenamento do Azure. Máquinas virtuais também podem ter um ou mais discos de dados que também são armazenados em VHDs. Este artigo também está disponível para [máquinas virtuais do Windows](virtual-machines-windows-about-disks-vhds.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

[AZURE.INCLUDE [virtual-machines-common-about-disks-vhds](../../includes/virtual-machines-common-about-disks-vhds.md)]

## Solucionar problemas
[AZURE.INCLUDE [virtual-machines-linux-lunzero](../../includes/virtual-machines-linux-lunzero.md)]

## Próximas etapas

-  [Anexar um disco](virtual-machines-linux-attach-disk-portal.md) para adicionar mais armazenamento à sua VM.
-  [Configure o RAID de software](virtual-machines-linux-configure-raid.md) para redundância.
-  [Capturar uma máquina virtual do Linux](virtual-machines-linux-classic-capture-image.md) para implantar rapidamente VMs adicionais.

<!---HONumber=AcomDC_0803_2016-->