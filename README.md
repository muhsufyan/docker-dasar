# VOLUME
Di versi docker terbaru tdk direkomendasikan use bind mount but use docker volume
<br>similiar bind mount, volume can management operation like make volume, see list volume, rmove/delete volume<br>
volume dianggap sbg storage u/ menyimpan data (pd bind mounts data disimpan dlm host scra langsung) di volume data dimanage didlm docker (disimpan dlm docker)<br>
by deafult all data disimpan didlm volume, volume dibuat scra otomatis sehingga nama volume itu acak (auto generate)<br>
melihat list volume<b>docker volume ls</b><br>
membuat volume : <b>docker volume create {nama volume}</b><br>
volume ini ibaratnya harddisk kosong, volume dpt dihapus tp container hrs di nonactive<br>
menghapus volume : <b>docker volume rm {nama volume}</b><br>

# CONTAINER VOLUME
mengintegrasikan volume dg container, agar data tetap aman<br>
cara use volume pd param --mount type nya isi dengan volume dan source diisi dengan nama volume<br>
pertama buat dulu volume nya ex docker volume create nginxvolume<br>lalu buat container barunya yg menggunakan volume tsb<br>
<b>docker container create --name {nama container} --mount "type=volume,source={nama volume},destination={lokasi file/folder inside container},readonly" {image}={tag}</b><br>
ex <b>docker run -d -it --name broken-container --mount "type=volume,source=nginxvolume,target=/usr/data" nginx:latest</b>

# BACKUP VOLUME
backup data dlm volume hrs dilakukan scra manual, cara backup volume adlh buat container baru lalu backup datanya dan simpan di archive (tar / zip) dan terakhir archive-nya disimpan di bind mounts<br>
tahap melakukan backup dapat dilihat di https://docs.google.com/presentation/d/1LoCIoqR68t-y7P7eOs_TVoooZy4mq-tc2cwInQAtfy0/edit#slide=id.p slide ke 89<br>contoh <br>
1. docker container stop mongovolume<br>
2. di host buat folder baru ex backup<br>
3. docker container create --name nginxbackup --mount "type=bind,source=/Users/khannedy/Developments/YOUTUBE/belajar-docker-dasar/backup,destination=/backup" --mount "type=volume,source=mongodata,destination=/data" nginx:latest <br>
4. docker container start nginxbackup<br>
5. docker container exec -i -t nginxbackup /bin/bash<br>
ls -l masuk ke /data hrsnya ada isinya lalu<br>
6. cd backup<br>
7. tar cvf /backup/backup.tar.gz /data : folder /data dibackup kedlm fole backup.tar.gz<br>
8. docker container stop nginxbackup<br>
9. docker container rm nginxbackup<br>
10. docker container start mongovolume<br>

CARA KEDUA, menggunakan perintah run (jlnkan perintah dicontainer) lalu --rm (otomatis hapus container stlh perintah dicontainer selesai)<br>
docker container run --rm --name ubuntubackup --mount "type=bind,source=/Users/khannedy/Developments/YOUTUBE/belajar-docker-dasar/backup,destination=/backup" --mount "type=volume,source=mongodata,destination=/data" ubuntu:latest tar cvf /backup/backup-lagi.tar.gz /data<br>
docker container start mongovolume<br>

# RESTORE VOLUME
menggunakan achive backup di volume baru, ex percobaan agar tahu apakah data backupnya corrupt ?<br>
<b>docker volume create mongorestore</b><br>
buat containernya cukup dijlnkan 1 kali yaitu untuk restore saja. ini sama sprti backup<br>
<b>docker container run --rm --name ubunturestore --mount "type=bind,source=/Users/khannedy/Developments/YOUTUBE/belajar-docker-dasar/backup,destination=/backup" --mount "type=volume,source=mongorestore,destination=/data" ubuntu:latest bash -c "cd /data && tar xvf /backup/backup.tar.gz --strip 1"</b><br>
perintah untuk extract data => cd /data && tar xvf /backup/backup.tar.gz --strip 1<br> 
buat container baru yg menggunakan data backup<br>
<b>docker container create --name mongorestore --publish 27020:27017 --mount "type=volume,source=mongorestore,destination=/data/db" --env MONGO_INITDB_ROOT_USERNAME=eko --env MONGO_INITDB_ROOT_PASSWORD=eko mongo:latest</b><br>
<b>docker container start mongorestore</b><br>
