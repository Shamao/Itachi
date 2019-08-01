#### 发送邮箱

- 选择发送地址(例如：qq)， 开启mail服务器， 获取授权码（相当于密码）
- 设置发送邮箱、收件邮箱、抄送邮箱 等信息
- 发送邮件 





```go
	m := gomail.NewMessage()
	m.SetHeader("From", "xxx@qq.com")
	m.SetHeader("To", "xxx@163.com")
	m.SetAddressHeader("Cc", "休闲鞋@126.com", "xxx")
	m.SetHeader("Subject", "Hello!")
	m.SetBody("text/html", "Hello <b>Bob</b> and <i>Cora</i>!")
	m.Attach("./x.txt") // 该路径为错误的 使用绝对地址 其实还不会
	d := gomail.NewDialer("smtp.qq.com", 587, "xxx@qq.com", "")

	// Send the email to Bob, Cora and Dan.
	if err := d.DialAndSend(m); err != nil {
		panic(err)
	}

	fmt.Println("发送成功")
```

