# linux是怎样工作的

## 进程发起的系统调用

- 通过 ` strace ` 命令对进程进行追踪
- ` strace -o hello.log ./hello`
- 打开 ` hello.log ` 文件

## 进程ID

- 在输入运行命令时附加一个 ` & ` ，即可获取程序进程ID
- ` ./ppidloop &`
- ` [1] 13389 `

## 结束进程

- ` kill *** `
- ***就是你进程ID，比如上面的13389

## CPU核心在运行什么

- ` sar -P ALL 1 `
- ` 1 ` 即为每秒采集一次数据
- ` Ctrl+C ` 结束运行，输出平均值
- 可通过第四个参数来指定采集信息次数
- ` sar -P ALL 1 2 `
- 最后那个 ` 2 ` 就是采集次数

## 执行系统调用所需时间

- 在 ` strace ` 命令后面加上 ` -T ` 选项

- ``` 
  strace -T -o hello.log ./hello
  cat hello.log
  ```

## OS提供的程序

- 初始化系统： ` init `
- 变更系统的运行方式： ` sysctl、nice、sync `
- 文件操作： ` touch、mkdir `
- 文本数据处理： ` grep、sort、uniq `
- 性能测试： ` sar、iostat `
- 编译： ` gcc `
- 脚本语言运行环境： ` perl、python、ruby `
- shell： ` bash `
- 视窗系统： ` X `

