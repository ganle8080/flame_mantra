```shell

docker load -i nsq_v1.3.0.tar

docker run --name lookupd \
-p 4160:4160 -p 4161:4161 \
-d nsqio/nsq:v1.3.0 \
/nsqlookupd

docker run --name nsqd \
-p 4150:4150 -p 4151:4151 \
-d nsqio/nsq:v1.3.0 /nsqd \
--broadcast-address=127.0.0.1 \
--lookupd-tcp-address=127.0.0.1:4160 \
--data-path=/data \
--mem-queue-size=0 \
--max-msg-timeout=5s


docker run -d --name nsqadmin \
-p 4171:4171 nsqio/nsq:v1.3.0 /nsqadmin 
--lookupd-http-address=127.0.0.1:4161 \

```


1. 首先启动 nsqlookupd
# 在一个终端窗口中
nsqlookupd
# 默认监听端口：4160（TCP），4161（HTTP）
2. 然后启动 nsqd
# 在另一个终端窗口中
nsqd --lookupd-tcp-address=127.0.0.1:4160
# 默认监听端口：4150（TCP），4151（HTTP）
3. 最后启动 nsqadmin
# 在第三个终端窗口中
nsqadmin --lookupd-http-address=127.0.0.1:4161
# 默认监听端口：4171（HTTP）