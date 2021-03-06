---
title: 'Tutorial: Criar um pipeline com o Modelo do Resource Manager| Microsoft Docs'
description: "Neste tutorial, irá criar um pipeline do Azure Data Factory com uma Atividade de Cópia com o modelo do Azure Resource Manager."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 1274e11a-e004-4df5-af07-850b2de7c15e
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 01/17/2017
ms.author: spelluru
translationtype: Human Translation
ms.sourcegitcommit: 5e6ffbb8f1373f7170f87ad0e345a63cc20f08dd
ms.openlocfilehash: 977fcf74c86e2ab312d99b3f1821cfb7efbe9745
ms.lasthandoff: 03/24/2017


---
# <a name="tutorial-create-a-pipeline-with-copy-activity-using-azure-resource-manager-template"></a>Tutorial: Criar um pipeline com a Atividade de Cópia com o Modelo do Azure Resource Manager
> [!div class="op_single_selector"]
> * [Descrição geral e pré-requisitos](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [Assistente de Cópia](data-factory-copy-data-wizard-tutorial.md)
> * [Portal do Azure](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Modelo do Azure Resource Manager](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [API REST](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [API .NET](data-factory-copy-activity-tutorial-using-dotnet-api.md)
> 
> 

Este tutorial mostra-lhe como criar e monitorizar uma fábrica de dados do Azure com o modelo do Azure Resource Manager. O pipeline na fábrica de dados copia dados do Armazenamento de Blobs do Azure para uma Base de Dados SQL do Azure.

> [!NOTE]
> O pipeline de dados neste tutorial copia dados a partir de um arquivo de dados de origem para um arquivo de dados de destino. Não transforma dados de entrada para produzir dados de saída. Para ver um tutorial sobre como transformar dados através do Azure Data Factory, consulte [Tutorial: Build a pipeline to transform data using Hadoop cluster (Tutorial: Criar um pipeline para transformar dados com o cluster do Hadoop)](data-factory-build-your-first-pipeline.md).
> 
> Pode encadear duas atividades (executar uma atividade após a outra) ao definir o conjunto de dados de saída de uma atividade como o conjunto de dados de entrada da outra atividade. Consulte [Scheduling and execution in Data Factory (Agendamento e execução no Data Factory)](data-factory-scheduling-and-execution.md) para obter informações detalhadas. 

## <a name="prerequisites"></a>Pré-requisitos
* Leia o artigo [Descrição Geral do Tutorial e Pré-requisitos](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) e conclua os passos de **pré-requisitos**.
* Siga as instruções no artigo [How to install and configure Azure PowerShell (Como instalar e configurar o Azure PowerShell)](/powershell/azureps-cmdlets-docs) para instalar a versão mais recente do Azure PowerShell no computador. Neste tutorial, utiliza o PowerShell para implementar entidades do Data Factory. 
* (opcional) Veja [Authoring Azure Resource Manager Templates (Criação de Modelos Azure Resource Manager)](../azure-resource-manager/resource-group-authoring-templates.md) para saber mais sobre os modelos Azure Resource Manager.

## <a name="in-this-tutorial"></a>Neste tutorial
Neste tutorial, cria uma fábrica de dados com as entidades do Data Factory seguintes:

| Entidade | Descrição |
| --- | --- |
| Serviço ligado do Storage do Azure |Liga a sua conta de Armazenamento do Azure à fábrica de dados. O Armazenamento do Azure é o arquivo de dados de origem e a base de dados SQL do Azure é o arquivo de dados de sink para a atividade de cópia no tutorial. Especifica a conta de armazenamento que contém os dados de entrada para a atividade de cópia. |
| Serviço ligado da Base de Dados SQL do Azure |Liga a base de dados SQL do Azure à fábrica de dados. Especifica a base de dados SQL do Azure que contém os dados de saída para a atividade de cópia. |
| Conjunto de dados de entrada de Blobs do Azure |Refere-se ao serviço ligado de Armazenamento do Azure. O serviço ligado refere-se a uma conta de armazenamento do Azure e o conjunto de dados do Blob do Azure especifica um contentor, uma pasta e um nome de ficheiro no armazenamento que contém os dados de entrada. |
| Conjunto de dados de saída SQL do Azure |Refere-se ao serviço ligado SQL do Azure. O serviço ligado SQL do Azure refere-se a um servidor SQL do Azure e o conjunto de dados SQL do Azure especifica o nome da tabela que contém os dados de saída. |
| Pipeline de dados |O pipeline tem uma atividade do tipo Copiar que aceita o conjunto de dados de blobs do Azure como uma entrada e o conjunto de dados SQL do Azure como uma saída. A atividade de cópia copia dados de um blob do Azure para uma tabela na base de dados SQL do Azure. |

Uma fábrica de dados pode ter um ou mais pipelines. Um pipeline pode conter uma atividade ou mais. Existem dois tipos de atividades: [atividades de movimento de dados](data-factory-data-movement-activities.md) e [atividades de transformação de dados](data-factory-data-transformation-activities.md). Neste tutorial, pode criar um pipeline com uma atividade (atividade de cópia).

![Copiar Blobs do Azure para a Base de Dados SQL do Azure](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/CopyBlob2SqlDiagram.png) 

A secção seguinte disponibiliza o modelo do Resource Manager completo para a definição de entidades do Data Factory, para que possa rapidamente dar uma vista de olhos pelo tutorial e testar o modelo. Para compreender como cada entidade do Data Factory está definida, consulte a secção [Data Factory entities in the template (Entidades do Data Factory no modelo)](#data-factory-entities-in-the-template).

## <a name="data-factory-json-template"></a>Modelo JSON do Data Factory
O modelo do Resource Manager de nível superior para definir uma fábrica de dados é: 

```json
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { ...
    },
    "variables": { ...
    },
    "resources": [
        {
            "name": "[parameters('dataFactoryName')]",
            "apiVersion": "[variables('apiVersion')]",
            "type": "Microsoft.DataFactory/datafactories",
            "location": "westus",
            "resources": [
                { ... },
                { ... },
                { ... },
                { ... }
            ]
        }
    ]
}
```
Crie um ficheiro JSON com o nome **ADFCopyTutorialARM.json** na pasta **C:\ADFGetStarted** com o seguinte conteúdo:

```json
{
    "contentVersion": "1.0.0.0",
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "parameters": {
      "storageAccountName": { "type": "string", "metadata": { "description": "Name of the Azure storage account that contains the data to be copied." } },
      "storageAccountKey": { "type": "securestring", "metadata": { "description": "Key for the Azure storage account." } },
      "sourceBlobContainer": { "type": "string", "metadata": { "description": "Name of the blob container in the Azure Storage account." } },
      "sourceBlobName": { "type": "string", "metadata": { "description": "Name of the blob in the container that has the data to be copied to Azure SQL Database table" } },
      "sqlServerName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Server that will hold the output/copied data." } },
      "databaseName": { "type": "string", "metadata": { "description": "Name of the Azure SQL Database in the Azure SQL server." } },
      "sqlServerUserName": { "type": "string", "metadata": { "description": "Name of the user that has access to the Azure SQL server." } },
      "sqlServerPassword": { "type": "securestring", "metadata": { "description": "Password for the user." } },
      "targetSQLTable": { "type": "string", "metadata": { "description": "Table in the Azure SQL Database that will hold the copied data." } 
      } 
    },
    "variables": {
      "dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]",
      "azureSqlLinkedServiceName": "AzureSqlLinkedService",
      "azureStorageLinkedServiceName": "AzureStorageLinkedService",
      "blobInputDatasetName": "BlobInputDataset",
      "sqlOutputDatasetName": "SQLOutputDataset",
      "pipelineName": "Blob2SQLPipeline"
    },
    "resources": [
      {
        "name": "[variables('dataFactoryName')]",
        "apiVersion": "2015-10-01",
        "type": "Microsoft.DataFactory/datafactories",
        "location": "West US",
        "resources": [
          {
            "type": "linkedservices",
            "name": "[variables('azureStorageLinkedServiceName')]",
            "dependsOn": [
              "[variables('dataFactoryName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
              "type": "AzureStorage",
              "description": "Azure Storage linked service",
              "typeProperties": {
                "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
              }
            }
          },
          {
            "type": "linkedservices",
            "name": "[variables('azureSqlLinkedServiceName')]",
            "dependsOn": [
              "[variables('dataFactoryName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
              "type": "AzureSqlDatabase",
              "description": "Azure SQL linked service",
              "typeProperties": {
                "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
              }
            }
          },
          {
            "type": "datasets",
            "name": "[variables('blobInputDatasetName')]",
            "dependsOn": [
              "[variables('dataFactoryName')]",
              "[variables('azureStorageLinkedServiceName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
              "type": "AzureBlob",
              "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
              "structure": [
                {
                  "name": "Column0",
                  "type": "String"
                },
                {
                  "name": "Column1",
                  "type": "String"
                }
              ],
              "typeProperties": {
                "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
                "fileName": "[parameters('sourceBlobName')]",
                "format": {
                  "type": "TextFormat",
                  "columnDelimiter": ","
                }
              },
              "availability": {
                "frequency": "Day",
                "interval": 1
              },
              "external": true
            }
          },
          {
            "type": "datasets",
            "name": "[variables('sqlOutputDatasetName')]",
            "dependsOn": [
              "[variables('dataFactoryName')]",
              "[variables('azureSqlLinkedServiceName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
              "type": "AzureSqlTable",
              "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
              "structure": [
                {
                  "name": "FirstName",
                  "type": "String"
                },
                {
                  "name": "LastName",
                  "type": "String"
                }
              ],
              "typeProperties": {
                "tableName": "[parameters('targetSQLTable')]"
              },
              "availability": {
                "frequency": "Day",
                "interval": 1
              }
            }
          },
          {
            "type": "datapipelines",
            "name": "[variables('pipelineName')]",
            "dependsOn": [
              "[variables('dataFactoryName')]",
              "[variables('azureStorageLinkedServiceName')]",
              "[variables('azureSqlLinkedServiceName')]",
              "[variables('blobInputDatasetName')]",
              "[variables('sqlOutputDatasetName')]"
            ],
            "apiVersion": "2015-10-01",
            "properties": {
              "activities": [
                {
                  "name": "CopyFromAzureBlobToAzureSQL",
                  "description": "Copy data frm Azure blob to Azure SQL",
                  "type": "Copy",
                  "inputs": [
                    {
                      "name": "[variables('blobInputDatasetName')]"
                    }
                  ],
                  "outputs": [
                    {
                      "name": "[variables('sqlOutputDatasetName')]"
                    }
                  ],
                  "typeProperties": {
                    "source": {
                      "type": "BlobSource"
                    },
                    "sink": {
                      "type": "SqlSink",
                      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                    },
                    "translator": {
                      "type": "TabularTranslator",
                      "columnMappings": "Column0:FirstName,Column1:LastName"
                    }
                  },
                  "Policy": {
                    "concurrency": 1,
                    "executionPriorityOrder": "NewestFirst",
                    "retry": 3,
                    "timeout": "01:00:00"
                  }
                }
              ],
              "start": "2016-10-02T00:00:00Z",
              "end": "2016-10-03T00:00:00Z"
            }
          }
        ]
      }
    ]
  }
```

## <a name="parameters-json"></a>Parâmetros JSON
Crie um ficheiro JSON com o nome **ADFCopyTutorialARM Parameters.json** que contém os parâmetros para o modelo do Azure Resource Manager. 

> [!IMPORTANT]
> Especifique o nome e a chave da sua Conta de armazenamento do Azure para os parâmetros **storageAccountName** e **storageAccountKey**.  
> 
> 

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentParameters.json#",
    "contentVersion": "1.0.0.0",
    "parameters": { 
        "storageAccountName": { "value": "<Name of the Azure storage account>"    },
        "storageAccountKey": {
            "value": "<Key for the Azure storage account>"
        },
        "sourceBlobContainer": { "value": "adftutorial" },
        "sourceBlobName": { "value": "emp.txt" },
        "sqlServerName": { "value": "<Name of the Azure SQL server>" },
        "databaseName": { "value": "<Name of the Azure SQL database>" },
        "sqlServerUserName": { "value": "<Name of the user who has access to the Azure SQL database>" },
        "sqlServerPassword": { "value": "<password for the user>" },
        "targetSQLTable": { "value": "emp" }
    }
}
```

> [!IMPORTANT]
> Pode ter ficheiros JSON de parâmetro separado para ambientes de desenvolvimento, teste e produção, que pode utilizar com o mesmo modelo JSON do Data Factory. Ao utilizar um script do Power Shell, pode automatizar a implementação de entidades do Data Factory nestes ambientes.  
> 
> 

## <a name="create-data-factory"></a>Criar fábrica de dados
1. Inicie o **Azure PowerShell** e execute o seguinte comando:
   * Execute o comando seguinte e introduza o nome de utilizador e a palavra-passe que utiliza para iniciar sessão no portal do Azure.
    ```PowerShell
    Login-AzureRmAccount       
    ```  
   * Execute o comando seguinte para ver todas as subscrições para esta conta.
    ```PowerShell
    Get-AzureRmSubscription
    ```   
   * Execute o comando seguinte para selecionar a subscrição com a qual pretende trabalhar. 
    ```PowerShell
    Get-AzureRmSubscription -SubscriptionName <SUBSCRIPTION NAME> | Set-AzureRmContext
    ```    
2. Execute o seguinte comando para implementar as entidades do Data Factory com o modelo do Resource Manager que criou no Passo 1.

    ```PowerShell   
    New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile C:\ADFGetStarted\ADFCopyTutorialARM.json -TemplateParameterFile C:\ADFGetStarted\ADFCopyTutorialARM-Parameters.json
    ```

## <a name="monitor-pipeline"></a>Monitorizar o pipeline

1. Inicie sessão no [Portal do Azure](https://portal.azure.com) com a sua conta do Azure.
2. Clique em **Fábricas de dados** no menu da esquerda (ou) clique em **Mais serviços** e clique em **Fábricas de dados** na categoria **INTELIGÊNCIA + ANÁLISE**.
   
    ![Menu de fábricas de dados](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factories-menu.png)
3. Na página **Fábricas de dados**, procure e encontre a sua fábrica de dados. 
   
    ![Procure a fábrica de dados](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/search-for-data-factory.png)  
4. Clique na sua fábrica de dados do Azure. Pode ver a home page da fábrica de dados.
   
    ![Home page da fábrica de dados](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-home-page.png)  
5. Clique no mosaico **Diagrama** para ver a vista de diagrama da fábrica de dados.
   
    ![Vista de diagrama da fábrica de dados](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/data-factory-diagram-view.png)
6. Na vista de diagrama, faça duplo clique no conjunto de dados **SQLOutputDataset**. Pode ver o estado do setor. Ao concluir a operação de cópia, define o estado como **Pronto**.
   
    ![Setor de saída no estado pronto](media/data-factory-copy-activity-tutorial-using-azure-resource-manager-template/output-slice-ready.png)
7. Quando o setor estiver no estado **Pronto**, certifique-se de que os dados são copiados para a tabela **emp** na base de dados SQL do Azure.

Veja [Monitor datasets and pipeline (Monitorizar os conjuntos de dados e o pipeline)](data-factory-monitor-manage-pipelines.md) para obter instruções sobre como utilizar os painéis do Portal do Azure para monitorizar o pipeline e os conjuntos de dados que criou neste tutorial.

Pode ainda utilizar a Aplicação de Monitorização e Gestão para monitorizar os seus pipelines de dados. Veja [Monitor and manage Azure Data Factory pipelines using Monitoring App (Monitorizar e gerir pipelines do Azure Data Factory com a Aplicação de Monitorização)](data-factory-monitor-manage-app.md) para obter detalhes sobre a utilização da aplicação.

## <a name="data-factory-entities-in-the-template"></a>Entidades do Data Factory no modelo
### <a name="define-data-factory"></a>Definir fábrica de dados
Defina uma fábrica de dados no modelo do Resource Manager, conforme mostrado no exemplo seguinte:  

```json
"resources": [
{
    "name": "[variables('dataFactoryName')]",
    "apiVersion": "2015-10-01",
    "type": "Microsoft.DataFactory/datafactories",
    "location": "West US"
}
```

O dataFactoryName é definido como: 

```json
"dataFactoryName": "[concat('AzureBlobToAzureSQLDatabaseDF', uniqueString(resourceGroup().id))]"
```

É uma cadeia de carateres exclusiva com base no ID do grupo do recursos.  

### <a name="defining-data-factory-entities"></a>Definir entidades do Data Factory
As seguintes entidades do Data Factory são definidas no modelo JSON: 

1. [Serviço ligado do Armazenamento do Azure](#azure-storage-linked-service)
2. [Serviço ligado SQL do Azure](#azure-sql-database-linked-service)
3. [Conjunto de dados de blobs do Azure](#azure-blob-dataset)
4. [Conjunto de dados SQL do Azure](#azure-sql-dataset)
5. [Pipeline de dados com uma atividade de cópia](#data-pipeline)

#### <a name="azure-storage-linked-service"></a>Serviço ligado do Storage do Azure
Especifique o nome e a chave da sua conta de armazenamento do Azure nesta secção. Veja [Azure Storage linked service (Serviço ligado de Armazenamento do Azure)](data-factory-azure-blob-connector.md#azure-storage-linked-service) para obter detalhes sobre as propriedades JSON utilizadas para definir um serviço ligado de Armazenamento do Azure. 

```json
{
    "type": "linkedservices",
    "name": "[variables('azureStorageLinkedServiceName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "type": "AzureStorage",
        "description": "Azure Storage linked service",
        "typeProperties": {
            "connectionString": "[concat('DefaultEndpointsProtocol=https;AccountName=',parameters('storageAccountName'),';AccountKey=',parameters('storageAccountKey'))]"
        }
    }
}
```

A connectionString utiliza os parâmetros storageAccountName e storageAccountKey. Os valores para estes parâmetros foram transmitidos através da utilização de um ficheiro de configuração. A definição também utiliza variáveis: azureStroageLinkedService e dataFactoryName definidas no modelo. 

#### <a name="azure-sql-database-linked-service"></a>Serviço ligado da Base de Dados SQL do Azure
Especifique o nome do servidor SQL do Azure, o nome da base de dados, o nome de utilizador e a palavra-passe do utilizador nesta secção. Consulte o [Azure SQL linked service (Serviço ligado SQL do Azure)](data-factory-azure-sql-connector.md#linked-service-properties) para obter detalhes sobre as propriedades JSON utilizadas para definir um serviço ligado SQL do Azure.  

```json
{
    "type": "linkedservices",
    "name": "[variables('azureSqlLinkedServiceName')]",
    "dependsOn": [
      "[variables('dataFactoryName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
          "type": "AzureSqlDatabase",
          "description": "Azure SQL linked service",
          "typeProperties": {
            "connectionString": "[concat('Server=tcp:',parameters('sqlServerName'),'.database.windows.net,1433;Database=', parameters('databaseName'), ';User ID=',parameters('sqlServerUserName'),';Password=',parameters('sqlServerPassword'),';Trusted_Connection=False;Encrypt=True;Connection Timeout=30')]"
          }
    }
}
```

O connectionString utiliza os parâmetros sqlServerName, databaseName, sqlServerUserName e sqlServerPassword, cujos valores são transmitidos através da utilização de um ficheiro de configuração. A definição também utiliza as seguintes variáveis do modelo: azureSqlLinkedServiceName, dataFactoryName.

#### <a name="azure-blob-dataset"></a>Conjunto de dados de blobs do Azure
Especifique os nomes do contentor de blob, da pasta e do ficheiro que contém os dados de entrada. Veja [Azure Blob dataset properties (Propriedades do conjunto de dados de Blobs do Azure)](data-factory-azure-blob-connector.md#dataset-properties) para obter detalhes sobre as propriedades JSON utilizadas para definir um conjunto de dados de Blobs do Azure. 

```json
{
    "type": "datasets",
    "name": "[variables('blobInputDatasetName')]",
    "dependsOn": [
      "[variables('dataFactoryName')]",
      "[variables('azureStorageLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
        "type": "AzureBlob",
          "linkedServiceName": "[variables('azureStorageLinkedServiceName')]",
        "structure": [
        {
              "name": "Column0",
              "type": "String"
        },
        {
              "name": "Column1",
              "type": "String"
        }
          ],
          "typeProperties": {
            "folderPath": "[concat(parameters('sourceBlobContainer'), '/')]",
            "fileName": "[parameters('sourceBlobName')]",
            "format": {
                  "type": "TextFormat",
                  "columnDelimiter": ","
            }
          },
          "availability": {
            "frequency": "Day",
            "interval": 1
          },
          "external": true
    }
}
```

#### <a name="azure-sql-dataset"></a>Conjunto de dados SQL do Azure
Especifique o nome da tabela na base de dados SQL do Azure que contém os dados copiados a partir do armazenamento de Blobs do Azure. Consulte [Azure SQL dataset properties (Propriedades do conjunto de dados SQL do Azure)](data-factory-azure-sql-connector.md#dataset-properties) para obter detalhes sobre as propriedades JSON utilizadas para definir um conjunto de dados SQL do Azure. 

```json
{
    "type": "datasets",
    "name": "[variables('sqlOutputDatasetName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
          "[variables('azureSqlLinkedServiceName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
          "type": "AzureSqlTable",
          "linkedServiceName": "[variables('azureSqlLinkedServiceName')]",
          "structure": [
        {
              "name": "FirstName",
              "type": "String"
        },
        {
              "name": "LastName",
              "type": "String"
        }
          ],
          "typeProperties": {
            "tableName": "[parameters('targetSQLTable')]"
          },
          "availability": {
            "frequency": "Day",
            "interval": 1
          }
    }
}
```

#### <a name="data-pipeline"></a>Pipeline de dados
Definir um pipeline que copia dados do conjunto de dados de blobs do Azure para o conjunto de dados SQL do Azure. Veja [Pipeline JSON (JSON do Pipeline)](data-factory-create-pipelines.md#pipeline-json) para obter descrições dos elementos JSON utilizados para definir um pipeline neste exemplo. 

```json
{
    "type": "datapipelines",
    "name": "[variables('pipelineName')]",
    "dependsOn": [
        "[variables('dataFactoryName')]",
          "[variables('azureStorageLinkedServiceName')]",
          "[variables('azureSqlLinkedServiceName')]",
          "[variables('blobInputDatasetName')]",
          "[variables('sqlOutputDatasetName')]"
    ],
    "apiVersion": "2015-10-01",
    "properties": {
          "activities": [
        {
              "name": "CopyFromAzureBlobToAzureSQL",
              "description": "Copy data frm Azure blob to Azure SQL",
              "type": "Copy",
              "inputs": [
            {
                  "name": "[variables('blobInputDatasetName')]"
            }
              ],
              "outputs": [
            {
                  "name": "[variables('sqlOutputDatasetName')]"
            }
              ],
              "typeProperties": {
                "source": {
                      "type": "BlobSource"
                },
                "sink": {
                      "type": "SqlSink",
                      "sqlWriterCleanupScript": "$$Text.Format('DELETE FROM {0}', 'emp')"
                },
                "translator": {
                      "type": "TabularTranslator",
                      "columnMappings": "Column0:FirstName,Column1:LastName"
                }
              },
              "Policy": {
                "concurrency": 1,
                "executionPriorityOrder": "NewestFirst",
                "retry": 3,
                "timeout": "01:00:00"
              }
        }
          ],
          "start": "2016-10-02T00:00:00Z",
          "end": "2016-10-03T00:00:00Z"
    }
}
```

## <a name="reuse-the-template"></a>Reutilizar o modelo
No tutorial, criou um modelo para definir as entidades do Data Factory e um modelo para transmitir os valores dos parâmetros. O pipeline copia dados de uma Conta de armazenamento do Azure para uma base de dados SQL do Azure especificada através de parâmetros. Para utilizar o mesmo modelo para implementar entidades do Data Factory em diferentes ambientes, pode criar um ficheiro de parâmetro para cada ambiente e utilizá-lo quando implementar nesse ambiente.     

Exemplo:  

```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Dev.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Test.json
```
```PowerShell
New-AzureRmResourceGroupDeployment -Name MyARMDeployment -ResourceGroupName ADFTutorialResourceGroup -TemplateFile ADFCopyTutorialARM.json -TemplateParameterFile ADFCopyTutorialARM-Parameters-Production.json
```

Tenha em atenção que o primeiro comando utiliza o ficheiro de parâmetro para o ambiente de desenvolvimento, o segundo para o ambiente de teste e o terceiro para o ambiente de produção.  

Também pode reutilizar o modelo para efetuar tarefas repetidas. Por exemplo, tem de criar muitas fábricas de dados com um ou mais pipelines que implementem a mesma lógica, mas cada fábrica de dados utiliza diferentes contas de armazenamento do Azure e da Base de Dados SQL do Azure. Neste cenário, utilize o mesmo modelo no mesmo ambiente (programação, teste ou produção) com os diferentes ficheiros de parâmetro para criar fábricas de dados.   

## <a name="see-also"></a>Veja também
| Tópico | Descrição |
|:--- |:--- |
| [Pipelines](data-factory-create-pipelines.md) |Este artigo ajuda-o a compreender os pipelines e as atividades no Azure Data Factory. |
| [Conjuntos de dados](data-factory-create-datasets.md) |Este artigo ajuda-o a compreender os conjuntos de dados no Azure Data Factory. |
| [Agendamento e execução](data-factory-scheduling-and-execution.md) |Este artigo explica os aspetos de agendamento e execução do modelo da aplicação do Azure Data Factory. |
