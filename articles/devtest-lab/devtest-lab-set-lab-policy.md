<properties
	pageTitle="Definir políticas de laboratório | Microsoft Azure"
	description="Aprenda a definir as políticas do laboratório, como os tamanhos das VMs, o número máximo de VMs por usuário e o desligamento automático."
	services="devtest-lab,virtual-machines"
	documentationCenter="na"
	authors="tomarcher"
	manager="douge"
	editor=""/>

<tags
	ms.service="devtest-lab"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/01/2016"
	ms.author="tarcher"/>

# Definir políticas de laboratório

> [AZURE.VIDEO how-to-set-vm-policies-in-a-devtest-lab]

## Visão geral

Com os Laboratórios de Desenvolvimento/Testes, você pode especificar as principais políticas que controlam como o seu laboratório e suas VMs são usados. Por exemplo, você pode definir as regras para os tamanhos de VM permitidos para criar VMs, o limite para o número de VMs que podem ser criadas, além de agendar trabalhos para iniciar/parar as VMs de laboratório automaticamente.

## Acessar as políticas do laboratório

Para exibir (e alterar) as políticas de um laboratório, siga estas etapas:

1. Entre no [Portal do Azure](http://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecione **Procurar**, e, em seguida, **DevTest Labs** na lista.

1. Na lista de laboratórios, selecione o laboratório desejado.

1. Escolha a opção **Configurações**.

	![Configurações](./media/devtest-lab-set-lab-policy/lab-blade-settings.png)

1. Na folha **Configurações**, há um agrupamento de configurações chamado **Políticas de VM**.

	![Configurações](./media/devtest-lab-set-lab-policy/policies.png)

	Selecione a política desejada na lista abaixo para saber mais sobre como configurá-la:

	- [Tamanhos de VM permitidos](#set-allowed-vm-sizes): selecione a lista de tamanhos de VM permitidos no laboratório. O usuário só pode criar VMs a partir dessa lista.

	- [Máximo de VMs por usuário](#set-maximum-vms-per-user) -especifique o número máximo de máquinas virtuais que podem ser criadas por um usuário.

	- [Total de VMs permitido](#set-total-vms-allowed) -especifique o número máximo de máquinas virtuais que podem ser criadas para um laboratório.

	- [Desligamento automático](#set-auto-shutdown) -especifique a hora em que as VMs de um laboratório atual serão desligadas automaticamente.

	- [Inicialização automática](#set-auto-start) -especifique a hora em que as VMs de um laboratório atual serão iniciadas automaticamente.

## Definir os tamanhos de VM permitidos

A política para definir os tamanhos de VM permitidos ajuda a minimizar o desperdício de laboratório, permitindo que você especifique quais tamanhos de VM são permitidos no laboratório. Quando essa política é ativada, somente os tamanhos de VM nesta lista podem ser usados para criar VMs.

1. Na folha **Configurações** do laboratório, em **Políticas de VM**, selecione **Tamanhos de VM Permitidos**.

	![Configurações](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)
 
1. Selecione **Ativado** para habilitar essa política e **Desativado** para desabilitá-la.

1. Se você habilitar essa política, selecione um ou mais tamanhos de VM que podem ser criados no laboratório.

1. Selecione **Salvar**.

## Definir o número máximo de VMs por usuário

A política para o **Número máximo de VMs por usuário** permite que você especifique o número máximo de VMs que podem ser criadas por um usuário individual. Se um usuário tentar criar uma nova VM quando o limite de usuários for alcançado, uma mensagem de erro indicará que a VM não poderá ser criada.

1. Na folha **Configurações** do laboratório, em **Políticas de VM**, selecione **Número Máximo de VMs por usuário**.

	![Configurações](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Selecione **Ativado** para habilitar essa política e **Desativado** para desabilitá-la.

1. Se você habilitar essa política, na caixa de texto **Número máximo de VMs permitido por usuário**, digite um valor numérico indicando o número máximo de VMs que podem ser criadas por um usuário. Se você digitar um número que não é válido, a interface do usuário exibirá o número máximo permitido para esse campo.

1. Selecione **Salvar**.

## Definir número total de VMs permitidas

A política para o **Número total de VMs permitidas** permite que você especifique o número máximo de VMs que podem ser criadas para o laboratório atual. Se um usuário tentar criar uma nova VM quando o limite de laboratórios for alcançado, uma mensagem de erro indicará que a VM não pode ser criada.

1. Na folha **Configurações** do laboratório, em **Políticas de VM**, selecione **Número total de VMs permitidas**.

	![Configurações](./media/devtest-lab-set-lab-policy/total-vms-allowed.png)

1. Selecione **Ativado** para habilitar essa política e **Desativado** para desabilitá-la.

1. Se você habilitar essa política, na caixa de texto **Número total de VMs permitidas nesse laboratório**, digite um valor numérico indicando o número máximo de VMs que podem ser criadas para o laboratório atual. Se você digitar um número que não é válido, a interface do usuário exibirá o número máximo permitido para esse campo.

1. Selecione **Salvar**.

## Definir o desligamento automático

A política de desligamento automático ajuda a minimizar o desperdício de laboratório, permitindo que você especifique a hora em que as VMs desse laboratório serão desligadas.

1. Na folha **Configurações** do laboratório, em **Políticas de VM**, selecione **Desligamento automático**.

	![Configurações](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Selecione **Ativado** para habilitar essa política e **Desativado** para desabilitá-la.

1. Se você habilitar essa política, especifique o horário local para desligar todas as VMs do laboratório atual.

1. Selecione **Salvar**.

1. Por padrão, uma vez habilitada, essa política se aplicará a todas as VMs do laboratório atual. Para remover essa configuração de uma VM específica, abra a folha da VM e altere sua configuração de **Desligamento Automático**

## Definir a inicialização automática

A política de início automático permite que você especifique quando as VMs do laboratório atual deverão ser iniciadas.

1. Na folha **Configurações** do laboratório, em **Políticas de VM**, selecione **Inicialização automática**.

	![Configurações](./media/devtest-lab-set-lab-policy/auto-start.png)

1. Selecione **Ativado** para habilitar essa política e **Desativado** para desabilitá-la.

1. Se você habilitar esta política, especifique o horário local de início agendado e os dias da semana para os quais o horário se aplica.

1. Selecione **Salvar**.

1. Quando habilitada, essa política não será aplicada automaticamente a quaisquer máquinas virtuais do laboratório atual. Para aplicar essa configuração a uma VM específica, abra a folha da VM e altere sua configuração de **Início automático**

## Próximas etapas

Depois de definir e aplicar as várias configurações da política de VM em seu laboratório, aqui estão algumas opções para você tentar em seguida:

- [Configurar gerenciamento de custos](./devtest-lab-configure-cost-management.md) - Este artigo ilustra como usar o gráfico **Tendência de custo estimado mensal** para exibir o custo estimado até a data do mês atual, bem como o custo projetado do final do mês para o mês atual.
- [Criar imagem personalizada](./devtest-lab-create-template.md) - Durante a criação de uma VM, você especifica uma base, que pode ser uma imagem personalizada ou uma imagem do Marketplace. Este artigo ilustra como criar uma imagem personalizada de um arquivo VHD.
- [Configurar imagens do Marketplace](./devtest-lab-configure-marketplace-images.md) - Os Laboratórios de Desenvolvimento/Teste dão suporte à criação de novas VMs com base em imagens do Azure Marketplace. Este artigo ilustra como especificar quais imagens (se houver alguma) do Azure Marketplace podem ser usadas durante a criação de novas VMs em um laboratório.
- [Criar uma VM em um laboratório](./devtest-lab-add-vm-with-artifacts.md): ilustra como criar uma nova VM de uma imagem de base (personalizada ou do Marketplace) e como trabalhar com artefatos em sua VM.

<!---HONumber=AcomDC_0803_2016-->