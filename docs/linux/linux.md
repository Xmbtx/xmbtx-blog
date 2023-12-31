## 目录

### 进入目录：

- **`cd`命令：** 用于改变当前工作目录。

  ```
  cd /path/to/directory  # 进入指定路径的目录
  ```
  
- **特殊符号：** 你也可以使用一些特殊符号来导航文件系统：

  - `cd ..`：进入上一级目录。
  - `cd ~`：进入当前用户的主目录（home directory）。
  - `cd -`：进入上次所在的目录。
  - `cd ./`：当前目录
  - `cd  `：返回用户目录

### 显示当前工作目录：

- `pwd`命令：

   显示当前工作目录的路径。

  ```
  pwd
  ```

### 返回上一级目录：

- `cd ..`命令：

   进入上一级目录。

  ```
  cd ..
  ```

## 文件夹

### 创建单个文件夹：

```
mkdir folder_name
```

这会在当前工作目录下创建一个名为`folder_name`的文件夹。

### 创建多个文件夹：

```
mkdir folder1 folder2 folder3
```

这会在当前目录下同时创建`folder1`、`folder2`和`folder3`这三个文件夹。

### 创建嵌套文件夹：

```
mkdir -p path/to/nested/folders
```

这会创建一个嵌套的文件夹结构。如果路径中的某些文件夹不存在，`-p`选项会自动创建这些不存在的文件夹。

### 创建具有权限设置的文件夹：

```
mkdir -m 755 folder_name
```

## 文件查询

### 1. **查找文件**

- **`find`命令：** 用于在指定路径下查找文件。

  ```
  find /path/to/search -name "filename"
  ```

- **`locate`命令：** 使用系统数据库快速查找文件，但可能需要更新数据库。

  ```
  locate filename
  ```

### 2. **列出文件**

- `ls`命令：

  列出目录中的文件和子目录。

  ```
  ls /path/to/directory
  ```

  - `ls -lt`：按时间顺序排、新到旧
  - `ls -lr`：按时间顺序排、旧到新
  - `ls -la`：显示全部（包括隐藏文件）

- `ll`命令：

   详细列出目录的文件内容

   ```
   ll /path/to/directory
   ```

### 3. **搜索文件内容**

- `grep`命令：

   在文件中搜索特定字符串或模式。

  ```
  grep "pattern" file
  ```

### 4. **统计文件**

- `wc`命令：

   计算文件中行数、字数和字符数。

  ```
  wc filename
  ```

### 5. **查看文件内容**

- **`cat`命令：** 在屏幕上显示文件内容。

  ```
  cat filename
  ```

- **`less`命令：** 分页显示文件内容，适用于大型文件。

  ```
  less filename
  ```

## 移动

- **移动文件：**

```
mv file.txt /path/to/destination/
```

这将把名为`file.txt`的文件移动到`/path/to/destination/`目录下。

- **移动并重命名文件：**

```
mv old_file.txt new_file.txt
```

这会将`old_file.txt`重命名为`new_file.txt`，如果它们在同一个目录下，也可以使用这个命令来移动文件。

- **移动文件夹（目录）：**

```
mv directory_name /path/to/destination/
```

这将把名为`directory_name`的文件夹移动到`/path/to/destination/`目录下。

## 复制

- **复制文件到指定目录：**

```
cp file.txt /path/to/destination/
```

这将把名为 `file.txt` 的文件复制到 `/path/to/destination/` 目录下。

- **复制并重命名文件：**

```
cp old_file.txt new_file.txt
```

这会复制 `old_file.txt` 并将其重命名为 `new_file.txt`。如果它们在同一个目录下，也可以使用这个命令来复制并重命名文件。

- **复制目录及其内容：**

```
cp -r source_directory /path/to/destination/
```

使用 `-r` 选项可以递归地复制目录及其所有内容到指定目标位置。

## 快捷方式

### 创建符号链接：

#### 1. 创建文件的符号链接：

```
ln -s /path/to/source_file /path/to/symlink
```

这将在指定位置创建一个指向源文件的符号链接。例如：

```
ln -s /home/user/documents/myfile.txt /home/user/desktop/myfile_link
```

这会在桌面上创建一个名为 `myfile_link` 的符号链接，指向 `/home/user/documents/myfile.txt`。

#### 2. 创建目录的符号链接：

```
ln -s /path/to/source_directory /path/to/symlink_directory
```

这会在指定位置创建一个指向源目录的符号链接。

## 删除

### 删除文件：

```
rm file.txt
```

这将永久删除名为 `file.txt` 的文件。

### 删除目录：

```
rm -r directory_name
```

