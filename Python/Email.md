# 发送Email

```python
import smtplib
from email.mime.text import MIMEText
from email.header import Header

sender = ''
receivers = ['']

mail_host=""  #设置服务器
mail_user=""    #用户名
mail_pass=""   #口令


message = MIMEText(recode, 'html', 'utf-8')
message['From'] = Header("", 'utf-8')
message['To'] = Header("", 'utf-8')

subject = ''
message['Subject'] = Header(subject, 'utf-8')

try:
    smtpObj = smtplib.SMTP()
    smtpObj.set_debuglevel(1)
    smtpObj.connect(mail_host, 25)  # 25 为 SMTP 端口号
    smtpObj.login(mail_user, mail_pass)
    smtpObj.sendmail(sender, receivers, message.as_string())
    print "邮件发送成功"
except smtplib.SMTPException:
    print "Error: 无法发送邮件"
```


