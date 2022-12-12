



ps aux|grep **** 查看进程

kill -9 pid 杀进程

netstat -tunlp | grep **** 查看端口号

rm -rf  *** 删文件



sh startup.sh -m standalone &  启动nacos





jps -l 查看jar 进程

nohup java -jar service_edu-0.0.1-SNAPSHOT.jar >spring.log & 输出日志到spring文件

cat nohup.out 查看 日志 (去上面文件里看就好了)



**pm2 list** 查看pm进程

pm2 start npm --name "xxx" -- run start 运行nuxt ,名字是json里的name

pm2 stop all 关闭所有

pm2 restart all 重启所有



查看npm当前版本

```
npm -v

```

如果不是最新版本，运行指令

```
npm install -g npm

```

如果想更新到指定版本，运行指令

```
npm -g install npm@6.4.1
```