使用 `-r` 选项可以递归地删除目录及其所有内容。请谨慎使用 `-r` 选项，因为它会删除目录中的所有文件和子目录。

### 删除时不提示确认：

```
rm -f file.txt
```

使用 `-f` 选项可以强制删除文件或目录而无需确认。

### 删除空目录：

```
rmdir empty_directory
```

`rmdir` 命令用于删除空目录。要注意，它只能删除空目录，如果目录中有文件或子目录，将无法删除。

### 删除多个文件：

```
rm file1.txt file2.txt file3.txt
```

你可以在一条命令中同时删除多个文件。

## 显示

### 显示文件内容：

- **`cat` 命令：** 显示文件的内容。适用于小型文件。

  ```
  cat filename
  ```

- **`more` 和 `less` 命令：** 用于分页显示文件内容。特别适用于大型文件。

  ```
  less filename
  ```

- **`head` 和 `tail` 命令：** 分别显示文件的开头和结尾部分，默认显示前/后 10 行。

  ```
  tail filename
  ```

### 显示系统信息：

- **`uname` 命令：** 显示系统信息，如内核名称、版本等。

  ```
  uname -a
  ```

- **`df` 命令：** 显示磁盘空间使用情况。

  ```
  df -h
  ```

- **`free` 命令：** 显示系统内存使用情况。

  ```
  free -m
  ```

## 进程管理

### 1. 查看进程：

- **`ps` 命令：** 显示当前运行的进程。

  ```
  ps
  ```

- **`ps aux` 命令：** 显示所有用户的所有进程，包括详细信息。

  ```
  ps aux
  ```

### 2. 启动进程：

- **后台运行进程：** 在命令后加 `&` 符号可以让命令在后台运行。

  ```
  command &
  ```

### 3. 终止进程：

- **`kill` 命令：** 终止一个进程，可以使用进程 ID（PID）。

  ```
  kill PID
  ```

- **`killall` 命令：** 终止指定名称的所有进程。

  ```
  killall process_name
  ```

### 4. 进程状态与监控：

- **`top` 命令：** 实时显示系统中各个进程的资源占用情况。

  ```
  top
  ```
  
- **`htop` 命令：** 类似于 `top`，但提供了更多的交互式选项和显示方式。

  ```
  htop
  ```
  
- **`ps` 命令结合管道与 `grep` 命令：** 过滤和查找特定进程。

  ```
  ps aux | grep process_name
  ```

## 挂载

### 挂载文件系统：

- **`mount` 命令：** 将文件系统挂载到指定的挂载点（目录）。

  ```
  mount /dev/sdXY /mnt/directory
  ```

  这里 `/dev/sdXY` 是设备名称（比如硬盘分区），`/mnt/directory` 是你想要将设备挂载到的目录。

### 查看已挂载的文件系统：

- `mount` 命令：

   不带参数直接运行 

  ```
  mount
  ```

   命令可以显示当前已挂载的文件系统列表。

  ```
  mount
  ```

### 卸载文件系统：

- `umount` 命令：

   将挂载的文件系统卸载。

  ```
  umount /mnt/directory
  ```

## 搜索grep

### 基本用法：

```
grep "pattern" filename
```

这将在指定的 `filename` 文件中搜索包含指定模式 `pattern` 的行，并将它们显示在屏幕上。

### 示例：

- **搜索包含特定字符串的行：**

  ```
  grep "word" file.txt
  ```

- **搜索时忽略大小写：**

  ```
  grep -i "word" file.txt
  ```

- **显示匹配行的行号：**

  ```
  grep -n "word" file.txt
  ```

- **显示不匹配模式的行：**

  ```
  grep -v "word" file.txt
  ```

- **递归搜索目录中的所有文件：**

  ```
  grep "pattern" -r /path/to/directory
  ```

- **使用正则表达式搜索：**

  ```
  grep "^start" file.txt
  ```

## 排序sort

### 基本用法：

```
sort filename
```

这将按照默认的字母顺序对指定的 `filename` 文件进行排序，并将结果显示在屏幕上。

### 示例：

- **按字母顺序排序：**

  ```
  sort file.txt
  ```

- **按数字大小排序：**

  ```
  sort -n file.txt
  ```

- **逆序排序：**

  ```
  sort -r file.txt
  ```

- **去除重复行并排序：**

  ```
  sort -u file.txt
  ```

- **按特定列排序（以空格或制表符分隔的列）：**

  ```
  sort -k2 file.txt
  ```

- **将排序结果保存到新文件：**

  ```
  sort file.txt > sorted_file.txt
  ```

## sleep

