```bash
# 需要执行的话 在后面加上 |bash
# 重命名
ls *.png | awk '{
print "mv \""  $0   "\" aml_" $2
mv  "$0 " aml_" $2
}'
ls *.jpg | awk '{print "mv \""  $0   "\" aml_" $2}'
# 缩放
ls *.png | awk '{print "sips -Z 300 " $0}'
# 图片压缩


# 格式转化
ls *.png | awk -F "." '{print "sips  -s format jpeg -s formatOptions default " $0  " --out " $1 ".jpg"} '

```

