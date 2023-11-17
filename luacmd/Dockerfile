# 使用 alpha 基础镜像  
FROM alpine:3.18
  
# 安装必要的工具和库    
RUN apk update && \  
    apk add wget gcc g++ make openssl-dev pcre-dev zlib-dev && \  
    rm -rf /var/cache/apk/*
  
# 下载、解压并安装 LuaJIT  
RUN wget http://luajit.org/download/LuaJIT-2.0.5.tar.gz && \  
    tar -zvxf LuaJIT-2.0.5.tar.gz && \  
    cd LuaJIT-2.0.5/ && \  
    make install PREFIX=/usr/local/LuaJIT && \  
    rm -rf LuaJIT-2.0.5.tar.gz  
  
# 复制共享库到正确位置  
RUN cp /usr/local/LuaJIT/lib/libluajit-5.1.so.2 /usr/lib/  

# 添加环境变量  
ENV LUAJIT_LIB /usr/local/LuaJIT/lib  
ENV LUAJIT_INC /usr/local/LuaJIT/include/luajit-2.0  
  
# 下载、解压 ngx_devel_kit 和 lua-nginx-module  
RUN cd / && \  
    wget https://github.com/simplresty/ngx_devel_kit/archive/v0.3.0.tar.gz && \  
    wget https://github.com/openresty/lua-nginx-module/archive/v0.10.13.tar.gz && \  
    tar -xzvf v0.3.0.tar.gz && \  
    tar -xzvf v0.10.13.tar.gz && \  
    rm -rf v0.3.0.tar.gz v0.10.13.tar.gz  
  
# 下载、解压并编译安装 nginx    
RUN wget http://nginx.org/download/nginx-1.21.4.tar.gz && \    
    tar -zxvf nginx-1.21.4.tar.gz && \    
    cd nginx-1.21.4 && \    
    ./configure --prefix=/usr/local/luanginx --sbin-path=/usr/sbin/nginx \    
                --with-http_realip_module --add-module=/ngx_devel_kit-0.3.0 \    
                --add-module=/lua-nginx-module-0.10.13 --with-cc-opt='-Wno-error' && \    
    make && make install && \    
    rm -rf nginx-1.21.4 nginx-1.21.4.tar.gz
    
# 清理不必要的文件，减小镜像体积  
RUN rm -rf /root/.cache/*

# 启动 Nginx  
CMD ["nginx", "-g", "daemon off;"]
