# re:从零开始的博客基础

## 图片折叠

- 博客里出现所有像下图一样的蓝框，大概率都是图片，可以点开看的

  ![picture](./picture.png)
  
- 上面那玩意是没折叠的图片嗷，点不开的（我更新完博客特么点半天没点开还以为自己写错了）（其实你调成白色背景就能看出来了）

- 因为图片折叠后可以节省很多空间所有下面图片大多是折叠形式出现

- 在博客的文章里如果想加图片，参考我markdown学习记录，但是我不知道是我的虚拟机的问题还是什么问题，如果我想在blog的docs文件夹里的文件夹里的文件中添加图片，我需要在添加图片的那个文件的文件夹中创建一个与这个文件夹同名的文件夹，把图片放在同名文件夹里才行（看不懂正常，我觉得我是特例）

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

- 你需要在你的blog文件夹的mkdocs.yml文件的nav部分做一些修改，如下图

  <details>
      <summary>目录</summary>
      <p>
          <img src="list2.png"/>
      </p>
  </details>

### 文件、文件夹格式

- 怎么说捏，bolg的docs文件夹里直接创建的.md文件直接写就行比如那个index.md
- 如果是docs文件夹里的文件，直接写什么什么.md就行
- 如果在docs文件夹里的文件夹里的文件，你需要写 文件夹名/文件名.md （有时候多个文件夹嵌套写法不变，多写几个/就行）

### 附录，列表格式

- 不想说了，不会描述，看上面图吧

## 博客底部右下角图标链接、图标引用

- 在[这个链接](https://github.com/FortAwesome/Font-Awesome/tree/6.x/svgs/brands)里面，往左边看，有很多类似![blbl](./blbl.png)的svg文件，点击后就可以查看图片效果，你可以根据文件名大概估计一下图片是啥样的
- 这个东西在哪更改捏，点开你的mkdocs.yml文件（跟上文是同一个mkdocs）,找到extra:那，里面有个social:，在social下面如
