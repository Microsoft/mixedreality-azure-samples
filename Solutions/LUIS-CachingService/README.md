# LUIS Caching Service

**LUIS Caching Service** is a reusable sample that showcases how to cache results from the [Language Understanding Intelligent Service](https://www.luis.ai/home) (LUIS) in [Microsoft Cognitive Services](https://azure.microsoft.com/services/cognitive-services/). The goal is to help provide partial support for LUIS in voice-based applications running on mobile and [Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality/mixed_reality) devices in poor connectivity areas.

As users speak into the app, their voice commands are sent to LUIS to extract the user's intent and entities out of the utterances. Thesae results are then cached and timestamped locally in a SQLite database, ready to be queried when the same utterance is used. An added benefit of this solution is that it reduces roundtrips to the LUIS APIs in the cloud, accelerating the retrieval of results and reducing API costs since many users often use the same voice commands in apps used on a daily basis.

## Project History

This project was originally developed jointly by Microsoft and [Kognitiv Spark](http://kognitivspark.com/) in February 2018 as part of a 4-day **Mixed Reality + AI** hackfest at the Microsoft Campus in Redmond, WA. Kognitiv Spark's RemoteSpark platform is a holographic worker support solution that uses the power of [Mixed Reality](https://developer.microsoft.com/en-us/windows/mixed-reality/mixed_reality) for workplace remote support. Since these remote workers often work in areas with poor or no connectivity, their main device - [Microsoft HoloLens](https://www.microsoft.com/hololens) - is not guaranteed to be connected to the cloud. Remote Spark makes extensive use of voice commands in the HoloLens app, and Kognitiv Spark's team wanted to expand support to full Natural Language Processing, thanks to LUIS. Since LUIS requires constant connectivity to Azure Cognitive Services, this project was born to provide partial LUIS support even when disconnected.

The original contributors during the hackfest were:

* [David Murphy](https://github.com/davejmurphy), Mixed Reality Developer, Kognitiv Spark
* [Ryan Groom](https://twitter.com/ryangroom), CTO, Kognitiv Spark
* [David Coppet](https://twitter.com/davidcoppet), Azure App Consult Program Lead, Microsoft EMEA
* [Nick Landry](https://github.com/ActiveNick), Mixed Reality Software Engineer, Microsoft Commercial Software Engineering

## Solution Architecture

LUIS Caching Service includes the following components:

* **LUIS Cache Library**: [Universal Windows Platform](https://docs.microsoft.com/windows/uwp/) (UWP) DLL that handles all client-side operations, including making calls to the LUIS API, caching results in SQLite, querying the cache when a new request is made, gathering analytics, and synchronizing LUIS & analytics data with the LUIS Cache Server in Azure. Requires Windows 10 Anniversary Update or higher (1607, build 14393). 
* **LUIS Cache Server**: Web Service application built with ASP.NET Web API & C# and hosted in Azure App Service. This API app receives cache & analytics data from the LUIS Cache Library and provides sync services between SQLite on the device side and Azure SQL Database on the cloud side.
* **LUIS Cache Client**: Test client application for UWP built with XAML & C# used to test & demonstrate the features of the LUIS Caching Service. 

![Solution Architecture](LuisCacheServiceDiagram.jpg)


## Getting Started: How to Deploy the Sample Solution

1. You will need an Azure subscription to deploy and use this solution. If you do not have one, you can [get started with a free trial here](https://azure.microsoft.com/free/).
2. Check to ensure that the build is passing 
    
    ![VSTS Build](https://azureappconsult.visualstudio.com/_apis/public/build/definitions/1d060d9e-a26e-46df-b635-ad9e3c64d8dc/7/badge)
3. (Optional) Fork this repository to your GitHub account. You can also deploy from this page on the Microsoft account but you won't be able to commit changes back. 
4. Click on the **Deploy to Azure** button below. You will be redirected to the Azure portal.

    [![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fmixedreality-azure-samples%2Ffeature%2FLUIScache%2FSolutions%2FLUIS-CachingService%2Fazuredeploy.json)
5. Once logged in, you will be presented with a custom deployment template form. This form is used to capture all the settings required to configure the required cloud components in Azure.
* Select your **Azure subscription** if you have access to more than one.
* It is recommended to create a new **resource group** for this solution. This ensures that all associated cloud services are grouped together without being cluttered with other unrelated services you may already have.
* Select the **location** (i.e. region) where the services will be located. You should pick the region that is closest to you to reduce latency. 
* Change the **App name** to the one you want to use. This is only a prefix used to name all the related Azure services consistently and a random suffix will be added to this name to insure uniqueness in Azure. You can keep the default if you prefer.
* An Azure SQL Database will be created for you. Enter your **SQL Server admin credentials** in the required fields.
*  Update the **repo url** and **branch** settings to match your own fork (as applicable).
6. Once you're ready to deploy, select the checkbox to agree to the terms and click the **Purchase** button. 
7. Open the cloud components solution **LuisCacheCloud.sln** and publish the project **LuisCacheServer** to the Azure Mobile App that was created for you:
* Right-click the **LuisCacheServer** project and select **Publish**.
* Select **App Service** as the publish target.
* Pick the **Select Existing** option and click the **Publish** button.
* In the App Service publishing dialog that appears, select your subscription and expand the resource group you created during the ARM template deployment.
* Select the App Service under that group. There should be only one, named using the prefix you selected with a random suffix appended. 
* Select the **OK** button to start deployment.
8. Select the Cognitive Service that was created by the ARM template deployment process. In the **Quick Start** section, open the [Language Understanding Portal](https://www.luis.ai/). Once authenticated, open the LUIS model you want to use with the caching service, or [create a new model](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/). Once your model is ready, select the option to [Publish](https://docs.microsoft.com/en-us/azure/cognitive-services/luis/publishapp) your LUIS model. Under **Resources and Keys**, add a new key and select the tenant, subscription name and key settings that match the new Cognitive Service that was just created for you. Once the new key is added, **Publish** your model. Make note of the LUIS Application ID and secret subscription key since you will need them below.
9. Open the client components solution **LuisCacheClient.sln** and edit the file **Mainpage.xaml.cs** in the UWP client project **LuisCacheClient**:
* In the **MainPage()** constructor, populate the three variables with your LUIS Application ID, secret subscription key, and App Service web uri. Note that while the LUIS service was created for you by the ARM template, you'll have to build your own LUIS model [in the LUIS portal](https://www.luis.ai) and publish it to Azure.
10. Run the LUISCacheClient UWP app locally on your computer. Type an utterance that matches your LUIS model and click the **Submit Utterance** button. The app will display the macthing intent and entities as returned by LUIS. The second part of the result indicates if the result came from the LUIS service (*false*) or from the cached data (*true*). Your first entry will always be *false* since the cache starts empty. Resubmit the same utterance and the caching result should display *true*.
11. Congratulations! Your LUIS Caching Service is now up and running, and ready to be integrated in your UWP and Windows Mixed Reality projects.

If you have questions or if you discover issues with this solution, please file an issue here on GitHub. 