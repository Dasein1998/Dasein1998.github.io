---

title: PowerShell插件安装
date: 2021-02-23 23:23:23
tags: 
- 折腾

categories: 
- 工具 

---
因为写博客，以及闲的没事喜欢折腾，还是会经常用到Windows默认的shell： [Powershell](https://github.com/PowerShell/PowerShell)
微软将Powershell开源后，不仅可以在Windows还可以在Linux，Mac os上使用powershell。本文使用的是Powershell7，在此记录一下进行的一些简单配置。

PowerShell的官方文档：https://docs.microsoft.com/en-us/powershell/

### 安装Nuget和PowerShellGet

安装powershell官方的插件管理

```powershell
Install-PackageProvider -Name NuGet -Force
Install-Module -Name PowerShellGet -Force
```



### 安装[oh-my-posh](https://ohmyposh.dev)和[posh-git](https://github.com/dahlbyk/posh-git)

posh-git是优化PowerShell中使用git的插件，常常配合oh-my-posh使用。可以很方便查看git的当前分支和其他信息。

```powershell
Install-Module oh-my-posh -Scope CurrentUser -AllowPrerelease
Install-Module posh-git -Scope CurrentUser -AllowPrerelease -Force
Get-PoshThemes   查看所有oh-my-posh主题
Set-PoshPrompt -Theme lambda
```

```
在powershell中输入 $PROFILE 则可查看其配置文件

```

配置文件中添加：

```
Import-Module posh-git
Import-Module oh-my-posh
Set-PoshPrompt -Theme Lambda
```

### 自己的一些配置

#### 快捷访问

PowerShell没有alias，但是可以使用function函数来完成：

``` powershell
function blog {cd D:\blog}       字面意思：敲入blog 则对应命令 cd D:\blog
```

#### [z.lua](https://github.com/skywind3000/z.lua)

> z.lua 是一个快速路径切换工具，它会跟踪你在 shell 下访问过的路径，通过一套称为 Frecent 的机制（源自 FireFox），经过一段简短的学习之后，z.lua 会帮你跳转到所有匹配正则关键字的路径里 Frecent 值最高的那条路径去。  

还是为了偷懒，因为我经常```cd D:\blog``` ，现在只需要``` z b``` 就行了。

##### 配置：

1. 安装lua
   1. 下载编译好的适用于win10的[lua库](http://joedf.ahkscript.org/LuaBuilds/)
   2. 解压到你喜欢的地方
   3. 复制目录，添加到系统变量
2. 安装[z.lua](https://github.com/skywind3000/z.lua)
   1. 去release下载[z.lua](https://github.com/skywind3000/z.lua) 
   2. 解压到你喜欢的地方
   3. 复制文件位置

``` powershell
Invoke-Expression (& { (lua /path/to/z.lua --init powershell) -join "`n" })
/path/to/z.lua   就是z.lua解压的目录
```

记录一下：

- zsh中使用，需要在.zshrc中添加

``` 
eval "$(lua /path/to/z.lua --init zsh)"
```

- Fish Shell中使用，创建文件

```
~/.config/fish/conf.d/z.fish
```

内容为：

```
source (lua /path/to/z.lua --init fish | psub)
```

或者

```
lua /path/to/z.lua --init fish > ~/.config/fish/conf.d/z.fish
```

- Bash中使用，在.bashrc中添加

  ```
  eval "$(lua /path/to/z.lua --init bash)"
  ```

  为了使用增强模式则：

  ``` 
  eval "$(lua /path/to/z.lua --init bash enhanced once)"
  ```

  

#### 代理

恩，从网上抄了一个代理的脚本，将其复制到`$PROFILE`中。

`Env-Proxy`设置系统代理

`Git-Proxy`设置git代理

`Scoop-Proxy`设置scoop代理等 具体阅读脚本。（我也看不懂 但是能用）



```powershell
$defalut_proxy = 'http://127.0.0.1:7890';

if ([System.Environment]::GetEnvironmentVariable('DEFAULT_PROXY') -ne $null) {
    $defalut_proxy = [System.Environment]::GetEnvironmentVariable('DEFAULT_PROXY');
}

function Env-Proxy($operate, $proxy = $defalut_proxy) {
    if ($operate -eq "get") {
        return $env:HTTP_PROXY;
    }
    elseif ($operate -eq "set") {
        $env:HTTP_PROXY = $proxy;
        $env:HTTPS_PROXY = $proxy;
        return "Success";
    }
    elseif ($operate -eq "del") {
        if ($env:HTTP_PROXY -ne $null) {
            Remove-Item env:HTTP_PROXY
            Remove-Item env:HTTPS_PROXY
        }
        return "Success";
    }
}

function Git-Proxy($operate, $proxy = $defalut_proxy) {
    if ($operate -eq "get") {
        return (git config --global --get http.proxy);
    }
    elseif ($operate -eq "set") {
        git config --global http.proxy $proxy;
        git config --global https.proxy $proxy;
        return "Success";
    }
    elseif ($operate -eq "del") {
        git config --global --unset http.proxy
        git config --global --unset https.proxy
        return "Success";
    }
}

function Node-Proxy($operate, $proxy = $defalut_proxy) {
    if ($operate -eq "get") {
        $yarn_proxy = (yarn config get proxy);
        if ($yarn_proxy -eq "undefined") {
            return $null;
        }
        else {
            return $yarn_proxy;
        }
    }
    elseif ($operate -eq "set") {
        npm config set proxy $proxy | Out-Null;
        npm config set https-proxy $proxy | Out-Null;
        yarn config set proxy $proxy | Out-Null;
        yarn config set https-proxy $proxy | Out-Null;
        return "Success";
    }
    elseif ($operate -eq "del") {
        npm config delete proxy | Out-Null
        npm config delete https-proxy | Out-Null
        yarn config delete proxy | Out-Null
        yarn config delete https-proxy | Out-Null
        return "Success";
    }
}

function Composer-Proxy($operate, $proxy = $defalut_proxy) {
    return (Env-Proxy $operate $proxy);
}

function Scoop-Proxy($operate, $proxy = $defalut_proxy) {
    if ($operate -eq "get") {
        $scoop_proxy = (scoop config proxy);
        if ($scoop_proxy -eq "'proxy' is not set") {
            return $null;
        }
        else {
            return $scoop_proxy;
        }
    }
    elseif ($operate -eq "set") {
        scoop config proxy $proxy.Substring(7) | Out-Null
        return "Success";
    }
    elseif ($operate -eq "del") {
        scoop config rm proxy | Out-Null
        return "Success";
    }
}

# 脚本部分
if ($args[0] -in "set", "get", "del") {
    $used_proxy = $defalut_proxy;
    if ($args[2] -ne $null) {
        $used_proxy = $args[2];
    }
    if ($args[1] -eq "env") {
        Write-Output (Env-Proxy $args[0] $used_proxy);
    }
    if ($args[1] -eq "git") {
        Write-Output (Git-Proxy $args[0] $used_proxy);
    }
    if ($args[1] -in "npm", "node", "yarn") {
        Write-Output (Node-Proxy $args[0] $used_proxy);
    }
    if ($args[1] -eq "composer") {
        Write-Output (Composer-Proxy $args[0] $used_proxy);
    }
    if ($args[1] -eq "all" -or $args[1] -eq $null) {
        Write-Output ("Env: " + (Env-Proxy $args[0] $used_proxy));
        Write-Output ("Git: " + (Git-Proxy $args[0] $used_proxy));
        Write-Output ("Node: " + (Node-Proxy $args[0] $used_proxy));
        Write-Output ("Composer: " + (Composer-Proxy $args[0] $used_proxy));
        Write-Output ("Scoop: " + (Scoop-Proxy $args[0] $used_proxy));
    }
    if ($args[1] -eq "proxy") {
        if ($args[0] -eq "set") {
            [System.Environment]::SetEnvironmentVariable('DEFAULT_PROXY', $args[2], [System.EnvironmentVariableTarget]::User);
        }
        elseif ($args[0] -eq "get") {
            [System.Environment]::GetEnvironmentVariable('DEFAULT_PROXY', [System.EnvironmentVariableTarget]::User);
        }
        elseif ($args[0] -eq "del") {
            [System.Environment]::SetEnvironmentVariable('DEFAULT_PROXY', '', [System.EnvironmentVariableTarget]::User);
        }
    }
}
```


---
关闭PowerShell的错误提示音：管理员权限运行
```
Set-Service beep -StartupType disabled
```

## 安装和重装后恢复scoop

安装scoop：

  > iwr -useb get.scoop.sh | iex

重装系统后，恢复scoop
1. 清除数据重装
    1. 重装系统之前, 先完整复制用户目录下的 scoop 文件夹到别的地方
    
	2. 重装系统之后, 将 scoop 文件夹粘贴回去用户目录
	3. 在环境变量设置中, 新建一个用户变量, 名字为 SCOOP, 值为当前 scoop 文件夹的地址, 即:
	  ```
	  C:\Users\xxxx\scoop
	  ```
	4. 允许脚本执行:
	  ```
	  set-executionpolicy remotesigned -s currentuser
	  ```
	5. 双击用户变量中的 path, 新建一个路径, 填入 :
	  ```
	  %SCOOP%\shims
	  ```
	6. 管理员权限 powershell 中运行:
	  ```
	  scoop reset *
	  ```
        即可恢复所有软件的正常使用.
2. 未清除数据重装

    从上述第三步开始。
