<properties 
	pageTitle="Export telemetry from Application Insights" 
	description="Export diagnostic and usage data continuously to storage in Microsoft Azure, and download it from there." 
	services="application-insights" 
	authors="alancameronwills" 
	manager="kamrani"/>

<tags 
	ms.service="application-insights" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="2015-02-04" 
	ms.author="awills"/>
 
# Export telemetry from Application Insights

The events you see in the Application Insights portal can be exported to storage in Microsoft Azure in JSON format. From there you can download and write whatever code you need to process it.  


## <a name="setup"></a> Set up continuous export

On your application's Overview blade in the Application Insights portal, open Continuous Export: 

![Scroll down and click Continuous Export](./media/app-insights-export-telemetry/01-export.png)

Add an export, and choose an [Azure storage account](http://azure.microsoft.com/documentation/articles/storage-introduction/) where you want to put the data:

![Click Add, Export Destination, Storage account, and then either create a new store or choose an existing store](./media/app-insights-export-telemetry/02-add.png)

Choose the event types you'd like to export:

![Click Choose event types](./media/app-insights-export-telemetry/03-types.png)


Once you’ve created your export, it starts going. (You only get data that arrives after you create the export.)


If you want to change the event types later, just edit the export:

![Click Choose event types](./media/app-insights-export-telemetry/05-edit.png)


To stop the stream, delete the export. Doing so doesn’t delete your data.

## <a name="analyze"></a> What events do you get?

The exported data is the raw telemetry we receive from your application, except: 

* Web test results aren’t currently included. 
* We add location data which we calculate from the client IP address.  

Calculated metrics are not included. For example, we don’t export average CPU utilisation, but we do export the raw telemetry from which the average is computed.

## <a name="get"></a> How do you get them?

When you open your blob store with a tool such as [Server Explorer](http://msdn.microsoft.com/library/azure/ff683677.aspx), you’ll see a container with a set of blob files. The URI of each file is application-id/telemetry-type/date/time. 

![Inspect the blob store with a suitable tool](./media/app-insights-export-telemetry/04-data.png)

The date and time are UTC and are when the telemetry was deposited in the store – not the time it was generated. So if you write code to download the data, it can move linearly through the data.

To download this data programmatically, use the [blob store REST API](http://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-blobs/#configure-access) or the [Azure PowerShell cmdlets](http://msdn.microsoft.com/library/azure/dn806401.aspx).

Or consider [DataFactory](http://azure.microsoft.com/services/data-factory/), in which you can set up pipelines to manage data at scale.

We start writing a new blob in each hour (if events are received). So you should always process up to the previous hour, but wait for the current hour to complete.


## <a name="format"></a> What does the data look like?

* Each blob is a text file that contains multiple '\n'-separated rows.
* Each row is an unformatted JSON document. If you want to sit and stare at it, try a viewer such as Notepad++ with the JSON plug-in:

![View the telemetry with a suitable tool](./media/app-insights-export-telemetry/06-json.png)


## How to process it?

On a small scale, you can write some code to pull apart your data, read it into a spreadsheet, and so on. For example:

    private IEnumerable<T> DeserializeMany<T>(string folderName)
    {
      var files = Directory.EnumerateFiles(folderName, "*.blob", SearchOption.AllDirectories);
      foreach (var file in files)
      {
         using (var fileReader = File.OpenText(file))
         {
            string fileContent = fileReader.ReadToEnd();
            IEnumerable<string> entities = fileContent.Split('\n').Where(s => !string.IsNullOrWhiteSpace(s));
            foreach (var entity in entities)
            {
                yield return JsonConvert.DeserializeObject<T>(entity);
            }
         }
      }
    }


On larger scales, consider [HDInsight](http://azure.microsoft.com/services/hdinsight/) - Hadoop clusters in the cloud. HDInsight provides a variety of technologies for managing and analyzing big data.

## <a name="delete"></a>Delete your old data
Please note that you are responsible for managing your storage capacity and deleting the old data if necessary. 

## If you regenerate your storage key...

If you change the key to your storage, continuous export will stop working. You'll see a notification in your Azure account. 

Open the Continuous Export blade and edit your export. Edit the Export Destination, but just leave the same storage selected. Click OK to confirm.

![](./media/app-insights-export-telemetry/07-resetstore.png)

The continuous export will restart.


## Q & A

* *But all I want is a one-time download of a chart.*  
 
    We’re working on that one separately. 

* *I set up an export, but there's no data in my store.*

    Did Application Insights receive any telemetry from your app since you set up the export? You'll only receive new data.

* *I tried to set up an export, but was denied access*

    If the account is owned by your organization, are you a member of the contributors or owners groups?

* *Can I export straight to my own on-premises store?* 

    No, sorry. Our export engine needs to rely on a big open throat to push the data into.  

* *Is there any limit to the amount of data you put in my store?* 

    No. We’ll keep pushing data in until you delete the export. We’ll stop if we hit the outer limits for blob storage, but that’s pretty huge. It’s up to you to control how much storage you use.  

* *I regenerated the key to my storage, and now the export doesn't work.*

    Edit the export and open the export destination blade. Leave the same storage selected as before, and click OK to confirm. Export will restart.

* *Can I pause the export?*

    That's on our backlog.


[AZURE.INCLUDE [app-insights-learn-more](../includes/app-insights-learn-more.md)]




