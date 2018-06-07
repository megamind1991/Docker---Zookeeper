# Docker---Zookeeper


## yml文件内容

    version: "3"
    services:
      zookeeper:
          image: zookeeper
          restart: always
          container_name: zookeeper
          deploy:
            replicas: 3
            restart_policy:
              condition: on-failure 
          ports:
              - "2181:2181"
          environment:
              ZOO_MY_ID: 1
              ZOO_SERVERS: server.1=zoo1:2888:3888 server.2=zoo2:2888:3888 server.3=zoo3:2888:3888  
          networks:
            - hbasesoft-network            
    networks:
      hbasesoft-network:


*docker stack deploy -c /home/liujie/zookeeper/zookeeper.yml zookeeper-cluster*

## stack版的yml 根据docker hub上的zookeeper的内容

    version: "3"
    services:
      zoo1:
        image: zookeeper
        restart: always
        hostname: zoo1
        ports:
          - 2181:2181
        environment:
          ZOO_MY_ID: 1
          ZOO_SERVERS: server.1=0.0.0.0:2888:3888 server.2=zoo2:2888:3888 server.3=zoo3:2888:3888
        networks:
           - outside  
        deploy:
          placement:
            constraints: [node.labels.env == redis]       
      zoo2:
        image: zookeeper
        restart: always
        hostname: zoo2
        ports:
          - 2182:2181
        environment:
          ZOO_MY_ID: 2
          ZOO_SERVERS: server.1=zoo1:2888:3888 server.2=0.0.0.0:2888:3888 server.3=zoo3:2888:3888
        networks:
          - outside
        deploy:
          placement:
            constraints: [node.labels.env == redis]            
      zoo3:
        image: zookeeper
        restart: always
        hostname: zoo3
        ports:
          - 2183:2181
        environment:
          ZOO_MY_ID: 3
          ZOO_SERVERS: server.1=zoo1:2888:3888 server.2=zoo2:2888:3888 server.3=0.0.0.0:2888:3888 
        networks:
           - outside
        deploy:
          placement:
            constraints: [node.labels.env == redis]             
    networks:
      outside:
        external:
          name: hbasesoft-network




docker stack ps getstartedlab

docker stack rm getstartedlab

docker node ls

docker swarm leave --force
