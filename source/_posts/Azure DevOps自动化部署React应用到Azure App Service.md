---
title: Azure DevOps自动化部署React应用到Azure App Service
date: 2021-06-29 0:15:33
tags: DevOps, Azure, Azure DevOps, React, Azure App Service
---

## 前言

Azure DevOps 提供了项目管理和源代码管理的功能，它的**自动化**构建和发布可以让我们专注于业务逻辑，省去了一些CI/CD的繁杂琐事。

根据 Azure 的官方文档，

> Microsoft Azure 具有充分的自由和灵活性，可以随时随地生成、管理和部署应用程序，帮助用户实现目标。使用你首选的语言、框架和基础结构（甚至可使用你自己的数据中心和其他云）解决大大小小的难题。

本文将介绍如何使用Azure DevOps创建React应用的构建和发布管道**部署到** Azure App Service，快速上手Azure DevOps**自动化部署**

## 创建应用服务

要部署 React 应用，首先需要在 Azure 上创建应用服务。顾名思义，应用程序服务（**Azure App Service**）是用于构建、部署和扩展 Web 应用程序的平台。

以下是步骤：

1. 如果尚未在 Azure 上创建帐户，则可以选择免费帐户。

![App_Services](/images/2021-06/3e3e7f80-013c-4db8-8019-f817a8eaa742_Screenshot_2020-08-26_Microsoft_Azure.png)

2. 对于**订阅**，请选择您的免费订阅。为此应用程序创建一个新的 **Resource Group**。本文使用名为 **react-example-deploy** 的资源组。选择 **Node 10.14** 作为 **Runtime Stack**，选择 **Windows** 作为 **Operating System**。最后，请选择离您最近的地区或保持默认。

![App_Service_Config](/images/2021-06/c47aa07d-795c-4b10-b16d-2592f53c2c7d_Screenshot_2020-08-26_Microsoft_Azure.png)

3. 点击创建几分钟后，您的新应用服务将上线，您将看到这样的消息。你可以访问 {AppServiceName}.azurewebsites.net 来查看它的实际效果。现在是一个磨人页面，之后 React 应用程序将部署在同一个 URL 上。

![Deployment_Complete](/images/2021-06/7aacaf93-4334-48a0-ac54-ee0d87df11a5_Screenshot_2020-08-26_Microsoft_Azure.png)

## 创建应用服务

在这里，我们将使用 Azure DevOps 在我们的应用程序中简化持续集成和持续部署。

### 创建 Azure DevOps 组织

如果你已经创建了 Azure DevOps 组织，请随意跳过。以下是创建 Azure DevOps 组织的步骤：

