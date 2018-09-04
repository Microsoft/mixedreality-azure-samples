# MixedReality Azure Samples

**MixedReality Azure Samples** is a collection of samples and reference implementations for using Azure services in simulations. Though "Mixed Reality" is mentioned throughout the project, the vast majority of resources here are not exclusive to Windows Mixed Reality devices. Each sample or reference includes a **System Requirements** section that clearly explains where the code can run.


## Organization

This project is organized into 3 key areas:

### Standalone Samples
Concise projects that demonstrate a capability quickly and don't have external dependencies. These samples can be downloaded independently and tested quickly with little or no server setup.

Samples:
- [Custom Vision with Windows ML in HoloLens](https://github.com/meulta/mixedreality-azure-samples/tree/master/Standalone-Samples/WindowsML-CustomVision-Hololens)
- [Unity with Custom Vision Treasure Hunt Demo for Hololens, Android, and iOS](https://github.com/Microsoft/mixedreality-azure-samples/tree/master/Standalone-Samples/VisionTreasureHunt)
- [Unity with Cognitive Services Text-to-Speech](https://github.com/Microsoft/mixedreality-azure-samples/tree/master/Standalone-Samples/Unity-Text-to-Speech)
- [Unity with Azure Storage](https://github.com/Microsoft/mixedreality-azure-samples/tree/master/Standalone-Samples/AzureStorageDemoUnity3D)

### Solutions
Also demonstrate a  capability but require additional setup. Solutions, for example, may require deploying an Azure workload like a bot, a function, or a database.

Solutions:
- [LUIS Caching Service](https://github.com/Microsoft/mixedreality-azure-samples/tree/master/Solutions/LUIS-CachingService): Reusable solution that showcases how to cache results from LUIS in Cognitive Services.

### Reference Architectures ###
Are a *set of capabilities* that have all been designed to work together. For example, Speech Recognition may be used on its own but it is also designed to work with Language Understanding. 

**Modules**
- [LUIS for XR](Reference-Architecture/Client/MixedReality-Azure-Unity/Assets/MixedRealityAzure/LUIS): A powerful Natural Language replacement for voice commands.


## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us the rights to use your contribution. For details, visit https://cla.microsoft.com.

When you submit a pull request, a CLA-bot will automatically determine whether you need to provide a CLA and decorate the PR appropriately (e.g., label, comment). Simply follow the instructions provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
