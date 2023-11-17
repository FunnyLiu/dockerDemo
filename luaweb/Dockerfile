FROM luanginx:0.0.1
COPY dist/ /usr/local/luanginx/html
COPY nginx.conf /usr/local/luanginx/conf/nginx.conf
EXPOSE 80
# 启动 Nginx  
CMD ["nginx", "-g", "daemon off;"]