### 基本用法：

```
sleep <number>[s|m|h]
```

- `` 是指暂停的时间长度，以秒为单位，默认为秒。
- `[s|m|h]` 是可选的时间单位，分别表示秒、分钟和小时。

### 示例：

- **暂停5秒钟：**

  ```
  sleep 5
  ```

- **暂停1分钟：**

  ```
  sleep 1m
  ```

- **暂停2小时：**

  ```
  sleep 2h
  ```

## jobs

### 基本用法：

```
jobs
```

这将列出当前 Shell 中运行的作业及其状态。

### 示例：

- **运行一个命令并将其放到后台执行：**

  ```
  command &
  ```

- **列出当前作业：**

  ```
  jobs
  ```

- **查pid：**

  ```
  jobs -l
  ```

## 别名alias

### 创建别名：

```
bashCopy code
alias short_name='long_command'
```

这里 `short_name` 是你想要创建的别名，`long_command` 是你想要关联的实际命令。

### 示例：

- **创建别名：**

  ```
  alias ll='ls -l'
  ```

  这将创建一个 `ll` 的别名，将其关联到 `ls -l` 命令，这样你可以通过输入 `ll` 来执行 `ls -l`。

- **显示当前所有别名：**

  ```
  alias
  ```

- **删除别名：**

  ```
  unalias short_name
  ```

  例如，要删除先前创建的 `ll` 别名，可以使用 `unalias ll`。

这些别名可以在命令行会话期间使用，但在关闭当前会话后，它们将被删除。如果你希望永久保存别名，可以将其添加到 Shell 的配置文件（如 `~/.bashrc` 或 `~/.bash_profile`）中。

### zshrc

`zshrc` 是 Zsh Shell 的配置文件，它类似于 Bash Shell 中的 `.bashrc` 文件，用于配置 Zsh Shell 的行为、环境变量以及加载各种设置和别名。

你可以使用文本编辑器打开 `~/.zshrc` 文件来编辑 Zsh Shell 的配置：

```
vim ~/.zshrc
```

在这个文件中，你可以添加和修改各种设置，例如：

- **添加别名：** 使用 `alias` 命令来创建命令的别名，使得你可以更快地执行常用命令。
- **配置环境变量：** 设置 `PATH` 或其他环境变量，使得系统能够找到特定的可执行文件或资源。
- **加载插件和主题：** 使用 Zsh 的插件管理器加载插件，并选择合适的主题。

在修改 `~/.zshrc` 文件后，可以通过重新加载配置文件或重新启动终端来使更改生效：

```
source ~/.zshrc
```

Zsh 是一个强大的 Shell，通过编辑 `~/.zshrc` 文件，你可以个性化定制 Shell 的行为和外观，以适应你的需求和偏好。

## 环境变量

### 查看环境变量：

- **查看所有环境变量：**

  ```
  printenv
  ```
  
- **查看特定环境变量的值：**

  ```
  echo $VARIABLE_NAME
  ```

### 设置环境变量：

- **临时设置环境变量：**

  ```
  VARIABLE_NAME=value
  ```
  
  这种方式设置的环境变量只在当前 Shell 会话中有效，关闭会话后将失效。
  
- **永久设置环境变量：**

  - **对于单个用户：** 在用户的配置文件（如 `~/.bashrc`, `~/.bash_profile`, `~/.zshrc` 等）中添加：

    ```
    export VARIABLE_NAME=value
    ```
    
  - **对于所有用户：** 在系统级别的配置文件中添加：
  
    在 `/etc/environment` 文件中添加：
  
    ```
    VARIABLE_NAME=value
    ```

### 常见环境变量：

- **PATH：** 定义系统在哪些目录中查找可执行文件。
- **HOME：** 指向当前用户的主目录。
- **USER：** 当前用户名。
- **SHELL：** 当前使用的 Shell。

### 示例：

- **设置自定义环境变量：**

  ```
  export MY_VARIABLE="Hello, World!"
  ```

- **将新的路径添加到 PATH 中：**

  ```
  export PATH="$PATH:/new/path"
  ```

- **删除环境变量：**

   ```
   unset MY_VARIABLE
   ```

(非系统变量不建议大写)

## 安装软件

### Ubuntu、Debian 等使用 APT 的发行版：

- 安装软件：

  ```
  sudo apt-get install package_name
  ```

## 换源

### 更改源的步骤：

- **备份源文件（可选）：**

在修改前，建议先备份当前的源文件，以便出现问题时可以恢复：

```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
```

- **编辑源文件：**

使用文本编辑器打开 `/etc/apt/sources.list` 文件：

