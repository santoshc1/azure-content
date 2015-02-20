<properties 
	pageTitle="Operational Insights Feature Guide" 
	description="Operational Insights is an analysis service that enables IT administrators to gain deep insight across on-premises and cloud environments. It enables you to interact with real-time and historical machine data to rapidly develop custom insights, and provides Microsoft and community-developed patterns for analyzing data." 
	services="operational-insights" 
	documentationCenter="" 
	authors="bandersmsft" 
	manager="jwhit" 
	editor=""/>

<tags 
	ms.service="operational-insights" 
	ms.workload="appservices" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="11/06/2014" 
	ms.author="banders"/>

<h1>Operational Insights Feature Guide</h1>

This guide helps you understand what problems Operational Insights can help you solve, what the Operational Insights environment consists of, and how you can create an Operational Insights account and sign in to the service.

<h2 id="whatisaad">What is Operational Insights?</h2>

Operational Insights is an analysis service that enables IT administrators to gain deep insight across on-premises and cloud environments. It enables you to interact with real-time and historical machine data to rapidly develop custom insights, and provides Microsoft and community-developed patterns for analyzing data.

With Operational Insights, you can transform machine data into operational intelligence. Specifically, you can:

- Send machine data to the system with or withot using an agent or in conjunction with Sysem Center Operations Manager.
- Manage the capacity of your private cloud infrastructure
- Check update status of your servers 
- Understand relationships between data in server log files
- Protect servers with antimalware
- Manage security risks with security assurance
- Track configuration changes across your servers
- Optimize SQL Server and Active Directory
- Search Event and IIS logs

<h2 id="">Operational Insights environment</h2>

The Operational Insights environment is made up of:

- Microsoft Azure-hosted workspaces which are containers for Azure accounts
- the Operational Insights web service, which is hosted in the cloud
- either separate agents that connect directly to the web service
- or, an attached service to System Center Operations Manager, but is not required




If you used the previous version of Operational Insights called System Center Advisor, you might have Advisor software installed in your local environment. However, Advisor software is not supported with Operational Insights.

Using Operational Insights software as an Operations Manager service consists of one or more management groups and at least one agent per management group. The Operations Manager agents collect data from your servers and analyze it by using intelligence packs (similar to a management pack in System Center Operations Manager). The data is regularly sent from Operations Manager to the Operational Insights web service (if needed, passing through a proxy server), with nothing being stored in any of the Operations Manager databases, so there is no additional load placed on them.

Similarly, agents installed on individual computers can connect directly to the web service to send collected data for processing.

The data in each intelligence pack is analyzed, indexed, and presented in the Operational Insights portal. You can view any alerts and associated remediation guidance, configuration assessments, infrastructure capacity issues, system update status, antimalware warnings, and log data. You can also perform detailed ad-hoc searches and explorations.

<h2 id="">Create an Operational Insights account and sign in</h2>

Use the following information set up an account to use with Microsoft Azure Operational Insights and sign in to Operational Insights.



<h3>Get an Organizational or a Microsoft account</h3>

If you want to create an Organizational account, sign up at <a href="http://go.microsoft.com/fwlink/?LinkId=396866" target="_blank">Microsoft Account For Organizations</a>. Then, you need to grant access to your Organizational account for Operational Insights in <a href="http://aka.ms/Mr1dtz" target="_blank">Windows Azure Active Directory</a>.


If you want to get a Microsoft Account (previously called a “Windows Live ID”), sign up at <a href="http://go.microsoft.com/fwlink/?LinkId=396868" target="_blank">Sign Up – Microsoft Account</a>. Access is automatically granted to Operational Insights with a Microsoft account.


<h3>Sign in to Operational Insights</h3>

1. Go to <a href="preview.opinsights.azure.com" target="_blank">Microsoft Azure Operational Insights</a>  and choose Sign in at the top of the page.
2. Select **Microsoft account** or **Organizational account** and then **sign in** with your credentials.
3. Create an Operational Insights account to associate with your sign-in information.
