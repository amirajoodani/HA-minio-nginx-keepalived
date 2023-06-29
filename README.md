# what is minio ?
MinIO is a high-performance, S3 compatible object store. It is built for large scale AI/ML, data lake and database workloads. It runs on-prem and <br>
on any cloud (public or private) and from the data center to the edge. MinIO is software-defined and open source under GNU AGPL v3. <br>
 ![minio picture](https://github.com/amirajoodani/HA_minio_nginx_keepalived/assets/42912741/37ab4057-185c-4449-b476-0d9db87fad6a) 

 # technology that we use in this project ?
 1-minio object storage <br>
 2-docker <br>
 3-nginx <br>
 4-keepalived <br>
# what we have done ?
we setup Ha for minio object storage with nginx and keepalievd service . as we can see in the picture : <br>
![diagram](https://github.com/amirajoodani/HA_minio_nginx_keepalived/assets/42912741/75e2f774-a546-48c4-8c90-c227ec04d3be)

so if any instance of minio goes down , we have another one up behind the our domain based on nginx confguration . 
# steps to setup :
# 1- install docker on both vm <br>
# 2- bring up dockerize minio instance on two port 9001 and 8001  with docker-compose.yml file in this repository <br>
```
docker-compose up -d
```
now you should be able to see minio web cosole with both ip on two port 8001 and 9001 <br>
![minio1](https://github.com/amirajoodani/HA_minio_nginx_keepalived/assets/42912741/ee609023-3fc3-48ce-ae0d-adfccbb3acce)
![minio2](https://github.com/amirajoodani/HA_minio_nginx_keepalived/assets/42912741/c57cf609-05cb-447a-9cea-53bea844dfd3)

 # 3- install nginx and keepalived service <br>
```
sudo apt install -y nginx keepalived htop
sudo systemctl enable nginx --now
```
enable http traffic on your firewalll <br>
```
ufw allow http
```
allow vrrp protocol on your firewall or allow all traffic form another vm on your vm : <br>
(on 1.63 vm)
```
ufw allow from 192.168.1.12
ufw allow to 192.168.1.12
```
# 4- config keepalived 
firest get backup form keepalived service : <br>
```
sudo cp /etc/keepalived/keepalived.conf /etc/keepalived/keepalived.conf.bck
```
![minio5](https://github.com/amirajoodani/HA_minio_nginx_keepalived/assets/42912741/f5575df9-cee7-4017-b1c2-7be87f0b02c4)

on 1.12 vm we should config keepalived as master node and on 1.63 config it as slave mode . we set virtual ip address 192.168.1.100 on both config node . wet lower priroty on slave mode . use master and slave config in this repository . and then start keepalived serrvice on both node  : <br>
```
systemctl start keepalived
```
on master node you will see : <br>
![minio3](https://github.com/amirajoodani/HA_minio_nginx_keepalived/assets/42912741/f08fc1be-2b7f-42e5-add2-11fd5f116307)

if your config was ok , your master node get message of slave node . <br>

on slave node : <br>
![minio4](https://github.com/amirajoodani/HA_minio_nginx_keepalived/assets/42912741/7562aace-299b-4e64-9c17-48ec60d11eca)

on slave node detect which one is master .<br>

# 5 -config nginx:
for setup ha in level of minio service, use nginx config in this repo and restart nginx service . <br>
# 6- finish 
now you can see your ha minio cluster behind your domain : <br>
![minio6](https://github.com/amirajoodani/HA_minio_nginx_keepalived/assets/42912741/02dc360a-e30c-4812-ae11-835dd2fd6257)





