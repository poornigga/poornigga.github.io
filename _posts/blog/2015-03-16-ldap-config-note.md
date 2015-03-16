---
layout: post
title: LDAP 配置备忘
description: 配置server接入其他LDAP服务器备忘
category: blog
---

公司里面有很多系统，不可能每个系统自己维护一套账号，比较合理的解决方案是LDAP. 
微软的ActiveDictionary 是否成功，需要用ldapsearch命令来验证，确保ok之后，写入配置文件.

### 如果系统中没有，需要安装下，

>  ubuntu ： apt-get install ldap-utils
>  centos :  yum install openldap openldap-clients

### 使用如下

>  ldapsearch -x "sAMAccountName" -W -D "cn=310xxxx,ou=Users,ou=CODE,dc=sample,dc=com" \ 
>  -b "ou=CODE,dc=sample,dc=com" -h code.sample.com

<ul>
<li>-x 验证；-W 输入密码 ； cn  用户名；cn；用户组；ou 组织单元；dc 域 ； </li>
<li>-D binddn, 这个需要从LDAP管理员那里获得 </li>
<li>-b search base,也要从LDAP管理员那里获取 </li>
<li>-x 后面接用户名，sAMAccountName不能修改( Use simple authentication instead of SASL).</li>
</ul>

gitlab中LADP配置:

```
  # These settings are documented in more detail at
  # https://gitlab.com/gitlab-org/gitlab-ce/blob/a0a826ebdcb783c660dd40d8cb217db28a9d4998/config/gitlab.yml.example#L136
  gitlab_rails['ldap_enabled'] = true
  gitlab_rails['ldap_servers'] = YAML.load <<-EOS # remember to close this block with 'EOS' below
main: # 'main' is the GitLab 'provider ID' of this LDAP server
  label: 'LDAP'
  host: '_your_ldap_server'
  port: 636
  # uid: 'sAMAccountName' 
  uid: 'userPrincipalName'
  method: 'plain' # "tls" or "ssl" or "plain" plain-636, 
  bind_dn: '_the_full_dn_of_the_user_you_will_bind_with'
  password: '_the_password_of_the_bind_user'
  active_directory: true
  allow_username_or_email_login: false ## if active_dir = true && uid = userPrincipalName 
  filter: ''

```

gitlab中配置postfix 发邮件选项[postfix][1]


[1]: http://1585205.blog.51cto.com/1575205/1153091 "postfix"

---
