---
title: Nginx通过反向代理的方法去访问Gitlab
date: 2018-04-28 09:59:56
tags: 
    - Gitlab 
    - nginx 
    - 服务器
categories: 
    - 技巧总结
---
### 背景:  
最近用为企业部署了**Gitlab**用来存放代码用。  
**Gitlab**是一个企业能够很方便的管理代码的工具，官方提供的安装配置也很方便的部署**Gitlab** 到企业的服务器
**Gitlab**本身自带了**Nginx**，并且如果不改动任何配置的话，**Gitlab**默认会部署到**80**端口。
由于企业在这个服务器有很多其他的web服务，要做到**Gitlab**和其他的web服务共存，那么需要做一点点修改
### 方案:  
- 方法一：干掉**Gitlab**的内置**Nginx**，然后在外部安装nginx服务器，并且代理了**Nginx**的socket端口。  
- 方法二：在外部的**Nginx**服务器做代理转发，这个方法不需要禁用**Gitlab**本身的**Nginx**服务，只需要在外部的**Nginx**做相关的改动即可。  
（由于不想改动太多，以及涉及到集成化的原因，就不打算动**Gitlab**内部的**Nginx**，方案二也就是本文介绍的方法，就是用我们外部的**Nginx**去代理转发请求到**Gitlab**的服务端口)  

### 实现(方案二): 
1. 修改**Gitlab**的配置文件： 
   - 默认安装的**Gitlab**，配置文件在 :`` /etc/gitlab/gitlab.rb``  
   - 修改**Gitlab**的监听端口号:
``nginx['listen_port'] = 端口号``    
   - 修改**Gitlab**的域名：  
``external_url '域名'``  
（注意，此处的域名不需要带上端口号，因为请求是被我们前级的Nginx处理的，然后把请求转发过来这个端口，对外访问而言，还是我们的前级Nginx的80端口)  
   - 最后执行  ``gitlab-ctrl reconfigure`` 让配置生效  
2. 配置**Nginx**服务器的站点配置文件 ```default.conf```：  
{% codeblock %}
upstream  git{
    # 域名对应 gitlab配置中的 external_url
    # 端口对应 gitlab 配置中的 nginx['listen_port']
    server  域名:端口;
}
server{
    listen 80;
    # 此域名是提供给最终用户的访问地址
    server_name 域名;

    location / {
        # 这个大小的设置非常重要，如果 git 版本库里面有大文件，设置的太小，文件push 会失败，根据情况调整
        client_max_body_size 50m;
        proxy_redirect off;
        #以下确保 gitlab中项目的 url 是域名而不是 http://git，不可缺少
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        # 反向代理到 gitlab 内置的 nginx
        proxy_pass http://git;
        index index.html index.htm;
    }
} 
{% endcodeblock %}
