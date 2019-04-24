# 序言

很久以前，在一个遥远的数据中心，一群被称为系统管理员的人正在手动部署基础设施。每个服务器，每条路由表命令，每个数据库配置和每个负载均衡器都是手动创建和管理的。这是一个黑暗和恐惧的时代：害怕停工，害怕意外错误配置，害怕部署缓慢及出错，并担心如果系统管理员休假了将会发生什么。好消息是，感谢DevOps Rebel联盟，现在有更好的方法可以做到：Terraform。

Terraform是一个开源工具，它允许您使用简单的声明性编程语言将基础架构定义为代码，并在各种云服务提供商（包括Amazon Web Services，Azure，Google Cloud，DigitalOcean和许多云平台）使用一些命令。例如，不是手动点击网页或运行数十个命令，而是配置服务器所需的所有代码：

```text
resource "aws_instance" "example" { ami = "ami-40d28157" instance_type = "t2.micro"
}
```

要部署它，您只需运行一个命令。

```text
terraform apply
```

由于其简单性和强大功能，Terraform迅速成为DevOps世界的主导者。它不仅取代了手动系统管理员工作，还取代了许多旧的基础架构，如Chef，Puppet，Ansible，SaltStack和CloudFormation等代码工具。

这是一个实践教程，不仅教你DevOps和基础设施作为代码原则，而且还引导您完成几个可以在家尝试的代码示例，因此请确保您的计算机就在您手边。 

当您完成时，您将准备好在现实世界中使用Terraform。 

## 谁应该读这本书 

本书适用于系统管理员，运营工程师，部署工程师，站点可靠性工程师，DevOps工程师，基础架构开发人员，全栈开发人员，工程经理，CTO以及编写代码后负责代码的任何其他人员。无论您的职位是什么，如果您是管理基础架构，部署代码，配置服务器，扩展集群，备份数据，监控应用程序以及在凌晨3点响应故障，那么本书就适合您。 

总的来说，所有这些任务通常被称为“操作”。虽然有许多开发人员知道如何编写代码，但理解操作的知识却少得多。你可以在过去侥幸逃脱，但在现代世界中，随着云计算和DevOps运动变得无处不在，几乎每个开发人员都需要为他们的工具带添加操作技能。 

您不需要已经是操作专家来阅读本书 —— 熟悉基本的编程，命令行和基于服务器的软件（例如网站）就已经足够 - 但如果您想要成为将基础设施作为代码进行管理这一领域的专家，您就有必要阅读本书。事实上，您不仅将学习如何使用Terraform通过代码方式管理基础架构，还将学习如何适应整个DevOps世界。以下是您在本书末尾可以回答的一些问题：

 •为什么要通过代码方式管理基础设施？

 •何时应该使用Chef，Ansible和Puppet等配置管理工具？

 •何时应该使用Terraform，CloudFormation和OpenStack Heat等编排工具？

 •何时应使用Docker和Packer等服务器模板工具？

 •Terraform如何运作以及如何使用它来管理我的基础设施？

 •您如何使Terraform成为自动化部署过程的一部分？

 •您如何使Terraform成为自动化测试过程的一部分？ 

•作为团队，使用Terraform的最佳实践是什么？

## 我为什么写这本书 

Terraform是一款功能强大的工具。它适用于所有流行的云提供商。它使用干净，简单的语言，强大地支持重用，测试和版本控制。它是开源的，拥有友好，活跃的社区。但是有一个领域缺乏：年龄。 在撰写本文时，Terraform仅仅2岁。因此，很难找到书籍，博客文章或专家来帮助您掌握该工具。如果您尝试从官方文档中学习Terraform，您会发现它在引入基本语法和功能方面做得很好，但它几乎不包含有关惯用模式，最佳实践，测试，可重用性或团队工作流程。这就像试图通过只学习词汇而不是任何语法或习惯来熟练掌握法语。 我写这本书的原因是为了帮助开发人员熟练掌握Terraform。我一直在使用Terraform超过其生命的一半，其中大部分是在我的公司Gruntwork的专业背景下，我花了很多时间来确定什么有效，什么不是主要通过试验 - 错。我的目标是分享我所学到的知识，这样你就可以避免这个漫长的过程，并在几个小时内流利。 当然，你只能通过阅读才能流利。为了精通法语，你将不得不花时间与法语母语人士交谈，观看法国电视节目和听法语音乐。要熟练使用Terraform，您必须编写真正的Terraform代码，用它来管理真实软件，并在真实服务器上部署该软件（不用担心，所有示例都使用Amazon Web Service的免费套餐，所以它不应该'你花了什么钱）。因此，准备好读取，写入和执行大量代码。



## 你会在本书中找到哪些内容？

