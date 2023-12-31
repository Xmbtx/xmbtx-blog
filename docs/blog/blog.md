# re:从零开始的博客基础

## 图片折叠

- 博客里出现所有像下图一样的蓝框，大概率都是图片，可以点开看的

  ![picture](./picture.png)
  
- 上面那玩意是没折叠的图片嗷，点不开的（我更新完博客特么点半天没点开还以为自己写错了）（其实你调成白色背景就能看出来了）

- 因为图片折叠后可以节省很多空间所以下面图片大多是折叠形式出现

- 在博客的文章里如果想加折叠图片，参考我[markdown学习记录](https://xmbtx.github.io/xmbtx-blog/mkd/mkd/) ~~但是我不知道是我的虚拟机的问题还是什么问题，如果我想在blog的docs文件夹里的文件夹里的文件中添加图片，我需要在添加图片的那个文件的文件夹中创建一个与这个文件夹同名的文件夹，把图片放在同名文件夹里才行（看不懂正常，我觉得我是特例）~~

## 博客上传更新

- 你的blog文件夹中，会有一个叫auto-update-this-repo.sh的文件

- 在blog文件夹内右键，点击open in terminal，输入bash a后，按tab键补全后回车

  <details>
      <summary>terminal</summary>
      <p>
          <img src="terminal.png"/>
      </p>
  </details>

- (要是不会用tab可以直接输入bash auto-update-this-repo.sh后回车)

- 效果图如下

  <details>
      <summary>上传</summary>
      <p>
          <img src="upload.png"/>
      </p>
  </details>

## 新建、打开文件

- 在文件夹内右键，点击open in terminal，输入touch 你想起的文件名.md

- touch后面要加空格，.md是文件类型

  <details>
      <summary>新建文件</summary>
      <p>
          <img src="touch.png"/>
      </p>
  </details>

- 创建完文件后，右键文件选择![open with](./application.png)，点击下面![view all](./view all.png)，从里面找到![typora](./typora.png)点击，点击后点击右上角Selsct就行（就能用typora打开md文件了)

## 目录列表主页附录

- 如果在博客里写目录的话，类似下图这样![list1](./list1.png)

- 注意，在 - 的后面要写空格，: 后面也要写空格

- zhr补充：缩进必须是空格，不能是tab，且每级缩进为2空格（甚至你在.md后面多打一个tab都会报错）

- 你需要在你的blog文件夹的mkdocs.yml文件的nav部分做一些修改，如下图

  <details>
      <summary>目录</summary>
      <p>
          <img src="list2.png"/>
      </p>
  </details>

### 文件、文件夹格式

- 怎么说捏，blog的docs文件夹里直接创建的.md文件直接写就行比如那个index.md
- 如果是docs文件夹里的文件，直接写什么什么.md就行
- 如果在docs文件夹里的文件夹里的文件，你需要写 文件夹名/文件名.md （有时候多个文件夹嵌套写法不变，多写几个/就行）

### 附录、列表格式

- 不想说了，不会描述，看上面图吧，跟你电脑里文件夹一个意思

## 博客底部右下角图标链接、图标引用

- 在[这个链接](https://github.com/FortAwesome/Font-Awesome/tree/6.x/svgs/brands)里面，往左边看，有很多类似![blbl](./blbl.png)的svg文件，点击后就可以查看图片效果，你可以根据文件名大概估计一下图片是啥样的

- 这个东西在哪更改捏，点开你的mkdocs.yml文件（跟上文是同一个mkdocs）,找到extra:那，里面有个social:，在social下面如

  ![icon](./icon.png)

- 这样的东西，格式建议照抄，因为我不会描述

- 大概说一下这仨单词的用法：icon后面统一写fontawesome/brands/啥啥啥，最后那个/后面填的东西就是你想显示的图标，填的内容就是上面.svg前面的内容，例如github.svg就填github就行

- link后面放的是你想跳转过去的链接

- name就是把光标移到博客里的图标上，悬浮显示出来的字

## 插件使用

- 还是在mkdocs.yml里面，theme: features对应的地方，可以自己加插件（本人还没自己加过啥东西，下次再说）

## 报错修复（未实测，不建议使用）

- 你可能会遇到：key失效，不能上传（几率极低）；没有联网，上传失败（几率低）；有网但是传不上去，当你用手机开的热点上传时又能成功上传（几率低）

### key失效

- 打开initializer文件夹，打开7-ssh-github.sh这个文件，确认里面邮箱和用户名无误后，在文件夹空白处右键打开terminal，输入bash 7-ssh......，一路回车（如果要输入y/n都输入y，不要搁这也回车了），最后会弹出一个文件，复制文件全部内容，打开你的github，点击右上角头像选择设置，点击界面右侧 `ssh and gpg keys` ，先删除旧的key，再点击右上 `new ssh keys` ，把刚才复制的粘贴进去，保存就行

### 没联网

- 看我博客主页10.17网络修复就行，大概率解决联网问题

### 开热点解决

- 忘了是咋回事了，下次碰见再说

### insufficient permission for adding an object to repository database .(少权限上传)

- 大概一句sudo chmod -R 777 ./就能解决，我也不知道是干啥了导致的
- [原文链接](https://blog.csdn.net/cheng1a/article/details/126101129)
