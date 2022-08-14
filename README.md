# NETWORK
by default setiap container terisolasi satu dg lainnya, jd container tdk dpt memanggil container lainnya. <br>
dg Network kita dpt menghub container satu dg lainnya (dpt berkomunikasi) selama dalam 1 network yg sama<br>
in network, can management network like see, make, delete network<br>
when make network we need set driver apa yg akan we use. banyak jenis dr network & kadang network tertentu perlu syarat"<br>
1. bridge => driver make virtual network so container can connection inside same bridge<br>
2. host => driver make network same with host system but syaratnya hanya jln di linux saja.<br>
3. none => default driver that network cant connection each other. container cant connection each other<br>
see docker network : <b> docker network ls</b><br>
make a new network : <b>docker network create --driver {nama driver (host, bridge, none)/ if empty default is bridge} {nama network}</b><br>
delete network : <b>docker network rm {nama network}</b><br>
ketika ingin menghapus network maka container yg memakai network tersebut hrs dihapus dahulu dari network yg akan dihapus<br>
contoh docker network create --driver bridge mongonetwork<br>
# CONTAINER NETWORK

menambahkan container ke network yg tlh kita buat sehingga container" dlm network dpt berkomunikasi<br>
cara container a mengakses container b dlm 1 network bridge cukup gunakan nama host nya b, nama host = nama container<br>
<u>ex studi kasusnya mongo express must connect to mongodb</u><br>
buat container dan append container to network use --network: docker container create --name {nama container} --network {nama network} {image}:{tag}<b><br>
contoh<br>
docker container create --name mongodb --network mongonetwork --env MONGO_INITDB_ROOT_USERNAME=eko --env MONGO_INITDB_ROOT_PASSWORD=eko mongo:latest<br>
docker image pull mongo-express:latest<br>
docker container create --name mongodbexpress --network mongonetwork --publish 8081:8081 --env ME_CONFIG_MONGODB_URL="mongodb://eko:eko@mongodb:27017/" mongo-express:latest<br>
now mongodb & mongo expressconnected, in mongo express env ME_CONFIG_MONGODB_URL is punya nya container mongodb bukan host (pc/laptop) ex mongodb:27017 => port 27017 adlh milik container mongodb bukan host pc/laptop & juga mongo adlh host dari container mongodb bukan host pc/laptop.<br>
stlh join kita ingin hapus container dari network caranya dg perintah<br>
<b>docker network disconnect {name network} {nama container}</b><br>
ex container mongodb keluar dari network mongonetwork menyebabkan mongoexpress tdk dpt berkomunikasi dg mongodb maka perintahnya<br>
docker network disconnect mongonetwork mongodb<br>
menambah container kedalam suatu network : <b>docker network connect {nama network} {nama container}</b><br>
ex memasukkan container mongodb kedalam network mongonetwork sehingga mongoexpress dpt berkomunikasi dg mongodb lagi perintahnya<br>
docker network connect mongonetwork mongodb<br>