以下是本书涵盖内容的概述： 

第1章，为何选择Terraform 

DevOps如何改变我们运行软件的方式;作为代码工具的基础架构概述，包括配置管理，编排和服务器模板;基础设施作为代码的好处; Terra- form，Chef，Puppet，Ansible，Salt Stack和CloudFormation的比较。 

 第2章，Terraform语法简介 

安装Terraform；Terraform语法概述；Terra-form CLI工具概述；如何部署单个服务器；如何部署Web服务器；如何部署Web服务器集群；如何部署负载均衡器；如何清理您创建的资源。 

第3章，如何管理Terraform状态 

什么是Terraform状态;如何存储状态，以便多个团队成员可以访问它;如何锁定状态文件以防止并发问题;如何隔离状态文件以限制错误造成的损害; Terraform项目的最佳实践文件和文件夹布局;如何使用只读状态。 

第4章，如何使用Terraform模块创建可重用的基础架构 

什么是模块;如何创建基本模块;如何使模块可配置;版本化模块;模块提示和技巧;使用模块来定义可重复使用的可配置基础架构。 

第5章，Terraform提示和技巧：循环，if语句，部署和陷阱

高级Terraform语法;循环; if语句; if-else语句;插值函数;零停机部署;常见的Terraform陷阱和陷阱。

 ???，作为一个团队 ，如何使用Terraform

版本控制; Terraform的黄金法则;编码指南; Terraform风格; Terraform的自动化测试;文件;团队的工作流程;使用Terraform实现自动化。 

您可以阅读本书从头到尾或跳到最感兴趣的章节。在本书的最后，您将找到推荐阅读列表，您可以在其中了解有关Terraform操作，基础架构作为代码和DevOps的更多信息。



## 你在本书中找不到哪些内容 

本书并不是Terraform的详尽参考手册。我没有涵盖所有云提供商，或每个云提供商支持的所有资源，或每个可用的Terraform命令。对于这些细节，我建议您使用Terraform文档。 该文档包含许多有用的答案，但如果您是Terraform，基础架构作为代码或操作的新手，您甚至不知道要问什么问题。因此，本书主要关注文档未涵盖的内容：即如何超越介绍性示例并在实际环境中使用Terraform。我的目标是通过讨论您可能想要首先使用Terraform的原因，如何使其适应您的工作流程以及哪些实践和模式最适合您，从而快速启动并运行。 x \|前言 为了演示这些模式，我已经包含了许多代码示例。几乎所有这些都是建立在Amazon Web Services（AWS）之上的，尽管无论您使用哪种云提供商，基本模式都应该大致相同。我的目标是让您尽可能轻松地在家尝试这些示例。这就是为什么代码示例都集中在一个云提供商（您只需要注册一个服务，AWS，第一年提供免费套餐）。这就是我在书中省略了所有其他第三方依赖关系的原因（包括HashiCorp的付费服务，Atlas）。这就是我将所有代码示例作为开源发布的原因。

## 开源代码示例 

书中的所有代码示例都可以在以下URL中找到：[https://github.com/brikis98/terraform-up-and-running-code](https://github.com/brikis98/terraform-up-and-running-code) 在开始阅读之前，您可能想要检查此回购，以便您可以在自己的计算机上跟随所有示例：git clone git@github.com:brikis98/ terraform-up-and-running-code.git 

该仓库的代码示例逐章分解。值得注意的是，大多数示例都向您展示了章节末尾的代码。如果你想最大化你的学习，你最好自己从头开始编写代码。 您将在第2章开始编写，在那里您将学习如何使用Terraform从头开始部署基本的Web服务器集群。之后，请按照后续章节中有关如何发展和改进此Web服务器群集示例的说明进行操作。按照指示进行更改，尝试自己编写所有代码，并仅使用GitHub仓库中的示例代码来检查您的工作或让您自己解开。



## 使用代码示例 

本书旨在帮助您完成工作，欢迎您使用程序和文档中的示例代码。除非您复制了大部分代码，否则您无需联系O'Reilly进行许可。例如，编写使用本书中几个代码块的程序不需要许可。出售或分发O'Reilly书籍中的示例CD-ROM需要获得许可。通过引用本书并引用示例代码来回答问题不需要许可。将本书中的大量示例代码合并到产品文档中需要完成任务。 归因于此，但不是必需的。归属通常包括标题，作者，出版商和ISBN。例如：“Terraform：由Yevgeniy Brikman启动并运行（O'Reilly）。版权所有2016 Yevgeniy Brikman，9781491977088。” 如果您认为您对代码示例的使用不属于合理使用范围或上述许可，请随时通过permissions@oreilly.com与O'Reilly Media联系。






