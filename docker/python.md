### docker部署python项目

#### 项目目录示例如下
```
docker-compose.yml
app
    Dockerfile
    main.py
```

#### docker-file详解
```
FROM python:3.9.0 as build_python
RUN pip install Flask
WORKDIR /app
COPY ./app /app
ENV FLASK_APP=/app/main.py //设置环境变量
EXPOSE 8094
CMD ["flask","run","--host=0.0.0.0","-p 8094"] //flask run --host=0.0.0.0 -p 8094
```

