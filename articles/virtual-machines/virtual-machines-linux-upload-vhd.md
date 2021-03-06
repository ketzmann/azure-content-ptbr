<properties
	pageTitle="Criar e carregar uma imagem personalizada do Linux | Microsoft Azure"
	description="Crie e carregue um VHD (disco rígido virtual) no Azure com uma imagem personalizada do Linux usando o modelo de implantação do Resource Manager."
	services="virtual-machines-linux"
	documentationCenter=""
	authors="iainfoulds"
	manager="timlt"
	editor="tysonn"
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machines-linux"
	ms.workload="infrastructure-services"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/15/2016"
	ms.author="iainfou"/>

# Carregar e criar uma VM com base em uma imagem de disco personalizada

Este artigo mostra como carregar um VHD (disco rígido virtual) usando o modelo de implantação do Resource Manager e criar VMs com base nessa imagem personalizada. Essa funcionalidade permite que você instale e configure uma distribuição do Linux segundo suas necessidades e use esse VHD para criar rapidamente máquinas virtuais (VMs) do Azure.

## Comandos rápidos
Verifique se você tem [a CLI do Azure](../xplat-cli-install.md) conectada e está usando o modo do Resource Manager (`azure config mode arm`).

Em primeiro lugar, crie um grupo de recursos:

```bash
azure group create TestRG --location "WestUS"
```

Crie uma conta de armazenamento para manter seus discos virtuais:

```bash
azure storage account create testuploadedstorage --resource-group TestRG \
	--location "WestUS" --kind Storage --sku-name PLRS
```

Liste as chaves de acesso da conta de armazenamento que você acabou de criar e anote `key1`:

```bash
azure storage account keys list testuploadedstorage --resource-group TestRG
```

Crie um contêiner dentro de sua conta de armazenamento usando a chave de armazenamento que você acabou de obter:

```bash
azure storage container create --account-name testuploadedstorage \
	--account-key <key1> --container vm-images
```

Por fim, carregue o VHD no contêiner que você acabou de criar:

```bash
azure storage blob upload --blobtype page --account-name testuploadedstorage \
	--account-key <key1> --container vm-images /path/to/disk/yourdisk.vhd
```

Agora, você pode criar uma VM com base no disco virtual carregado [usando um modelo do Resource Manager](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) ou por meio da CLI, especificando o URI do disco, da seguinte forma:

```bash
azure vm create TestVM -l "WestUS" --resource-group TestRG \
	-Q https://testuploadedstorage.blob.core.windows.net/vm-images/yourdisk.vhd
```

Observe que a conta de armazenamento de destino deve ser a mesma em que você carregou o disco virtual. Você também precisará especificar todos os parâmetros adicionais necessários ou responder a prompts deles pelo comando `azure vm create`, como rede virtual, endereço IP público, nome de usuário, chaves SSH, etc. Leia mais sobre os [parâmetros do Resource Manager na CLI disponíveis](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).


## Etapas detalhadas
Há uma série de etapas que abrangem a preparação de sua imagem personalizada do Linux e seu carregamento no Azure. O restante deste artigo fornece informações mais detalhadas sobre cada uma das etapas indicadas no conjunto anterior de comandos rápidos.


## Requisitos
Para concluir as etapas acima, você precisará de:

