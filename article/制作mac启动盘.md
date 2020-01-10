### 准备
1.1 下载`Install macOS Sierra.dmp`镜像程序
1.2 打开`Install macOS Sierra.dmp`， 会有一个`Install macOS Sierra.app`程序，安装它
1.3 准备一个大于8G的U盘
1.4 抹掉该U盘，并将其设置为`Mac OS 日志式`，名称为`InstallOS`

### 执行命令
```bash shell
sudo /Applications/Install\ macOS\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/InstallOS --applicationpath /Applications/Install\ macOS\ Sierra.app —nointeraction

# 其中，第一部分红色的是镜像的名称(Install\ macOS\ Sierra.app)；第二部分红色的是U盘的名称(InstallOS)
```

### 完成（以下为自动命令，无需操作）
```bash shell
Erasing Disk: 0%… 10%… 20%… 30%…100%…
Copying installer files to disk…
Copy complete.
Making disk bootable…
Copying boot files…
Copy complete.
Done.
```

### 启动
重启电脑，按住option键，即可切换u盘启动
