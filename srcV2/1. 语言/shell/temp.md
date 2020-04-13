```bash
# 重命名
ls *.png | awk '{print "mv \""  $0   "\" aml_" $2}'|bash
# 缩放
ls *.png | awk '{print "sips -Z 300 " $0}'|bash
# 图片压缩

```