- **Sistema operacional Linux instalado em um arquivo .vhd**: instale uma [distribuição Linux endossada pelo Azure](virtual-machines-linux-endorsed-distros.md) (ou consulte as [informações para as distribuições não endossadas](virtual-machines-linux-create-upload-generic.md)) em um disco virtual no formato VHD. Existem várias ferramentas para criar uma VM e o VHD:
	- Instalar e configurar o [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) ou o [KVM](http://www.linux-kvm.org/page/RunningKVM), tomando o cuidado de usar o VHD como seu formato de imagem. Você pode [converter uma imagem](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) usando `qemu-img convert` se necessário.
	- Também pode usar o Hyper-V [no Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) ou [no Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [AZURE.NOTE] Não há suporte para o formato VHDX mais recente no Azure. Ao criar uma VM, especifique VHD como o formato. Se necessário, será possível converter discos VHDX para VHD usando o cmdlet [`qemu-img convert`](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) ou [`Convert-VHD`](https://technet.microsoft.com/library/hh848454.aspx) do PowerShell. Além disso, o Azure não dá suporte ao carregamento de VHDs dinâmicos, por isso você precisa converter esses discos para VHDs estáticos antes de carregar. Você pode usar ferramentas como o [Azure VHD Utilities for GO](https://github.com/Microsoft/azure-vhd-utils-for-go) para converter os discos dinâmicos durante o processo de carregamento no Azure.

- As VMs criadas com base em sua imagem personalizada devem residir na mesma conta de armazenamento que a imagem
	- Criar uma conta de armazenamento e um contêiner para manter a imagem personalizada e as VMs criadas
	- Depois de criar todas as VMs, você poderá excluir a imagem com segurança


<a id="prepimage"> </a>
## Preparar a imagem a ser carregada

O Azure dá suporte a várias distribuições do Linux (confira [Distribuições Endossadas](virtual-machines-linux-endorsed-distros.md)). Os seguintes artigos explicam como preparar as diversas distribuições Linux com suporte no Azure:

- **[Distribuições com base em CentOS](virtual-machines-linux-create-upload-centos.md)**
- **[Debian Linux](virtual-machines-linux-debian-create-upload-vhd.md)**
- **[Oracle Linux](virtual-machines-linux-oracle-create-upload-vhd.md)**
- **[Red Hat Enterprise Linux](virtual-machines-linux-redhat-create-upload-vhd.md)**
- **[SLES e openSUSE](virtual-machines-linux-suse-create-upload-vhd.md)**
- **[Ubuntu](virtual-machines-linux-create-upload-ubuntu.md)**
- **[Outros — Distribuições não endossadas](virtual-machines-linux-create-upload-generic.md)**

Veja também as **[Observações de instalação do Linux](virtual-machines-linux-create-upload-generic.md#general-linux-installation-notes)** para obter mais dicas gerais sobre como preparar as imagens do Linux para o Azure.

> [AZURE.NOTE] O [SLA da plataforma Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/) se aplica às VMs que executam o Linux somente quando uma das distribuições endossadas é usada com os detalhes da configuração, conforme especificado na seção “Versões com suporte” em [Linux em distribuições endossadas pelo Azure](virtual-machines-linux-endorsed-distros.md).


## Criar um grupos de recursos
Os grupos de recursos reúnem logicamente todos os recursos do Azure para dar suporte às suas máquinas virtuais, como a rede virtual e o armazenamento. Leia mais sobre os [grupos de recursos do Azure aqui](../resource-group-overview.md). Antes de carregar sua imagem de disco personalizada e criar VMs, primeiro você precisa criar um grupo de recursos:

```bash
azure group create TestRG --location "WestUS"
```

## Criar uma conta de armazenamento
As VMs são armazenadas como blobs de páginas em uma conta de armazenamento. Leia mais sobre o [armazenamento de blobs do Azure aqui](../storage/storage-introduction.md#blob-storage). Você precisa criar uma conta de armazenamento para suas VMs e imagens de disco personalizadas. Todas as VMs criadas com base na imagem de disco personalizada precisam estar na mesma conta de armazenamento que a imagem.

Crie uma conta de armazenamento no grupo de recursos que você acabou de criar:

```bash
azure storage account create testuploadedstorage --resource-group TestRG \
	--location "WestUS" --kind Storage --sku-name PLRS
```

## Listar chaves da conta de armazenamento
O Azure gera duas chaves de acesso de 512 bits para cada conta de armazenamento. Essas chaves de acesso são usadas durante a autenticação na conta de armazenamento, por exemplo, para executar operações de gravação. Leia mais sobre [como gerenciar o acesso ao armazenamento aqui](../storage/storage-create-storage-account.md#manage-your-storage-account). É possível exibir as chaves de acesso com o comando `azure storage account keys list`.

Veja as chaves de acesso da conta de armazenamento que você acabou de criar:

```bash
azure storage account keys list testuploadedstorage --resource-group TestRG
```

A saída será semelhante a:

```
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK

```
Anote `key1`, pois você usará isso para interagir com sua conta de armazenamento nas próximas etapas.

## Criar um contêiner de armazenamento
Da mesma forma que você cria diferentes diretórios para organizar logicamente seu sistema de arquivos local, crie contêineres em uma conta de armazenamento para organizar discos virtuais e imagens de disco. Uma conta de armazenamento pode conter qualquer quantidade de contêineres.

Crie um novo contêiner, especificando a chave de acesso obtida na etapa anterior:

```bash
azure storage container create --account-name testuploadedstorage \
	--account-key <key1> --container vm-images
```

## Carregar o VHD
Agora, você pode efetivamente carregar sua imagem de disco personalizada. Assim como ocorre com todos os discos virtuais usados pelas VMs, você carregará e armazenará sua imagem de disco personalizada como um blob de páginas.

Você precisará especificar sua chave de acesso, o contêiner que você criou na etapa anterior e o caminho para a imagem de disco personalizada no computador local:

```bash
azure storage blob upload --blobtype page --account-name testuploadedstorage \
	--account-key <key1> --container vm-images /path/to/disk/yourdisk.vhd
```

## Criar uma VM com base em uma imagem personalizada
Quando você cria VMs com base em sua imagem de disco personalizada, é necessário especificar o URI da imagem de disco e verificar se a conta de armazenamento de destino corresponde à localização em que a imagem de disco personalizada está armazenada. Você pode criar sua VM usando a CLI do Azure ou o modelo JSON do Resource Manager.


### Criar uma VM usando a CLI do Azure
Especifique o parâmetro `--image-urn` (ou apenas `-Q`) com o comando `azure vm create` para apontar para a imagem de disco personalizada. Verifique se `--storage-account-name` (ou `-o`) corresponde à conta de armazenamento em que a imagem de disco personalizada está armazenada. Você não precisa usar o mesmo contêiner usado para a imagem de disco personalizada para armazenar suas VMs. Apenas lembre-se de criar contêineres adicionais da mesma forma como mostrado nas etapas anteriores antes de carregar as imagens de disco personalizadas.

Crie uma VM com base em sua imagem de disco personalizada:

```bash
azure vm create TestVM -l "WestUS" --resource-group TestRG \
	-Q https://testuploadedstorage.blob.core.windows.net/vm-images/yourdisk.vhd
	-o testuploadedstorage
```

Observe que você ainda precisará especificar todos os parâmetros adicionais necessários ou responder a prompts deles pelo comando `azure vm create`, como rede virtual, endereço IP público, nome de usuário, chaves SSH, etc. Leia mais sobre os [parâmetros do Resource Manager na CLI disponíveis](azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### Criar uma VM usando um modelo JSON
Os modelos do Azure Resource Manager são arquivos JSON (JavaScript Object Notation) que definem o ambiente que você deseja criar. Os modelos são divididos em diferentes provedores de recursos, como computação ou rede. Você pode usar os modelos existentes ou escrever seus próprios. Saiba mais sobre como [usar o Resource Manager e os modelos](../resource-group-overview.md).

No provedor `Microsoft.Compute/virtualMachines` do modelo, haverá um nó `storageProfile` que contém os detalhes de configuração da VM. Os dois parâmetros principais a serem editados são os URIs `image` e `vhd` que apontam para a imagem de disco personalizada e o disco virtual da nova VM. Veja abaixo um exemplo do JSON para o uso de uma imagem de disco personalizada:

```bash
"storageProfile": {
          "osDisk": {
            "name": "TestVM",
            "osType": "Linux",
            "caching": "ReadWrite",
			"createOption": "FromImage",
            "image": {
              "uri": "https://testuploadedstorage.blob.core.windows.net/vm-images/yourdisk.vhd"
            },
            "vhd": {
              "uri": "https://testuploadedstorage.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Você pode usar [este modelo existente para criar uma VM de uma imagem personalizada](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) ou ler sobre como [criar seus próprios modelos do Azure Resource Manager](../resource-group-authoring-templates.md).

Depois de configurar um modelo, crie suas VMs usando o comando `azure group deployment create`. Especifique o URI do modelo JSON com o parâmetro `--template-uri`:

```bash
azure group deployment create --resource-group TestTemplateRG
	--template-uri https://uri.to.template/yourtemplate.json
```

Se você tiver um arquivo JSON armazenado localmente no computador, será possível usar o parâmetro `--template-file`:

```bash
azure group deployment create --resource-group TestTemplateRG
	--template-file /path/to/yourtemplate.json
```


## Próximas etapas
Depois de preparar e carregar seu disco virtual personalizado, leia mais sobre como [usar o Resource Manager e os modelos](../resource-group-overview.md). É recomendável [adicionar um disco de dados](virtual-machines-linux-add-disk.md) às novas VMs. Se você tiver aplicativos em execução nas VMs que precisa acessar, não se esqueça de [abrir as portas e os pontos de extremidade](virtual-machines-linux-nsg-quickstart.md).

<!---HONumber=AcomDC_0810_2016-->