```
sudo nano /etc/apt/sources.list
```

- **更改软件源：**

在编辑器中，将现有的源替换为你想要使用的新源。你可以在 Ubuntu 的官方网站或者专门的镜像站点上找到适合你的源地址。

示例（Ubuntu 20.04）：

```
# 默认源
deb http://archive.ubuntu.com/ubuntu/ focal main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ focal-updates main restricted universe multiverse
deb http://archive.ubuntu.com/ubuntu/ focal-backports main restricted universe multiverse
deb http://security.ubuntu.com/ubuntu/ focal-security main restricted universe multiverse

# 更换为清华大学镜像源（示例，可以根据实际情况替换）
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ focal-security main restricted universe multiverse
```

- **保存更改并退出编辑器：**

使用 `Ctrl + X`，按下 `Y` 确认保存，然后按下 `Enter`。

- **更新软件包列表：**

在更改源后，要确保使用新的源更新软件包列表：

```
sudo apt-get update
```

这样就完成了 APT 软件源的更改。记住，更改源是一项敏感操作，请确保你在更改前了解你所做的更改，并且使用可靠的源。

## 更新

### 使用 `apt-get` 命令更新：

1. **更新软件包列表：**

   ```
   sudo apt-get update
   ```

   这个命令会从软件源获取最新的软件包列表信息，但不会安装新的软件包。

2. **升级已安装的软件包：**

   ```
   sudo apt-get upgrade
   ```

   这个命令会安装所有可用更新的软件包。它会提示是否要安装，输入 `Y` 确认开始更新。

### 使用 `apt` 命令更新（推荐）：

- **更新软件包列表：**

  ```
  sudo apt update
  ```

   这个命令会更新本地的软件包列表，确保你获取到最新的软件包信息。

- **升级已安装的软件包：**

  ```
  sudo apt upgrade
  ```

   这个命令会安装可用的软件包更新。它会列出将要更新的软件包，并询问是否要进行更新。按 Y 确认开始更新。

### 注意事项：

- `apt` 是 `apt-get` 的一个更现代化的替代品，提供了更友好的界面和更多的功能。在新版本的 Ubuntu 或者 Debian 中，`apt` 命令被推荐使用。
- 在执行任何更新之前，建议先备份重要的数据以及系统设置。
- 这些命令需要管理员权限（使用 `sudo`）才能执行。

## 卸载

### 使用 APT（终端方式）：

使用 `apt-get` 命令可以卸载软件包。

```
sudo apt-get remove package_name
```

- `remove` 命令用于卸载指定的软件包。
- `package_name` 是你要卸载的软件包的名称。

如果你想同时删除软件包及其配置文件，可以使用 `purge` 命令：

```
sudo apt-get purge package_name
```

## 组group

### 在 Linux 中查看组的信息：

#### 1. **查看当前用户所属的组：**

可以使用 `groups` 命令查看当前用户属于哪些组：

```
groups
```

这将显示当前用户所属的所有组。

#### 2. **查看特定用户所属的组：**

```
groups username
```

将 `username` 替换为你想要查看的特定用户的用户名，这将列出该用户所属的所有组。

#### 3. **查看所有组的列表：**

你可以查看系统中所有组的列表，通常保存在 `/etc/group` 文件中。你可以使用 `cat` 命令或者 `less` 命令查看该文件：

```
cat /etc/group
```

这将显示所有组的详细信息，包括组名、组 ID、组成员等。

## 文件和文件夹

### 基本用法：

```
chmod permissions file_or_directory
```

### 权限表示方式：

- **符号表示法：**
  - **`u`（用户）、`g`（组）、`o`（其他）、`a`（所有）**
  - **`+`（添加权限）、`-`（移除权限）、`=`（设置权限）**
  - **`r`（读取权限）、`w`（写入权限）、`x`（执行权限）**
- **数字表示法：**
  - **读（`4`）、写（`2`）、执行（`1`）**
  - 分别用数字表示权限，然后将它们相加：
    - **读取权限：`4`**
    - **写入权限：`2`**
    - **执行权限：`1`**

### 示例：

- **使用符号表示法设置文件的权限：**

  ```
  chmod u+rwx file.txt     # 为文件所有者添加读、写、执行权限
  chmod go-w file.txt      # 移除组和其他用户的写权限
  chmod a=r file.txt       # 将所有用户的权限设置为只读
  ```

- **使用数字表示法设置文件的权限：**

  ```
  chmod 644 file.txt       # 将文件权限设置为 -rw-r--r--
  chmod 755 directory      # 将目录权限设置为 rwxr-xr-x
  ```