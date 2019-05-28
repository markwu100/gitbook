# 第2章 Terraform语法简介

在本章中，您将学习Terraform语法的基础知识。这是一种比较简单的编程语言，所以在大约25页的篇幅中，你将从第一页开始 Terraform一直使用Terraform来部署服务器集群 使用负载均衡器在其间分配流量。这个基础设施很好 运行可扩展，高可用性Web服务和微服务的起点。 在随后的章节中，您将进一步发展此示例。 Terraform可以为许多不同类型的云提供商提供基础架构， 包括亚马逊网络服务（AWS），Azure，谷歌云，DigitalOcean和 很多其他的。对于本章中的几乎所有代码示例以及其余部分 预订，您将使用AWS。 AWS是学习Terraform的不错选择 因为： •到目前为止，AWS是最受欢迎的云基础架构提供商。它占45％的份额 在云基础设施市场，这是超过下三个最大的市场 竞争对手（微软，谷歌和IBM）合并 •AWS提供大量可靠且可扩展的云托管服务， 包括Elastic Compute Cloud（EC2），AKA虚拟服务器，Auto Scaling 组（ASG），AKA服务器集群和Elastic Load Balancing（ELB） AKA负载均衡器。

AWS提供了一个丰富的免费套餐，可以让您运行所有这些示例 免费。 如果你已经用完了你的免费等级学分，那么就是这个例子 书还应该花费你不超过几美元。 如果您之前从未使用过AWS或Terraform，请不要担心，因为本教程是专门设计的 对于这两种技术的新手。 我将引导您完成以下步骤：

•设置您的AWS账户 

•安装Terraform

 •部署单个服务器

 •部署单个Web服务器

 •部署Web服务器群集

 •部署负载均衡器

 • 清理

## 设置您的AWS账户 

