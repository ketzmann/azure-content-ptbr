<properties 
	pageTitle="Baixar ativos de mídia" 
	description="Aprenda a baixar ativos para seu computador. Os exemplos de código são escritos em C# e usam a SDK dos Serviços de Mídia para .NET." 
	services="media-services" 
	documentationCenter="" 
	authors="juliako" 
	manager="erikre" 
	editor=""/>

<tags 
	ms.service="media-services" 
	ms.workload="media" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article"
	ms.date="06/22/2016"
	ms.author="juliako"/>

#Como fornecer um ativo por Download

Este tópico descreve as opções para a entrega de ativos de mídia carregados nos Serviços de Mídia. Você pode entregar conteúdo dos Serviços de Mídia em vários cenários de aplicativos. Você pode baixar ativos de mídia ou acessá-los usando um localizador. Você pode enviar conteúdo de mídia para outro aplicativo ou para outro provedor de conteúdo. Para melhor desempenho e escalabilidade, você também pode fornecer conteúdo usando uma CDN (Rede de Entrega de Conteúdo).

Este exemplo mostra como baixar ativos de mídia dos Serviços de Mídia no computador local. O código consulta os trabalhos associados à conta dos Serviços de Mídia por ID do trabalho e acessa sua coleção **OutputMediaAssets** (que é o conjunto de um ou mais ativos de mídia de saída que resulta da execução de um trabalho). Este exemplo mostra como baixar os ativos de mídia da saída de um trabalho, mas você pode aplicar a mesma abordagem para baixar outros ativos.

	
	// Download the output asset of the specified job to a local folder.
	static IAsset DownloadAssetToLocal( string jobId, string outputFolder)
	{
	    // This method illustrates how to download a single asset. 
	    // However, you can iterate through the OutputAssets
	    // collection, and download all assets if there are many. 
	
	    // Get a reference to the job. 
	    IJob job = GetJob(jobId);
	
	    // Get a reference to the first output asset. If there were multiple 
	    // output media assets you could iterate and handle each one.
	    IAsset outputAsset = job.OutputMediaAssets[0];
	
		// Create a SAS locator to download the asset
	    IAccessPolicy accessPolicy = _context.AccessPolicies.Create("File Download Policy", TimeSpan.FromDays(30), AccessPermissions.Read);
	    ILocator locator = _context.Locators.CreateLocator(LocatorType.Sas, outputAsset, accessPolicy);
	
	    BlobTransferClient blobTransfer = new BlobTransferClient
	    {
	        NumberOfConcurrentTransfers = 20,
	        ParallelTransferThreadCount = 20
	    };
	
	    var downloadTasks = new List<Task>();
	    foreach (IAssetFile outputFile in outputAsset.AssetFiles)
	    {
	        // Use the following event handler to check download progress.
	        outputFile.DownloadProgressChanged += DownloadProgress;
	
	        string localDownloadPath = Path.Combine(outputFolder, outputFile.Name);
	
	        Console.WriteLine("File download path:  " + localDownloadPath);
	
	        downloadTasks.Add(outputFile.DownloadAsync(Path.GetFullPath(localDownloadPath), blobTransfer, locator, CancellationToken.None));
	
	        outputFile.DownloadProgressChanged -= DownloadProgress;
	    }
	
	    Task.WaitAll(downloadTasks.ToArray());
	
	    return outputAsset;
	}
	
	static void DownloadProgress(object sender, DownloadProgressChangedEventArgs e)
	{
	    Console.WriteLine(string.Format("{0} % download progress. ", e.Progress));
	}



##Roteiros de aprendizagem dos Serviços de Mídia

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##Fornecer comentários

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

   
##Consulte também 

[Fornecer conteúdo de streaming](media-services-deliver-streaming-content.md)

<!---HONumber=AcomDC_0629_2016-->