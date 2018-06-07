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