如果您还没有AWS账户，请访问[https://aws.amazon.com/](https://aws.amazon.com/) 注册。 首次注册AWS时，您最初以root用户身份登录。 这个用户 帐户具有访问权限，可以在帐户中执行任何操作，因此从a 从安全角度来看，每天使用root用户并不是一个好主意。 在 事实上，您应该使用root用户的唯一方法是创建其他用户帐户 权限更有限，并立即切换到其中一个帐户 要创建更有限的用户帐户，您需要使用Identity and Access 管理（IAM）服务。 IAM是您管理用户帐户的地方 每个用户的权限。 要创建新的IAM用户，请转到IAM控制台， 单击“用户”，然后单击蓝色的“创建新用户”按钮。 输入用户的名称 并确保选中“为每个用户生成访问密钥”，如下所示 图2-1。



单击“创建”按钮，AWS将显示该安全凭据 用户，包括访问密钥ID和秘密访问密钥，如图所示 图2-2。 您必须立即保存，因为它们永远不会再显示。 我建议 将它们存储在安全的地方（例如密码管理器，如1Password， LastPass或OS X Keychain）所以你可以在本教程中稍后使用它们。

图2-2。将您的AWS凭证存储在安全的地方。切勿与任何人分享。 别担心，截图中的内容是假的。 保存凭据后，单击“关闭”按钮（两次），您就可以了 被带到IAM用户列表。单击刚刚创建的用户，然后选择“权限” 标签。默认情况下，新的IAM用户没有任何权限，因此， 在AWS账户中无法执行任何操作。 要授予IAM用户执行某些操作的权限，您需要关联一个或多个 具有该用户帐户的IAM策略。 IAM策略是一个JSON文档 定义用户是或不被允许做什么。您可以创建自己的IAM策略或 使用一些预定义的IAM策略，称为托管策略 要运行本书中的示例，您需要添加以下托管策略 给你的IAM用户，如图2-3所示： 

1. AmazonEC2FullAccess：本章要求。 

2. AmazonS3FullAccess：第3章要求。

 3. AmazonDynamoDBFullAccess：第3章所需。

 4. AmazonRDSFullAccess：第3章所要求的。

5. CloudWatchFullAccess：第5章所要求的。

6. IAMFullAccess：第5章要求。



#### 关于默认VPC的说明 

请注意，如果您使用的是现有AWS账户，则必须使用 有一个默认的VPC。 VPC或虚拟私有云是一种 您的AWS账户的隔离区域，拥有自己的虚拟网络 和IP地址空间。 几乎每个AWS资源都部署到一个 VPC。 如果您没有明确指定VPC，那么资源将是 部署到默认VPC中，这是每个新AWS的一部分 帐户。 本书中的所有示例都依赖于此默认VPC，因此 如果由于某种原因您删除了帐户中的那个，请使用 不同的区域（每个区域都有自己的默认VPC）或联系人 AWS客户支持，他们可以重新创建默认VPC 你（没有办法自己将VPC标记为“默认”）。 除此以外， 您需要更新几乎所有示例以包含a 指向自定义VPC的vpc\_id或subnet\_id参数。

## 安装Terraform 

您可以从Terraform主页下载Terraform。 单击下载 链接，为您的操作系统选择适当的包，下载zip 存档，并将其解压缩到您希望安装Terraform的目录中。该archive将提取一个名为terraform的二进制文件，您需要将其添加到您的文件中 PATH环境变量。 要检查是否有效，请运行terraform命令，您应该看到 使用说明：

```text
terraform
usage: terraform [--version] [--help] <command> [<args>]
Available commands are:
apply Builds or changes infrastructure
destroy Destroy Terraform-managed infrastructure
get Download and install modules for the configuration
graph Create a visual graph of Terraform resources
```

为了让Terraform能够在您的AWS账户中进行更改，您将会这样做 需要为先前创建的IAM用户设置AWS凭据作为环境变量： AWS\_ACCESS\_KEY\_ID and  AWS\_SECRET\_ACCESS\_KEY:

```text
> export AWS_ACCESS_KEY_ID=(your access key id) 
> export AWS_SECRET_ACCESS_KEY=(your secret access key)
```

#### 验证选项 

除了环境变量，Terraform也支持相同的功能 身份验证机制与所有AWS CLI和SDK工具一样。 因此， 它还能够在〜/ .aws /凭证中使用凭证， 如果您运行configure，则会自动生成 您可以添加到AWS CLI或IAM角色的命令 AWS中几乎所有资源。 有关详细信息，请参阅配置 AWS命令行界面。

## 部署单个服务器 

Terraform代码在HashiCorp配置语言（HCL）中以文件形式编写 扩展名为.tf。 它是一种声明性语言，所以你的目标是描述 你想要的基础设施，Terraform将弄清楚如何创建它。 Terraform可以 在各种平台上创建基础架构，或称为提供商， 包括AWS，Azure，Google Cloud，DigitalOcean等等。 您可以在任何文本编辑器中编写Terraform代码。 如果你四处搜寻 （注意，你可能需要搜索单词“HCL”而不是“Terraform”），你可以 找到Terraform语法突出显示支持大多数编辑器，包括vim，emacs， Sublime Text，Atom，Visual Studio Code和IntelliJ（后者甚至支持 重构，找到用法，并去宣言）。

使用Terraform的第一步通常是配置您想要的提供程序 使用。 创建一个空文件夹，并在其中放入一个名为main.tf的文件，其中包含以下内容：

```text
provider "aws" {
    region = "us-east-1"
}
```

这告诉Terraform您将使用AWS作为您的提供商 您希望将基础架构部署到us-east-1区域。 AWS拥有数据中心 遍布全球，分为区域和可用区域。 AWS区域是 一个单独的地理区域，例如us-east-1（北弗吉尼亚州），eu-west-1（爱尔兰）， 和ap-southeast-2（悉尼）。 在每个区域内，有多个孤立的数据 称为可用区的中心，例如us-east-1a，us-east-1b等 对于每种类型的提供者，您可以创建许多不同类型的资源， 例如服务器，数据库和负载平衡器。 例如，部署单个服务器 在AWS中，称为EC2实例，您可以将aws\_instance资源添加到 main.tf：

```text
resource "aws_instance" "example" {
    ami = "ami-40d28157"instance_type = "t2.micro"
}
```

Terraform资源的通用语法结构如下： 

```text
resource "PROVIDER_TYPE" "NAME" {
    [CONFIG ...]
}
```

如果PROVIDER是提供者的名称（例如aws），则NAME是您可以使用的标识符 在整个Terraform代码中引用此资源（例如示例）和CONFIG 由一个或多个特定于该资源的配置参数组成 （例如ami =“ami-40d28157”）。 对于aws\_instance资源，有许多不同的 配置参数，但是现在，您只需要设置以下内容：

ami

要在EC2实例上运行的Amazon Machine Image（AMI）。 你可以找到 AWS Marketplace中的免费和付费AMI，或使用此类工具创建您自己的AMI 作为Packer（有关机器的讨论，请参见第23页的“服务器模板工具图像和服务器模板）。 上面的示例将ami参数设置为 us-east-1中Ubuntu 16.04 AMI的ID。

instance\_type

要运行的EC2实例的类型。 每种类型的EC2实例都提供了不同的实例 数量CPU，内存，磁盘空间和网络容量。 EC2实例 “类型”页面列出了所有可用选项。 上面的例子使用了t2.micro， 它有1个虚拟CPU，1GB内存，是AWS免费套餐的一部分。在终端中，进入创建main.tf的文件夹，然后运行terraform 计划命令：

```text
> terraform plan
Refreshing Terraform state in-memory prior to plan...
(...)
+ aws_instance.example
    ami: "ami-40d28157"
    availability_zone: "<computed>"
    instance_state: "<computed>"
    instance_type: "t2.micro"key_name: "
    <computed>"private_dns: "
    <computed>"private_ip: "<computed>"
    public_dns: "<computed>"
    public_ip: "<computed>"security_groups.#: "<computed>"
    subnet_id: "<computed>"
    vpc_security_group_ids.#: "<computed>"
    (...)
Plan: 1 to add, 0 to change, 0 to destroy.
```

plan命令可让您在实际制作之前查看Terraform将要执行的操作 变化。 这是一种很好的方法，可以在将代码释放到代码之前对其进行完整性检查 世界。 plan命令的输出类似于diff命令的输出 这是Unix，Linux和git的一部分：带有加号（+）的资源将会发生 要创建，带有减号（ - ）的资源将被删除，资源将被删除 波浪号（〜）将被修改。 在上面的输出中，您可以看到Terraform 正计划创建一个单独的EC2实例，而不是其他任何东西，这正是 你想要什么。

要实际创建实例，请运行terraform apply命令：

```text
> terraform apply
aws_instance.example: Creating...
    ami: "" => "ami-40d28157"
    availability_zone: "" => "<computed>"
    instance_state: "" => "<computed>"
    instance_type: "" => "t2.micro"
    key_name: "" => "<computed>"
    private_dns: "" => "<computed>"
    private_ip: "" => "<computed>"
    public_dns: "" => "<computed>"
    public_ip: "" => "<computed>"
    security_groups.#: "" => "<computed>"
    subnet_id: "" => "<computed>"
    vpc_security_group_ids.#: "" => "<computed>"
    (...)
    
    aws_instance.example: Still creating... (10s elapsed)
    aws_instance.example: Still creating... (20s elapsed)
    aws_instance.example: Creation complete
    
    Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

恭喜，您刚刚部署了Terraform服务器！ 要验证这一点，请转到 EC2控制台，您应该看到类似于图2-4的内容。

果然服务器在那里，尽管不可否认，这不是最激动人心的例子。 让我们更有趣一点吧。 首先，请注意EC2实例没有 有一个名字。 要添加一个，您可以向aws\_instance资源添加标记：

```text
resource "aws_instance" "example" {
    ami = "ami-40d28157"
    instance_type = "t2.micro"
    tags {name = "terraform-example"
    }
}
```

再次运行计划命令以查看这将执行的操作：

```text
> terraform plan
    aws_instance.example: Refreshing state... (ID: i-6a7c545b)
    (...)
    ~ aws_instance.example
        tags.%: "0" => "1"
        tags.Name: "" => "terraform-example"
        
    Plan: 0 to add, 1 to change, 0 to destroy.
```

Terraform会跟踪为这组模板创建的所有资源， 所以它知道你的EC2实例已经存在（注意Terraform说“刷新 state ...“当你运行计划命令时），它可以显示你之间的差异 目前已部署以及Terraform代码中的内容（这是其中一项优势 如在Terraform中所讨论的那样，在程序性语言中使用声明性语言 与第32页上的代码工具等其他基础结构相比较。 上面的差异 表明Terraform想要创建一个名为“Name”的标签，这正是如此 你需要，所以再次运行apply命令。 刷新EC2控制台时，您会看到类似于图2-5的内容。

## 部署单个Web服务器

 下一步是在此实例上运行Web服务器。 目标是部署最简单的 Web架构可能：一个可以响应HTTP的Web服务器 请求，如图2-6所示。

