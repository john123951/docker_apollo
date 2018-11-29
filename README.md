# docker_apollo
Docker Image for Apollo (https://github.com/ctripcorp/apollo)

# SQL
```
UPDATE ApolloConfigDB.ServerConfig SET Value='http://192.168.1.12:6080/eureka/' WHERE `Key` = 'eureka.service.url';
```

# Run Config Service
```
docker run -d --name apollo-configservice \
  -v /tmp/logs:/opt/logs \
  -e spring_datasource_url="jdbc:mysql://192.168.1.10:3306/ApolloConfigDB?useUnicode=true&characterEncoding=utf-8&autoReconnect=true" \
  -e spring_datasource_username=root \
  -e spring_datasource_password=123456 \
  -p 6080:8080 \
  apollo-configservice
```

# Run Admin Service
```
docker run -d --name apollo-adminservice \
  -v /tmp/logs:/opt/logs \
  --link apollo-configservice:configservice \
  -e hostname=configservice \
  -e spring_datasource_url="jdbc:mysql://192.168.1.10:3306/ApolloConfigDB?useUnicode=true&characterEncoding=utf-8&autoReconnect=true" \
  -e spring_datasource_username=root \
  -e spring_datasource_password=123456 \
  apollo-adminservice
```

# Run Portal
```
docker run -d --name apollo-portal \
  -v /tmp/logs:/opt/logs \
  -e DEV_META="http://192.168.1.12:6080" \
  -e spring_datasource_url="jdbc:mysql://192.168.1.10:3306/ApolloPortalDB?useUnicode=true&characterEncoding=utf-8&autoReconnect=true" \
  -e spring_datasource_username=root \
  -e spring_datasource_password=123456 \
  -p 8070:8070 \
  apollo-portal
```
