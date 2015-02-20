<properties 
   pageTitle="How to use Azure Redis Cache with Java" 
   description="Get started with Azure Redis Cache using Java" 
   services="cache" 
   documentationCenter="" 
   authors="MikeWasson" 
   manager="wpickett" 
   editor=""/>

<tags
   ms.service="cache"
   ms.devlang="nodejs"
   ms.topic="article"
   ms.tgt_pltfrm="cache-redis"
   ms.workload="required" 
   ms.date="02/20/2014"
   ms.author="mwasson"/>

# How to use Azure Redis Cache with Node.js 

Azure Redis Cache gives you access to a secure, dedicated Redis cache, managed by Microsoft. Your cache is accessible from any application within Microsoft Azure.

This topic shows how to get started with Azure Redis Cache using Node.js.


## Prerequisites

Install [node_redis](https://github.com/mranney/node_redis):

    npm install redis


## Create a Redis cache on Azure

In the [Azure Management Portal Preview](http://go.microsoft.com/fwlink/?LinkId=398536), click **New** and select **Redis Cache**. 

  ![][1]

Enter a DNS hostname. It will have the form `<name>.redis.cache.windows.net`. Click **Create**.

  ![][2]


Once the cache is created, click on it in the portal to view the cache settings. Click the link under **Keys** and copy the primary key. You will need this to authenticate requests.

  ![][4]


## Enable the non-SSL endpoint


Click the link under **Ports**, and click **No** for "Allow access only via SSL". This will enable the non-SSL port for the cache. The node_redis client currently does not support SSL.

  ![][3]


## Add something to the cache and retrieve it

	var redis = require("redis");
	
    // Put in your cache name and access key.
	var client = redis.createClient(6379,'<name>.redis.cache.windows.net', {auth_pass: '<key>' });
	
	client.set("foo", "bar", function(err, reply) {
	    console.log(reply);
	});
	
	client.get("foo",  function(err, reply) {
	    console.log(reply);
	});
    

Output:

	OK
	bar


## Next steps

- [Enable cache diagnostics](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) so you can [monitor](https://msdn.microsoft.com/library/azure/dn763945.aspx) the health of your cache. 
- Read the official [Redis documentation](http://redis.io/documentation).


<!--Image references-->
[1]: ./media/cache-java-get-started/cache01.png
[2]: ./media/cache-java-get-started/cache02.png
[3]: ./media/cache-java-get-started/cache03.png
[4]: ./media/cache-java-get-started/cache04.png

