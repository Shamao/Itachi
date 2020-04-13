1.官方解释

Git tag 给当前分支打标签

其实道理和commit 的commit-sha1有些相似，其实就是给当前的版本做个标记，以便回退到此版本。如果使用commit-sha1,大家都记不住那条冗长的sha1码，所以用tag标签来做记录

发布一个版本时，我们通常先在版本库中打一个标签（tag)

3.

1) git tag <name>就可以打一个新标签

> $ git tag testtag

标签都只存储在本地，不会自动推送到远程。
2) 可以用命令git tag查看所有标签：

> $ git tag testtag

3)在历史提交上打 tag
查看commit id，使用commit id打tag

>  $ git tag v0.9 6df3aa614a45e687697a628a9174106822f79bfc

注意，标签不是按时间顺序列出，而是按字母排序的。可以用git show <tagname>查看标签信息：

> $ git show testtag

4)带有说明的标签，用-a指定标签名，-m指定说明文字：

> $ git tag -a v0.1 -m "version 1.0" 88d434eb9992fa7502ec246ecfb89aef19f16873

5)删除标签

> $ git tag -d testtag

6) 推送某个标签到远程，使用命令git push origin <tagname>：

> $ git push origin testtag 

7) 一次性推送全部尚未推送到远程的本地标签

> $ git push origin --tags