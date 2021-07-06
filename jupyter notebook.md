## 下载主题和插件

```
pip install jupyterthemes==1.18.2
pip install jupyter_contrib_nbextensions
# pip install jypyter_nbextensions_configurator
jupyter nbextensions_configurator enable --user
jupyter contrib nbextension install --user --skip-running-check
# 更新了核心
pip install --upgrade jupyter_client
```

## jt 指令的一些操作

[github文档](https://github.com/dunovank/jupyter-themes)

1. jt -t	 更换主题

2. jt -l 	主题列表

3. jt -r 	还原默认设置

4. jt        加其他参数

`jt -t onedork -fs 95 -altp -tfs 11 -nfs 115 -cellw 88% -T`

