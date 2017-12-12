# Terraform - Infrastructure as Code

## 什么是Terraform？

Terraform是一个安全、高效的进行基础设施的部署、更改、版本化的工具。  

Terraform用配置文件来描述应用所需的组件。 
Terraform会根据配置文件生成一个执行计划，这个计划会列出为达到所需的状态而执行的操作，然后执行计划以达到期望的状态。  
如果配置被更改，Terraform能够通过比对得到更改的内容，并创建对应的增量执行计划。 
terraform通过插件机制管理不同的资源提供者，以此来接入各种资源，如虚拟机，存储，网络和各种应用服务。

## Terraform的主要特性

### Infrastructure as Code

基础架构使用配置语法进行描述，这可以让数据中心的构建计划可以像其他代码一样进行版本化和追踪。

### Execution Plans

Terraform有一个规划步骤，它生成一个执行计划。  
执行计划显示当您调用应用程序时Terraform将执行的操作。  
使用这个功能可以保证操作基础设施时不发生意外。

### Resource Graph

Terraform创建了一个所有资源的视图。这使得Terraform可以并行化没有依赖的创建与修改。  
因此，Terraform可以高效地构建基础架构。操作员也能更加了解环境的结构。 

### Change Automation

Terraform会自动的分析什么是需要修改的，从而避免了许多可能的人为错误。

## Terraform vs. Other Software

与Terraform类似的Infrastructure as Code工具大概有下面几种：

