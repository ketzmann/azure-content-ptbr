
Para ver os limites gerais em VMs do Azure, consulte [Limites de assinatura e serviços do Azure, cotas e restrições](../articles/azure-subscription-service-limits.md).

Os tamanhos padrão consistem em várias séries: A, D, DS, F, Fs, G e GS. Entre as considerações sobre algumas dessas dimensões estão:

*   As VMs da série D são projetadas para executar aplicativos que exigem maior capacidade de computação e de desempenho de disco temporário. As VMs da série D fornecem processadores mais rápidos, uma maior taxa de memória por núcleo e uma unidade de estado sólido (SSD) para o disco temporário. Para obter detalhes, confira o anúncio no blog do Azure, [Novos tamanhos de máquina virtual da série D](https://azure.microsoft.com/blog/2014/09/22/new-d-series-virtual-machine-sizes/).

*   A série Dv2, uma continuação da série D original, apresenta uma CPU mais potente. A CPU da série Dv2 é aproximadamente 35% mais rápida do que a CPU da série D. Ela se baseia na última geração do processador Intel Xeon® E5-2673 v3 (Haswell) de 2.4 GHz e, com a Intel Turbo Boost Technology 2.0, pode chegar a até 3.1 GHz. A série Dv2 tem as mesmas configurações de memória e disco que a série D.

* A série F é baseada no processador 2.4 GHz Intel Xeon® E5-2673 v3 (Haswell), que pode obter velocidades de relógio tão altas quanto 3.1 GHz com o Intel Turbo Boost Technology 2.0. Esse é o mesmo desempenho de CPU que o das VMs da série Dv2. A um preço de lista inferior por hora, a série F é o melhor valor de preço/desempenho no portfólio do Azure com base na ACU (Unidade de Computação do Azure) por núcleo.

	A série F também introduz um novo padrão em nomenclatura de tamanho de VM para o Azure. Para esta série e os tamanhos de VM lançados no futuro, o valor numérico após a letra do sobrenome corresponderá ao número de núcleos de CPU. Recursos adicionais, como armazenamento otimizado para premium, serão designados por letras após a contagem de núcleos de CPU numérica. Esse formato de nomeação será usado para futuras tamanhos de VM lançados mas não vai retroativamente alterar os nomes de quaisquer tamanhos VM existentes que foram lançados.


*   As VMs da série G oferecem a maior memória e são executadas em hosts com processadores da família Intel Xeon E5 V3.


*   As VMs das séries DS, DSv2, Fs e GS podem usar o Armazenamento Premium, que fornece armazenamento de alto desempenho e baixa latência para cargas de trabalho com uso intenso de E/S. Essas VMs usam SSDs (unidades de estado sólido) para hospedar os discos da máquina virtual e também oferecem um cache de disco SSD local. O Armazenamento Premium está disponível em determinadas regiões. Para obter detalhes, confira [Armazenamento Premium: armazenamento de alto desempenho para cargas de trabalho das máquinas virtuais do Azure](../articles/storage/storage-premium-storage.md).


*   As VMs da série A podem ser implantadas em uma variedade de tipos de hardware e processadores. O tamanho é limitado, com base no hardware, para oferecer desempenho de processador consistente para a instância em execução, independentemente do hardware em que é implantado. Para determinar o hardware físico no qual esse tamanho é implantado, consulte o hardware virtual de dentro da Máquina Virtual.

*   O tamanho A0 está assinado em excesso no hardware físico. Para este tamanho específico somente, outras implantações de clientes podem afetar o desempenho da carga de trabalho em execução. O desempenho relativo é descrito a seguir como a linha de base esperada, sujeito a uma variação aproximada de 15%.


O tamanho da máquina virtual afeta os preços. O tamanho também afeta a capacidade de processamento, memória e armazenamento da máquina virtual. Os custos de armazenamento são calculados separadamente com base nas páginas usadas na conta de armazenamento. Para obter detalhes, confira [Detalhes de preços de máquinas virtuais](https://azure.microsoft.com/pricing/details/virtual-machines/) e [Preços de armazenamento do Azure](https://azure.microsoft.com/pricing/details/storage/).


As considerações a seguir podem ajudá-lo a escolher um tamanho:


* Os tamanhos A8-A11 também são conhecidos como *instâncias de computação intensiva*. O hardware de datacenter do Azure que executa esses tamanhos é projetado e otimizado para aplicativos de uso intensivo de computação e rede, incluindo aplicativos, modelagem e simulações de cluster HPC (computação de alto desempenho). Para obter informações detalhadas e considerações sobre o uso desses tamanhos, confira [Sobre as instâncias de computação intensiva A8, A9, A10 e A11](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).


*	As séries Dv2, D e G e as equivalentes DS/GS são ideais para aplicativos que exigem CPUs mais rápidas, melhor desempenho de disco local ou que têm maior demanda de memória. Elas oferecem uma combinação poderosa para vários aplicativos de nível empresarial.

* As VMs série F são uma ótima opção para cargas de trabalho que exigem CPUs mais rápidas, mas não precisam de tanta memória ou SSD local por núcleo da CPU. Cargas de trabalho como análise, servidores de jogos, servidores Web e de processamento em lote serão beneficiados com o valor da série F.

*   Alguns dos hosts físicos em data centers do Azure podem não aceitar tamanhos de máquina virtual maiores, como A5 – A11. Como resultado, você poderá ver a mensagem de erro **Falha ao configurar a máquina virtual <nome da máquina>** ou **Falha ao criar a máquina virtual <nome da máquina>** ao redimensionar uma máquina virtual existente para um novo tamanho, criar uma nova máquina virtual em uma rede virtual criada antes de 16 de abril de 2013 ou adicionar uma nova máquina virtual a um serviço de nuvem. Confira [Erro: "Falha ao configurar a máquina virtual"](https://social.msdn.microsoft.com/Forums/9693f56c-fcd3-4d42-850e-5e3b56c7d6be/error-failed-to-configure-virtual-machine-with-a5-a6-or-a7-vm-size?forum=WAVirtualMachinesforWindows) no fórum de suporte de soluções alternativas para cada cenário de implantação.


## Considerações sobre o desempenho

Criamos o conceito da ACU (unidade de computação do Azure) para fornecer uma maneira de comparar o desempenho de computação (CPU) em SKUs do Azure. Isso ajudará você a identificar facilmente qual SKU é tem maior probabilidade de satisfazer suas necessidades de desempenho. A ACU atualmente é padronizada como uma VM pequena (Standard\_A1) sendo 100 e todas as SKUs representam, aproximadamente, o quanto a SKU pode executar um parâmetro de comparação padrão mais rapidamente.

>[AZURE.IMPORTANT] A ACU é apenas uma diretriz. Os resultados para sua carga de trabalho podem variar.

<br>

|Família de SKU |ACU/núcleo |
|---|---|
|[Standard\_A0](#a-series) |50 |
|[Standard\_A1-4](#a-series) |100 |
|[Standard\_A5-7](#a-series) |100 |
|[A8-A11](#a-series) |225*|
|[D1-14](#d-series) |160 |
|[D1-15v2](#dv2-series) |210 - 250*|
|[DS1-14](#ds-series) |160 |
|[DS1-15v2](#dsv2-series) |210-250* |
|[F1-F16](#f-series) | 210-250*|
|[F1s-F16s](#fs-series) | 210-250*|
|[G1-5](#g-series) |180 - 240*|
|[GS1-5](#gs-series) |180 - 240*|


ACUs marcados com um * usam tecnologia Intel® Turbo para aumentar a frequência da CPU e fornecer um aumento de desempenho. A quantidade do aumento pode variar com base no tamanho da VM, na carga de trabalho e em outras cargas de trabalho em execução no mesmo host.

## Tabelas de tamanho

As tabelas a seguir mostram os tamanhos e as capacidades oferecidas.

* A capacidade de armazenamento é representada usando-se 1024^3 bytes como a unidade de medida para GB. Isso às vezes é chamado de gibibyte ou definição de base 2. Ao comparar tamanhos que usam diferentes sistemas de base, lembre-se de que os tamanhos de base 2 podem parecer menores do que os de base 10, mas para qualquer tamanho específico (por exemplo, 1 GB), um sistema de base 2 fornece mais capacidade do que um sistema de base 10, porque 1024^3 é maior que 1000^3.

* A largura de banda de rede máxima é a largura de banda agregada máxima alocada e atribuída por tipo de VM. A largura de banda máxima fornece diretrizes para selecionar o tipo correto de VM, a fim de garantir que uma capacidade adequada de rede está disponível. Durante a movimentação entre Baixa, Moderada, Alta e Muito Alta, a taxa de transferência será aumentada de acordo. O desempenho real da rede dependerá de vários fatores, incluindo cargas de rede e aplicativos, bem como configurações de rede do aplicativo.


## Séria A

|Tamanho |Núcleos de CPU|Memória|NICs (Máx.)|Tamanho máximo do disco|Máx. de discos de dados (1023 GB cada)|Máx. IOPS (500 por disco)| Largura de banda de rede máxima |
|---|---|---|---|---|---|---|---|
|Standard\_A0 |1|768 MB|1| Temporário = 20 GB |1|1 x 500| baixo |
|Standard\_A1 |1|1,75 GB|1|Temporário = 70 GB |2|2x500| moderada |
|Standard\_A2 |2|3,5 GB|1|Temporário = 135 GB |4|4x500| moderada |
|Standard\_A3 |4|7 GB|2|Temporário = 285 GB |8|8 x 500| alto |
|Standard\_A4 |8|14 GB|4|Temporário = 605 GB |16|16 x 500| alto |
|Standard\_A5 |2|14 GB|1|Temporário = 135 GB |4|4X500| moderada |
|Standard\_A6 |4|28 GB|2|Temporário = 285 GB |8|8 x 500| alto |
|Standard\_A7 |8|56 GB|4|Temporário = 605 GB |16|16 x 500| alto |



## Série A – Instâncias de computação intensiva

Para obter informações e considerações sobre o uso desses tamanhos, confira [Sobre as instâncias A8, A9, A10 e A11 com computação intensiva](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md).

|Tamanho |Núcleos de CPU|Memória|NICs (Máx.)|Tamanho máximo do disco|Máx. de discos de dados (1023 GB cada)|Máx. IOPS (500 por disco)| Largura de banda de rede máxima |
|---|---|---|---|---|---|---|---|
|Standard\_A8|8|56 GB|2| Temporário = 382 GB |16|16 x 500| alto |
|Standard\_A9|16|112 GB|4| Temporário = 382 GB |16|16 x 500| muito alta |
|Standard\_A10|8|56 GB|2| Temporário = 382 GB |16|16 x 500| alto |
|Standard\_A11|16|112 GB|4| Temporário = 382 GB |16|16 x 500| muito alta |

## Série D

|Tamanho |Núcleos de CPU|Memória|NICs (Máx.)|Tamanho máximo do disco|Máx. de discos de dados (1023 GB cada)|Máx. IOPS (500 por disco)| Largura de banda de rede máxima |
|---|---|---|---|---|---|---|---|
|Standard\_D1 |1|3,5 GB|1|Temporário (SSD) =50 GB |2|2x500| moderada |
|Standard\_D2 |2|7 GB|2|Temporário (SSD) =100 GB |4|4x500| alto |
|Standard\_D3 |4|14 GB|4|Temporário (SSD) =200 GB |8|8 x 500| alto |
|Standard\_D4 |8|28 GB|8|Temporário (SSD) =400 GB |16|16 x 500| alto |
|Standard\_D11 |2|14 GB|2|Temporário (SSD) =100 GB |4|4x500| alto |
|Standard\_D12 |4|28 GB|4|Temporário (SSD) =200 GB |8|8 x 500| alto |
|Standard\_D13 |8|56 GB|8|Temporário (SSD) =400 GB |16|16 x 500| alto |
|Standard\_D14 |16|112 GB|8|Temporário (SSD) =800 GB |32|32 x 500| muito alta |


## Série Dv2

|Tamanho |Núcleos de CPU|Memória|NICs (Máx.)|Tamanho máximo do disco|Máx. de discos de dados (1023 GB cada)|Máx. IOPS (500 por disco)| Largura de banda de rede máxima |
|---|---|---|---|---|---|---|---|
|Standard\_D1\_v2 |1|3,5 GB|1|Temporário (SSD) =50 GB |2|2x500| moderada |
|Standard\_D2\_v2 |2|7 GB|2|Temporário (SSD) =100 GB |4|4x500| alto |
|Standard\_D3\_v2 |4|14 GB|4|Temporário (SSD) =200 GB |8|8 x 500| alto |
|Standard\_D4\_v2 |8|28 GB|8|Temporário (SSD) =400 GB |16|16 x 500| alto |
|Standard\_D5\_v2 |16|56 GB|8|Temporário (SSD) =800 GB |32|32 x 500| extremamente alta |
|Standard\_D11\_v2 |2|14 GB|2|Temporário (SSD) =100 GB |4|4x500| alto |
|Standard\_D12\_v2 |4|28 GB|4|Temporário (SSD) =200 GB |8|8 x 500| alto |
|Standard\_D13\_v2 |8|56 GB|8|Temporário (SSD) =400 GB |16|16 x 500| alto |
|Standard\_D14\_v2 |16|112 GB|8|Temporário (SSD) =800 GB |32|32 x 500| extremamente alta |
|Standard\_D15\_v2 |20|140 GB|8|Temporário (SSD) = 1 TB |40|40 x 500| extremamente alta |


## Série DS*

|Tamanho |Núcleos de CPU|Memória|NICs (Máx.)|Tamanho máximo do disco|Máx. de discos de dados (1023 GB cada)|Tamanho do cache (GB)|Máx. de IOPS de disco &amp; largura de banda| Largura de banda de rede máxima |
|---|---|---|---|---|---|---|---|---|
|Standard\_DS1 |1|3,5|1|Disco SSD local = 7 GB |2|43| 3\.200 32 MB por segundo | moderada |
|Standard\_DS2 |2|7|2|Disco SSD local = 14 GB |4|86| 6\.400 64 MB por segundo | alto |
|Standard\_DS3 |4|14|4|Disco SSD local = 28 GB |8|172| 12\.800 128 MB por segundo | alto |
|Standard\_DS4 |8|28|8|Disco SSD local = 56 GB |16|344| 25\.600 256 MB por segundo | alto |
|Standard\_DS11 |2|14|2|Disco SSD local = 28 GB |4|72| 6\.400 64 MB por segundo | alto |
|Standard\_DS12 |4|28|4|Disco SSD local = 56 GB |8|144| 12\.800 128 MB por segundo | alto |
|Standard\_DS13 |8|56|8|Disco SSD local = 112 GB |16|288| 25\.600 256 MB por segundo | alto |
|Standard\_DS14 |16|112|8|Disco SSD local = 224 GB |32|576| 51\.200 512 MB por segundo | muito alta |

*O máximo de operações de entrada/saída por segundo (IOPS) e de taxa de transferência (largura de banda) possível com uma VM da série DS é afetado pelo tamanho do disco. Para obter detalhes, confira [Armazenamento Premium: armazenamento de alto desempenho para cargas de trabalho das máquinas virtuais do Azure](../articles/storage/storage-premium-storage.md).


## Série DSv2*

|Tamanho |Núcleos de CPU|Memória|NICs (Máx.)|Tamanho máximo do disco|Máx. de discos de dados (1023 GB cada)|Tamanho do cache (GB)|Máx. de IOPS de disco &amp; largura de banda| Largura de banda de rede máxima |
|---|---|---|---|---|---|---|---|---|
|Standard\_DS1\_v2 |1|3,5|1|Disco SSD local = 7 GB |2|43| 3\.200 48 MB por segundo | moderada |
|Standard\_DS2\_v2 |2|7|2|Disco SSD local = 14 GB |4|86| 6\.400 96 MB por segundo | alto |
|Standard\_DS3\_v2 |4|14|4|Disco SSD local = 28 GB |8|172| 12\.800 192 MB por segundo | alto |
|Standard\_DS4\_v2 |8|28|8|Disco SSD local = 56 GB |16|344| 25\.600 384 MB por segundo | alto |
|Standard\_DS5\_v2 |16|56|8|Disco SSD local = 112 GB |32|688| 51\.200 768 MB por segundo | extremamente alta |
|Standard\_DS11\_v2 |2|14|2|Disco SSD local = 28 GB |4|72| 6\.400 96 MB por segundo | alto |
|Standard\_DS12\_v2 |4|28|4|Disco SSD local = 56 GB |8|144| 12\.800 192 MB por segundo | alto |
|Standard\_DS13\_v2 |8|56|8|Disco SSD local = 112 GB |16|288| 25\.600 384 MB por segundo | alto |
|Standard\_DS14\_v2 |16|112|8|Disco SSD local = 224 GB |32|576| 51\.200 768 MB por segundo | extremamente alta |
|Standard\_DS15\_v2 |20|140 GB|8|Disco SSD local = 280 GB |40| 720|64\.000 960 MB por segundo | extremamente alta |


*O máximo de operações de entrada/saída por segundo (IOPS) e de taxa de transferência (largura de banda) possível com uma VM da série DS é afetado pelo tamanho do disco. Para obter detalhes, confira [Armazenamento Premium: armazenamento de alto desempenho para cargas de trabalho das máquinas virtuais do Azure](../articles/storage/storage-premium-storage.md).


## Série F


| Tamanho | Núcleos de CPU | Memória | NICs (Máx.) | Tamanho do disco | Máx. de discos de dados (1.023 GB cada) | IOPS máx. (500 por disco) | Largura de banda de rede máxima |
|--------------|-----------|--------|------------|-------------------------|--------------------------|--------------------------|-------------|
| Standard\_F1 | 1 | 2 GB | 1 | Temporário (SSD) = 16 GB | 2 | 2x500 | moderada |
| Standard\_F2 | 2 | 4 GB | 2 | Temporário (SSD) = 32 GB | 4 | 4x500 | alto |
| Standard\_F4 | 4 | 8 GB | 4 | Temporário (SSD) = 64 GB | 8 | 8 x 500 | alto |
| Standard\_F8 | 8 | 16 GB | 8 | Temporário (SSD) = 128 GB | 16 | 16 x 500 | alto |
| Standard\_F16 | 16 | 32 GB | 8 | Temporário (SSD) = 256 GB | 32 | 32 x 500 | extremamente alta |



## Série Fs*

| Tamanho | Núcleos de CPU | Memória | NICs (Máx.) | Tamanho do disco | Máx. de discos de dados (1.023 GB cada) | Tamanho do cache (GB) | Máx. de IOPS de disco &amp; largura de banda | Largura de banda de rede máxima |
|---------------|-----------|--------|------------|------------------------|-----------|-----------|----------------------------|------------|
| Standard\_F1s | 1 | 2 | 1 | Disco SSD local = 4 GB | 2 | 12 | 3\.200 48 MB por segundo | moderada |
| Standard\_F2s | 2 | 4 | 2 | Disco SSD local = 8 GB | 4 | 24 | 6\.400 96 MB por segundo | alto |
| Standard\_F4s | 4 | 8 | 4 | Disco SSD local = 16 GB | 8 | 48 | 12\.800 192 MB por segundo | alto |
| Standard\_F8s | 8 | 16 | 8 | Disco SSD local = 32 GB | 16 | 96 | 25\.600 384 MB por segundo | alto |
| Standard\_F16s | 16 | 32 | 8 | Disco SSD local = 64 GB | 32 | 192 | 51\.200 768 MB por segundo | extremamente alta |



*O máximo de operações de entrada/saída por segundo (IOPS) e de taxa de transferência (largura de banda) possível com uma VM da série Fs é afetado pelo tamanho do disco. Para obter detalhes, confira [Armazenamento Premium: armazenamento de alto desempenho para cargas de trabalho das máquinas virtuais do Azure](../articles/storage/storage-premium-storage.md).





## Série G

|Tamanho |Núcleos de CPU|Memória|NICs (Máx.)|Tamanho máximo do disco|Máx. de discos de dados (1023 GB cada)|Máx. IOPS (500 por disco)| Largura de banda de rede máxima |
|---|---|---|---|---|---|---|---|
|Standard\_G1 |2|28 GB|1|Disco SSD local = 384 GB |4|4 x 500| alto |
|Standard\_G2 |4|56 GB|2|Disco SSD local = 768 GB |8|8 x 500| alto |
|Standard\_G3 |8|112 GB|4|Disco SSD local = 1.536 GB |16|16 x 500| muito alta | 
|Standard\_G4 |16|224 GB|8|Disco SSD local = 3.072 GB |32|32 x 500| extremamente alta |
|Standard\_G5 |32|448 GB|8|Disco SSD local = 6.144 GB |64| 64 x 500 | extremamente alta |




## Série GS

|Tamanho |Núcleos de CPU|Memória|NICs (Máx.)|Tamanho máximo do disco|Máx. de discos de dados (1023 GB cada)|Tamanho do cache (GB)|Máx. de IOPS de disco &amp; largura de banda| Largura de banda de rede máxima |
|---|---|---|---|---|---|---|---|---|
|Standard\_GS1|2|28|1|Disco SSD local = 56 GB |4|264| 5\.000 125 MB por segundo | alto |
|Standard\_GS2|4|56|2|Disco SSD local = 112 GB |8|528| 10\.000 250 MB por segundo | alto | 
|Standard\_GS3|8|112|4|Disco SSD local = 224 GB |16|1056| 20\.000 500 MB por segundo | muito alta |
|Standard\_GS4|16|224|8|Disco SSD local = 448 GB |32|2112| 40\.000 1.000 MB por segundo | extremamente alta |
|Standard\_GS5|32|448|8|Disco SSD local = 896 GB |64|4224| 80\.000 2.000 MB por segundo | extremamente alta |

## Série N (visualização)

Os tamanhos NC e NV também são conhecidos como instâncias habilitadas para GPU. Trata-se de máquinas virtuais especializadas que incluem as placas GPU da NVIDIA, otimizadas para diferentes cenários e casos de uso. Os tamanhos NV são otimizados e projetados para cenários de visualização remota, streaming, jogos, codificação e VDI usando estruturas como OpenGL e DirectX. Os tamanhos NC são mais otimizados para aplicativos que fazem uso intensivo de computação e da rede, algoritmos, incluindo aplicativos baseados em CUDA e OpenCL, e simulações.


### Instâncias NV
As instâncias NV têm a tecnologia das GPUs Tesla M60 da NVIDIA e do NVIDIA GRID para aplicativos acelerados de área de trabalho e áreas de trabalho virtuais, em que os clientes poderão visualizar seus dados ou simulações. Os usuários poderão visualizar seus fluxos de trabalho com uso intensivo de gráficos em instâncias NV para obter capacidade gráfica superior, além de executar cargas de trabalho de precisão única, como codificação e renderização. O Tesla M60 oferece 4096 núcleos CUDA em um design de GPU duplo com até 36 fluxos de 1080p H.264.


| Tamanho | Núcleos | GPU | Memória | Disco |
|------|-------|----------------|--------|-------------|
| Standard\_NV6 | 6 | 1 x NVIDIA M60 | 56 GB | 380 GB de SSD |
| Standard\_NV12 | 12 | 2 x NVIDIA M60 | 112 GB | 680 GB de SSD |
| Standard\_NV24 | 24 | 4 x NVIDIA M60 | 224 GB | 1440 GB de SSD | 



### Instâncias de NC

As instâncias de NC têm a tecnologia Tesla K80 da NVIDIA. Agora, os usuários podem analisar dados muito mais rapidamente tirando proveito do CUDA para aplicativos de exploração de energia, simulações de falhas, renderização com ray tracing, aprendizado aprofundado e muito mais. O Tesla K80 oferece 4992 núcleos CUDA com um design de GPU duplo, desempenho de até 2,91 teraflops de precisão dupla e até 8,93 teraflops de precisão simples.


| Tamanho | Núcleos | GPU | Memória | Disco |
|------|-------|----------------|--------|-------------|
| Standard\_NC6 | 6 | 1 x NVIDIA K80 | 56 GB | 380 GB de SSD |
| Standard\_NC12 | 12 | 2 x NVIDIA K80 | 112 GB | 680 GB de SSD |
| Standard\_NC24 | 24 | 4 x NVIDIA K80 | 224 GB | 1440 GB de SSD |

## Observações: Standard A0 - A4 usando a CLI e o PowerShell 


No modelo de implantação clássica, alguns nomes de tamanhos de VM são ligeiramente diferentes na CLI e no PowerShell:

* Standard\_A0 é ExtraSmall
* Standard\_A1 é pequeno
* Standard\_A2 é médio
* Standard\_A3 é grande
* Standard\_A4 é ExtraLarge


## Próximas etapas

- Saiba mais sobre a [assinatura do Azure e limites de serviços, cotas e restrições](../articles/azure-subscription-service-limits.md).
- Saiba mais [sobre as instâncias A8, A9, A10 e A11 com computação intensiva](../articles/virtual-machines/virtual-machines-windows-a8-a9-a10-a11-specs.md) para cargas de trabalho como HPC (Computação de Alto Desempenho).

<!---HONumber=AcomDC_0810_2016-->