1. 登录到 [Azure DevOps](https://go.microsoft.com/fwlink/?LinkId=307137).
2. 点击 **New organization**.

![New_Organization](/images/2021-06/c3d9ecdd-1219-459a-a104-461cd09f07d5_Screenshot_2020-08-26_Projects_-_Home.png)

3. 你会被问到是否继续. 选择 **继续**.

![Continue](/images/2021-06/a67a7903-00b0-48a3-8d04-3110b5ddbf55_Screenshot_2020-08-26_Signup.png)

4. 然后您将被要求为您的组织提供一个唯一的名称。输入一个唯一的名称并点击继续。本文使用名为 ReactExampleDeploy 的组织。
   反应示例部署

![ReactExampleDeploy](/images/2021-06/7f7435be-14c3-4d15-9f2a-0f09d076df99_Screenshot_2020-08-26_Signup.png)

您的组织现已创建，可以通过 https://dev.azure.com/{yourorganization} 访问，其中 yourorganization 是您组织的名称。

### Creating a Project创建项目

在 Azure DevOps 组织仪表板上，创建一个新项目。为您的项目指定一个唯一名称，然后点击创建项目

![New_Project](/images/2021-06/e63c7526-2144-404c-b9bb-5ba0b811ef98_Screenshot_2020-08-26_Projects_-_Home.png)

### 推动 React 项目

您现在将 React 应用程序从您的本地机器推送到您刚刚通过命令行创建的项目。

1. 在您的应用程序的根目录中，运行以下命令。

```bash
git init
git add .
git commit -m "Initial Commit"
```

bash

2. 在您的项目仪表板上，转到 Repos。复制从命令行推送现有存储库下的代码并在命令行中运行相同的代码。可能会要求您登录 Azure 帐户以继续操作。把代码push到仓库。

![Pushing Project](/images/2021-06/63fdbff3-7269-4eb7-8245-1bbb15fc6f6c_Screenshot_2020-08-26_Files_-_Repos.png)

运行此命令后，您可以在 Repos → Files 下看到您的 React 项目。![Pushed](/images/2021-06/b56256ad-25fc-4c7a-90c7-5ef83c9a6c17_Screenshot_2020-08-26_React_Deploy_Example_-_Repos.png)

现在你已将应用推送到 Azure DevOps 项目，下一步是创建用于构建工件的管道和用于部署 React 应用的发布管道。

## 创建构建工件管道

根据 Azure 的文档，“您可以使用 Azure Pipelines运行构建、执行测试并将代码（发布）自动部署到各种开发和生产环境。”

以下是创建构建工件管道的步骤。

1. 单击 **Pipelines** 选项卡然后单击**Create Pipeline**.

![Create a Pipeline](/images/2021-06/1ca5f76c-462d-41db-bbc3-92a5ccba3ed7_Screenshot_2020-08-26_Pipelines_-_Recent.png)

2. 当被问到“你的代码在哪里？”时，选择使用经典编辑器选项。

![Use the classic editor](/images/2021-06/60e35362-aaf2-48d5-b411-fc00492a299c_Screenshot_2020-08-26_New_pipeline_-_Pipelines.png)

4. 选择 **Azure Repos Git** 作为源，将其他所有内容保留为默认值，然后点击**Continue**.



![Azure Repos Git](/images/2021-06/da1f9012-08ff-4981-84f4-181107d118d8_Screenshot_2020-08-26_Select_a_build_pipeline_template_-_Azure_DevOps_Services.png)

5. 当要求提供模板时，选择**Empty job**.

![Empty_Job](/images/2021-06/6f77988f-d215-4d53-b002-c75c854ad5a0_Screenshot_2020-08-26_Select_a_build_pipeline_template_-_Azure_DevOps_Services.png)

6. 单击 + 号

![React CI](/images/2021-06/8f173509-c4f7-473b-b468-c0f96cfeed73_Screenshot_2020-08-26_React_Deploy_Example-CI_-_Azure_DevOps_Services.png)

搜索“npm”并添加两次 npm 任务。

![npm](/images/2021-06/617d2c57-1221-4ebf-9ae0-2eabea5dd49e_Screenshot_2020-08-27_React_Deploy_Example-CI_-_Azure_DevOps_Services.png)

你可能会问，为什么要两次？由于此任务用于node包，因此您需要另一个用于构建项目的任务；不存在默认任务，因此您需要为此创建自定义任务。

点击第二个npm任务，配置如下。

- 将显示名称更改为 **npm run build**.
- 将命令更改为自定义.
- 在命令和参数中添加运行构建

![npm run build](/images/2021-06/f676d67a-3492-43d6-9ce1-8822c1d9426c_Screenshot_2020-08-27_React_Deploy_Example-CI_-_Azure_DevOps_Services.png)

7. 现在，搜索发布构建工件并将其添加到代理作业 1。

![Publish build artifacts](/images/2021-06/d150b433-a058-4fe5-b534-2e8361bf4665_Screenshot_2020-08-27_React_Deploy_Example-CI_-_Azure_DevOps_Services.png)

单击 **Publish build artifacts** 然后修改 **Path to publish**为 **build**.

![Path to publish](/images/2021-06/85b1ba09-a21c-4fa0-9364-71f8c980fcf4_Screenshot_2020-08-27_React_Deploy_Example-CI_-_Azure_DevOps_Services.png)

8. 转到触发器选项卡并选择启用持续集成。您可以将其他所有内容保留为默认值。

![CI](/images/2021-06/1e0eb583-8f74-46da-b045-219fc4df12b9_Screenshot_2020-08-27_React_Deploy_Example-CI_-_Azure_DevOps_Services.png)

9. 最后，单击  **Save & queue** 并再次点击 **Save & queue**。

![Save & queue](/images/2021-06/d6676f6e-f877-4962-838d-3ab3bda0a66b_Screenshot_2020-08-27_React_Deploy_Example-CI_-_Azure_DevOps_Services.png)

在运行管道模式中，将所有内容保留为默认值，然后点击保存并运行。

![Save and run](/images/2021-06/7db6e272-598c-4dd2-a271-7ff321cd2b04_Screenshot_2020-08-27_React_Deploy_Example-CI_-_Azure_DevOps_Services.png)

几分钟后，管道将成功完成运行。您可以在 Pipelines 选项卡下检查以验证它。

## 创建发布管道

发布管道将部署在上一节中构建的工件。

以下是创建发布管道的步骤：

1. 转到Pipelines** → **Releases**  然后单击 **New pipeline**.

![New pipeline release](/images/2021-06/a6befd50-2ba4-419b-932c-02a0f0aa6517_Screenshot_2020-08-27_Releases_-_Pipelines.png)

2. 选择**Azure App Service Deployment** 然后点击 **Apply**.

![Azure App Service Deployment](/images/2021-06/7e4fc497-2b42-4f50-8361-509108076c04_Screenshot_2020-08-27_New_release_pipeline_-_Pipelines.png)

您将被要求为**Stage**命名。进入 **Production** 并关闭模态。

![Production](/images/2021-06/1a9f6b2b-3464-4d40-9515-26b9fdceb93c_Screenshot_2020-08-27_New_release_pipeline_-_Pipelines.png)

3. 现在点击 **Add an artifact**.

![Add an artifact](/images/2021-06/0846557d-4dac-4e79-9fef-6248539e3be8_Screenshot_2020-08-27_New_release_pipeline_-_Pipelines.png)

对于 Source，选择您在上一节中创建的管道，即 React Deploy Example-CI。将其他所有内容保留为默认值，然后点击添加。

![React Deploy Example-CI](/images/2021-06/369d13ca-013e-40bb-9e93-39f2c6a8a7f3_Screenshot_2020-08-27_New_release_pipeline_-_Pipelines.png)

4. 要配置持续部署，请单击 Artifacts 中的闪电图标。

![Ligthning](/images/2021-06/0b6b47a5-a83b-4c2f-a9ae-c6a84a45eee6_Screenshot_2020-08-27_New_release_pipeline_-_Pipelines.png)

启用第一个禁用选项，在 Build 分支中输入 master，然后关闭模态框。

![master](/images/2021-06/a9770a53-78ba-4114-9d84-560bda45771c_Screenshot_2020-08-27_New_release_pipeline_-_Pipelines.png)

5. 在 **Stages**下面 点击 **1 job, 1 task** .

![Stages](/images/2021-06/14d07b88-ca17-4f3b-8383-b3aaa45d95ea_Screenshot_2020-08-27_New_release_pipeline_-_Pipelines.png)

会弹出配置 **Deploy Azure App Service**.

![Deploy Azure App Service](/images/2021-06/e7e9a20d-480e-471c-9d7f-7b430ebc8f36_Screenshot_2020-08-27_New_release_pipeline_-_Pipelines.png)

根据您的订阅选择 Azure 订阅。您可能需要单击授权并授权订阅。

![Azure subscription](/images/2021-06/3b2f08e1-7658-45fb-a7ec-5f0428165cf4_Screenshot_2020-08-27_New_release_pipeline_-_Pipelines.png)

![App service name](/images/2021-06/34047e33-fe7f-402f-a19d-022ac245b665_Screenshot_2020-08-27_New_release_pipeline_-_Pipelines.png)

点击 **Deploy Azure App Service**, 然后在 **Package or folder** 输入:

```
$(System.DefaultWorkingDirectory)/_React Deploy Example-CI/drop
```

![drop](/images/2021-06/445bb1b9-e451-426c-9490-835a46d81a5d_Screenshot_2020-08-27_New_release_pipeline_-_Pipelines.png)

6. 最后点击保存

![Save](/images/2021-06/9dc625f8-0746-474c-931b-1ed7d0fbbe3a_Screenshot_2020-08-27_New_release_pipeline_-_Pipelines.png)

Leave the modal that appears at default and hit **OK**.

![OK](/images/2021-06/ad975bb4-5ca9-4f7e-a318-1f282c5dd6c5_Screenshot_2020-08-27_New_release_pipeline_-_Pipelines.png)

现在，单击 Create release，它将出现在 Save 按钮的右侧。

![Create release](/images/2021-06/9e0e1305-1fd2-4f6d-a1aa-21f83b120554_Screenshot_2020-08-27_New_release_pipeline_-_Pipelines.png)

创建发布

![Create](/images/2021-06/bbb2725a-d5a5-44d7-b1ea-02a49982c4de_Screenshot_2020-08-27_New_release_pipeline_-_Pipelines.png)

几分钟后，您的应用程序将部署在 {yourappservicename}.azurewebsites.net。

由于您已经配置了 CI/CD，您可以在本地机器上更新应用程序并将提交推送到 repo，这将触发新版本，并且您的 React 应用程序将被更新。

## 总结

在本文中，我们讨论了如何使用 CI/CD 将 React 应用程序部署到 Azure App Service 的过程。

下一步，可以去尝试探索 Azure 提供的不同功能。

以下是一些相关文档。

- [Azure App Service](https://azure.microsoft.com/en-in/services/app-service/#features)
- [Azure Pipelines](https://docs.microsoft.com/en-us/azure/devops/pipelines/?view=azure-devops)