* [Chef](https://www.chef.io)
* [Puppet](https://puppet.com)
* [Ansible](https://www.ansible.com)
* [SaltStack](https://saltstack.com)
* [CloudFormation](https://aws.amazon.com/cn/cloudformation)

下面将从几个方面来说明Terraform与其他工具对比的优势

### 配置管理工具与编排工具

Chef, Puppet, Ansible, and SaltStack 都可以称为配置管理工具，这些工具的主要目标是在已经存在的机器上安装和管理软件。  
而Terraform和CloudFormation 可以称为编排工具，更注重于数据中心以及相关服务的高级抽象。他们的工作重点是创建资源并且引导进行初始化。  
而且在现在的环境下，大家使用容器等服务，镜像已经包括了软件的安装与配置。一旦你有了镜像，你需要的是一些服务器去运行它。  
对于提供服务器这种需求，编排工具会比配置管理工具更适合做此类工作。  

### 编程式语言与声明式语言

Chef and Ansible希望你去一步步编写程序以达到最终所期望的状态。  
Terraform, CloudFormation, SaltStack, and Puppet希望你去声明最终想要的资源与资源的状态，工具本身会自动分析达到想要的状态需要进行怎样的操作。  
在使用编程式语言时，工具不会获取历史的状态，所以我们需要考虑的更多以达到与之前版本的兼容。  
并且使用编程式语言会导致代码库变得越来越庞大，不利用人们理解与代码的复用。  
不过声明式的语言的表达能力是较为欠缺的，例如我们需要基础设施的滚动升级时，声明式的语言是很难满足要求的。  
为此Terraform提供了一些基础服务，例如[输入变量](https://www.terraform.io/intro/getting-started/variables.html)，[输出变量](https://www.terraform.io/intro/getting-started/outputs.html)，[在销毁之前创建](https://www.terraform.io/docs/configuration/resources.html#create_before_destroy)等。  

### 客户端服务器架构与客户端架构

Chef, Puppet, and SaltStack在默认情况下都使用了客户端服务器架构。客户端（可能是Web UI或CLI工具）是用来发出命令（例如"deploy X"）的东西。  
这些命令到达一个服务器，它负责执行你的命令并存储系统的状态。要执行这些命令，服务器会与agent进行通信，agent必须在要配置的每个服务器上运行，这有几个缺点：  

* 您必须在每台服务器上安装并运行额外的软件。
* 为了配置管理，您必须部署额外的服务器（甚至是一组服务器以实现高可用性）。
* 由于客户端，服务器和代理都需要通过网络进行通信，因此您必须为其打开额外的端口，并配置相互验证。  
* 这些配置会引入大量不同类型的故障，当收到错误警告时，必须要弄清楚是哪一部分出现了故障。

CloudFormation也是客户端/服务器，但AWS透明地处理所有的服务器细节，作为最终用户，您只需要考虑客户端代码。Ansible客户端通过SSH直接连接到您的服务器。  
Terraform使用云提供商API来配置基础架构，因此除了您已经使用云提供商之外，没有新的身份验证机制，并且不需要直接访问您的服务器。  

### 比较表格

|          | Chef        | Puppet      | Ansible     | SaltStack   | CloudFormation | Terraform   |
| -------- | ----------- | ----------- | ----------- | ----------- | -------------- | ----------- |
| 是否开源     | 开源 | 开源 | 开源 | 开源 | 闭源  | 开源 |
| 支持的云    | All         | All         | All         | All         | AWS only       | All         |
| 工具类型     | 配置管理        | 配置管理        | 配置管理        | 配置管理        | 编排工具           | 编排工具        |
| 语言类型 | 编程式         | 声明式         | 编程式         | 声明式         | 声明式            | 声明式         |
| 架构       | 客户端服务器      | 客户端服务器      | 客户端         | 客户端服务器      | 客户端            | 客户端         |


## Terraform的使用场景

+ 多层应用的部署
+ 自动搭建全新的测试环境
+ 软件定义网络的配置
+ 多云环境的部署

## Terraform-QingCloud使用

### 1.terraform及terraform-provider-qingcloud安装

#### 安装terraform

我们首先安装terraform，我们需要进到terraform的官网找到[适合的软件包](https://www.terraform.io/downloads.html)进行下载。  
下载Terraform后，解压压缩包。压缩包中有一个名为terraform的文件，我们只需要这个二进制文件就可以使用terraform了。  
最后一步是设置terraform的PATH。如何在Linux和Mac中设置PATH可以参考这个[页面](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux-unix)，
如何在Windows当中设置PATH可以参考这个[页面](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows)

#### 验证terraform安装

在安装完terraform之后，我们可以打开一个新的终端来验证terraform安装成功了。  
执行`terraform -v`可以看到类似下面的输出：

```shell
$ terraform -v
Terraform v0.11.1
```

#### 安装terraform-provider-qingcloud

terraform-provider-qingcloud同样是以二进制文件进行发布，我们可以到Github上找到[适合的软件包](https://github.com/yunify/terraform-provider-qingcloud/releases)进行下载。  
下载完成后里面会包含一个二进制文件，解压压缩包。  
在Linux以及Mac当中我们需要将文件名改为`terraform-provider-qingcloud`，并把这个二进制文件放到用户的"Application Data" 目录下的`terraform.d/plugins/`当中.  
在Windows当中我们需要将文件名改为`terraform-provider-qingcloud.exe`，并把这个二进制文件放到`~/.terraform.d/plugins/`当中。 

### 2.terraform使用

我们将会介绍如何使用terraform，并且进行一键在青云平台创建下图的结构，并在主机当中运行docker以及nginx。  
例子源码：https://github.com/yunify/terraform-provider-qingcloud/tree/master/terraform/example/vpc_one_instance

> 注意    
> 使用terraform apply会创建实际的资源，将会产生一些费用。  

 ![topo.jpg](./images/topo.jpg)

#### 理解配置文件

像git一样，每个terraform项目都需要自己的目录，我们可以直接使用vpc_one_instance目录进行试验。  
在vpc_one_instance目录下面执行terraform相关命令时，terraform会加载这个目录下的`*.tf`文件。  
terraform的配置文件是HashiCorp公司的[HCL](https://www.terraform.io/docs/configuration/syntax.html)语言。

#### terraform init

与git类似，我们需要在terraform项目的根目录运行terraform init去初始化项目。  
在初始化项目的时候，terraform会解析目录下的`*.tf`文件并加载相关的provider插件。
在vpc_one_instance文件夹下运行`terraform init`会看到类似下面的输出：
```shell
$ terraform init
Initializing provider plugins...

The following providers do not have any version constraints in configuration,
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes, it is recommended to add version = "..." constraints to the
corresponding provider blocks in configuration, with the constraint strings
suggested below.

* provider.null: version = "~> 1.0"

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```
##### 验证terraform init
在vpc_one_instance文件夹下运行`terraform -v`会得到类似下面的输出：
```shell
$ terraform -v
Terraform v0.11.1
+ provider.null v1.0.0
+ provider.qingcloud (unversioned)

```

#### 指定provider

在`./vpc_one_instance/var.tf`文件我们指定了provider，qingcloud的provider需要`access_key`与`secret_key`进行调用API，key可以在Qingcloud Web控制台进行申请。  
`zone`指定了资源会在哪个区中进行创建，默认为pek3a区。

#### 理解resource

HCL语言是一种声明式语言，即在`*.tf`文件中声明了我们所期望的资源状态。  
我们在`vpc_one_instance/main.tf`文件当中指定了我们想要的资源以及他们的状态。  
在定义的资源的时候我们可以在一个资源当中引用其他资源的字段，terraform会自动解析这些引用并且按顺序进行创建。  
```hcl
resource "qingcloud_security_group_rule" "ssh-in" {
  security_group_id = "${qingcloud_security_group.foo.id}" //引用别名为foo的qingcloud_security_group的id
  protocol          = "tcp"
  priority          = 0
  action            = "accept"
  direction         = 0
  from_port         = 22
  to_port           = 22
}
```
在上面的例子当中，`qingcloud_security_group_rule`为资源的名称，需要provider支持特定的资源。  
`ssh-in`为资源的别名，是在这个项目当中唯一的。  
上面我们创建了一个类型为`qingcloud_security_group_rule`的资源，也就是一个防火墙规则资源。  
在这个资源中我们指定了防火墙的ID，以及规则的协议、优先级、动作、方向以及端口范围。  

* qingcloud_eip.foo:创建一个带宽为2的弹性公网IP  
* qingcloud_keypair.foo:使用`~/.ssh/id_rsa.pub`的文件内容创建一个SSH key 
* qingcloud_security_group.foo:创建一个名称为first_sg的防火墙
* qingcloud_security_group_rule.http-in:为防火墙添加一条接收80端口TCP请求的规则
* qingcloud_security_group_rule.http-in:为防火墙添加一条接收22端口TCP请求的规则
* qingcloud_vpc.foo:创建一个vpc网络，并且绑定了防火墙与弹性公网IP,VPC的子网范围为`192.168.0.0/16`
* qingcloud_vxnet:创建一个受管的vxnet，并且加入VPC当中，子网范围是`192.168.0.0/24`
* qingcloud_instance.foo:创建一个实例，绑定了上面创建的SSH key，并且加入到了vxnet当中
* qingcloud_vpc_static.http-portforward:为VPC添加一条端口转发规则，将80端口的请求转发到instance的80端口当中
* qingcloud_vpc_static.ssh-portforward:为VPC添加一条端口转发规则，将22端口的请求转发到instance的22端口当中

#### 使用Provisioners进行环境配置

Provisioners可以在资源创建/销毁时在本地/远程执行脚本。  
Provisioners通常用来引导一个资源，在销毁资源前完成清理工作，进行配置管理等。  
Provisioners拥有多种类型可以满足多种需求，如：文件传输(`file`)，本地执行(`local-exec`)，远程执行(`remote-exec`)等
Provisioners可以添加在任何的resource当中：  

```hcl-terraform
resource "qingcloud_instance" "foo" {
  # ...

  provisioner "local-exec" {
    command = "echo ${self.private_ip} > file.txt"
  }
}
```

在example当中，我们使用了null_resource再加上provisioner完成了在qingcloud_instance上安装docker并启动docker-nginx。  
在`null_resource.run_docker_nginx`当中，我们指定了`depends_on`参数，保证了在所有依赖资源创建完成后再进行执行`provisioner`。  

#### 执行terraform plan查看terraform计划

`terraform plan`命令用于输出执行计划。除非明确禁用，terraform会调用refresh方法重新查询当前资源的状态。  
完成状态刷新后，terraform会自动分析要进行的操作以达到配置文件中所需要的状态，并把分析的结果输出出来。
在vpc_one_instance文件夹下执行`terraform plan`会`得到类似下面的结果：
```shell
$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.


------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + null_resource.run_docker_nginx
      id:                <computed>

  + qingcloud_eip.foo
      id:                <computed>
      addr:              <computed>
      bandwidth:         "2"
      billing_mode:      "bandwidth"
      need_icp:          "0"
      resource.%:        <computed>
      tag_names.#:       <computed>

  + qingcloud_instance.foo
      id:                <computed>
      cpu:               "1"
      image_id:          "centos73x64"
      instance_class:    "0"
      keypair_ids.#:     <computed>
      managed_vxnet_id:  "${qingcloud_vxnet.foo.id}"
      memory:            "1024"
      private_ip:        <computed>
      public_ip:         <computed>
      security_group_id: <computed>
      tag_names.#:       <computed>

......


Plan: 11 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.

```

#### 使用terraform apply提交资源创建及配置

`terraform apply`命令用于应用所需的更改以达到所需的配置状态。
为了更加方便的得到我们所关注的输出结果，可以使用output单独输出部分字段。
如在`vpc_one_instance/main.tf`当中，我们单独获取了`qingcloud_vpc.foo`的public_ip：

```hcl-terraform
output "ip" {
  value = "${qingcloud_vpc.foo.public_ip}"
}
```

填写`vpc_one_instance/var.tf`中的`access_key`与`secret_key`后，我们使用`terraform apply`可以完成资源的创建与配置。

> 注意    
> 在example中是根据文件来获取SSH key，在您机器上可能不存在此文件，请您自行创建SSH key。  

[如何创建SSH key](https://stackoverflow.com/questions/14102116/how-to-generate-ssh-on-github-using-terminal)

我们会在输出的结尾获取到类似下图的输出：  
 ![output.jpg](./images/output.jpg)  
 打开浏览器，输入output的IP，可以看到nginx以及响应请求：  
 ![nginx.jpg](./images/nginx.jpg)  

 
## Reference
Terraform官网：[https://www.terraform.io](https://www.terraform.io)  
Terraform青云官方Github：[https://github.com/yunify/terraform-provider-qingcloud](https://github.com/yunify/terraform-provider-qingcloud)  
Why we use Terraform ： https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c  
Multi-Cloud模式下Terraform和Packer开源工具实践分享：[https://yq.aliyun.com/articles/79509](https://yq.aliyun.com/articles/79509)  
