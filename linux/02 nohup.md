
## nohup

当我们用ssh登录远程服务器跑一个时间较长的代码，即使我们使用后台模式`&`，一旦我们退出ssh会话，这个运行的代码也会被杀死；为了避免子进程被杀死，我们使用nohup命令，这样一来，当我们从ssh退出后，后台代码还能继续跑。语法为：

```shell
nohup command &
```
默认情况下，nohup会产生一个`nohup.out`的文件，并将标准输出和标准错误重定向到这个文件，如果我们不期望这样的模式，可以手动添加重定向的方式，一个典型的使用方式是将输出全部重定向到`/dev/null`:
```shell
nohup command  &>/dev/null &
```

**参考**
- [nohup作用](https://askubuntu.com/questions/995179/what-is-the-function-of-the-nohup-command)
> Let us consider you have opened a gedit text editor from a terminal and working on it. If you close the terminal before closing gedit, the gedit also gets closed as soon as closing the terminal. So what is going on here? The gedit runs as a child process under the terminal. When you close the terminal a hang up signal (SIGHUP) is sent to the process which kills the child process.

On the other hand if you want your child process (here gedit) to keep on running even after closing the parent terminal, you would want your process immune to hangup signal. So that closing the terminal do not close the child process. nohup does exactly this job.
- [nohup重定向详解](https://stackoverflow.com/questions/10408816/how-do-i-use-the-nohup-command-without-getting-nohup-out)

