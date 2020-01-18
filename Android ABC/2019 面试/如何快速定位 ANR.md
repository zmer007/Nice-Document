出现 ANR 时，
首先查看 logcat 日志，定位出现 ANR 文件，查看 ANR reason，如果 reason 定位不到问题，就使用
```sh
# 旧版本
$ adb pull data/anr/trace.txt

# 新版本使用，解压 your-custom-file 后，进行 FS/data/anr/anr_* 此处 anr_* 就是 anr 详细日志
$ adb bugreporter your-custom-file # 可以不写 your-custom-file，不写会输出默认文件名 bugreport-*
```
获取 ANR 文件，搜索 main