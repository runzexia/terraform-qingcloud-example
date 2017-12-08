# Terraform - Infrastructure as Code

## Terraform-QingCloud使用

### 1.terraform及terraform-provider-qingcloud安装

#### 安装terraform

我们首先安装terraform，我们需要进到terraform的官网找到[适合的软件包](https://www.terraform.io/downloads.html)进行下载。  
下载Terraform后，解压压缩包。压缩包中有一个名为terraform的文件，我们只需要这个二进制文件就可以使用terraform了。  
最后一步是设置terraform的PATH。如何在Linux和Mac中设置PATH可以参考这个[页面](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux-unix)，
如何在Windows当中设置PATH可以参考这个[页面](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows)

#### 验证terraform安装

在安装完terraform之后，我们可以打开一个新的终端来验证terraform安装成功了。  
执行`terraform`可以看到类似下面的输出：

```text
$ terraform
Usage: terraform [--version] [--help] <command> [args]

The available commands for execution are listed below.
The most common, useful commands are shown first, followed by
less common or more advanced commands. If you're just getting
started with Terraform, stick with the common commands. For the
other commands, please read the help and docs before usage.

Common commands:
    apply              Builds or changes infrastructure
    console            Interactive console for Terraform interpolations
# ...
```

#### 安装terraform-provider-qingcloud

terraform-provider-qingcloud同样是以二进制文件进行发布，我们可以到Github上找到[适合的软件包](https://github.com/yunify/terraform-provider-qingcloud/releases)进行下载。  
下载完成后里面会包含一个二进制文件，解压压缩包。  
在Linux以及Mac当中我们需要将文件名改为`terraform-provider-qingcloud`，并把这个二进制文件放到与terraform同一路径下。  
在Windows当中我们需要将文件名改为`terraform-provider-qingcloud.exe`，并把这个二进制文件放到与terraform同一路径下。 
