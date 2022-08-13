# docker-dasar
CONTAINER

container terbuat dari image. <br>
container sprti perangkat-nya sedangkan image adlah aplikasinya, artinya 1 image bisa membuat 2/lbh container syaratnya nama dari container harus berbeda. Dg kata lain container itu menggunakan image sehingga ketika image dihapus maka container tdk akan bisa berjalan<br>
when complete make container, the container status is non-active<br>
display all container (active & non-active) : <b>docker container ls -a</b><br>
display container with status is active : <b>docker container ls</b><br>
* make a new container : <b>docker create --name {nama container yg kita berikan} {nama image untuk membuat/yg akan digunakan oleh container}:{tag/versi dari image}</b><br>
cara bacanya docker buatkan container baru dg nama {nama container yg kita berikan} yg menggunakan image {nama image untuk membuat/yg akan digunakan oleh container} dimana tag/versi dari image tersebut adalah tag/versi ke {tag/versi dari image}<br>
nama dari container hrs unik & nama nantinya berfungsi juga sebagai alias untuk menggantikan container id, jd untuk mengakses / melakukan operasi(run, hapus) pada container bisa lewat container id / nama dari container yg kita berikan<br>
* make container to active status (running container) : <b>docker container start {container Id atau bisa juga gunakan nama dari container}</b><br>
pada saat display all container we see PORT, ex 123/tcp artinya container tsb menggunakan port 123 & port 123 itu milik docker bukan milik server/pc/laptop sehingga jika ingin mengakses maka port dari docker tsb harus di expose<br>dan jika ada container yg menggunakan port yg sama itu tdk akan masalah karena 1) port tsb mrpkn port docker 2) setiap container menggunakan port yg terisolasi antara container 1 dg yg lainnya.<br>
ex container A memakai port 123 dan container B memakai port 123 itu tdk masalah meskipun portnya sama karena 2 alasan td.
* stop / non-active container : <b>docker container stop {container id atau nama container yg ingin distop/nonaktifkan}</b><br>
jika ingin menghapus container maka harus di stop/non-active kan terlbh dahulu
* remove/delete container : <b>docker container rm {container id atau nama container yg ingin dihapus}</b><br>

# CONTAINER LOG

untuk melihat log, ex if our app error so we see the log for know detail what happen

* see log from a container (not real-time mode) : <b>docker container logs {container id atau nama container yg ingin dilihat log nya}</b><br>

* see log from a container (real-time mode) : <b>docker container logs -f {container id atau nama container yg ingin dilihat log nya}</b><br>

# CONTAINER EXEC

untuk mengekseskusi kode program(query sql, dll) dari dlm aplikasi(redis, mysql, dll) atau container-nya gunakan container exec. perintahnya<br>
<b>docker container exec -i -t {container id/nama container} /bin/bash</b><br>
<i>-i</i> => keep input always active, apa yg kita ketik akan dieksekusi<br>
<i>-t</i> => masuk kedalam container dengan mode terminal akses, artinya kita dpt mengakses container menggunakan terminal layaknya di linux<br>
<i>/bin/bash</i> => app based on linux/unix usually have /bin/bash<br>
ex masuk kedlm container redis yg kita beri nama redis1 => <u>docker container exec -i -t redis1 /bin/bash</u> selanjutnya kita bisa mengeksekusi perintah redis didlm container redis1<br>

# CONTAINER PORT

setiap container itu terisolasi dr container lainnya sehingga laptop/pc kita tdk bisa mengakses langsung app dlm container. Untuk mengakses bisa memakai container exec / container mengexpose port melalui port forwarding. port forward itu kita membuka port didlm pc/laptop lalu forward kedlm docker container. ex port 123:456 artinya 123 adlh port didlm container yg terisolasi (tdk dpt diakses diluar container) sedangkan 456 port pc/laptop yg akan mengakses ke container, maksudnya port 456 yg mrpkn port dr pc/laptop kita akan melakukan port forwading ke port 123 sehingga pc/laptop kita dpt terhub ke container.
<br>perintah membuat container & melakukan port forwarding<br>
<b>docker container create --name {nama container yg kita beri} --publish {port untuk host/pc/laptop}:{port untuk container-nya} {image}:{tag}</b><br>
kita dpt melakukan port forwarding lbh dr 1 kali dg cara tambahkan 2 kali param --publish <br>
<i>--publish</i> dpt disingkat jd <i>-p</i><br>
jika sdh membuat container tp tanpa port forwarding maka container nya hrs dihapus dulu, tdk bisa diedit<br>
ex docker container --name nginxserver1 -p 8080:80  nginx:latest<br>
dr perintah tsb untuk mengakses container nginxserver1 maka gunakan port 8080 di pc/laptop kita yg nantinya akan melakukan port forwading ke container nginxserver1 di port 80.<br>

# CONTAINER ENVIRONMENT VARIABLE

untuk menggunakan, menambahkan environment variable di container gunakan printah <b>--env</b> atau <b>-e</b><br> perintah buat container baru dg set environment variablenya sbb<br>
<b>docker container --name {nama container} --env {KEY HARUS HURUF KAPITAL SEMUA}="{value}" --env {KEY2 HARUS HURUF KAPITAL SEMUA}="{value 2}" {image}:{tag}</b><br>
ex => docker container --name mongodb1 --publish 27017:27017 --env MONGO_INITDB_ROOT_USERNAME=root --env MONGO_INITDB_ROOT_PASSWORD=password mongo:latest

# CONTAINER STATS

melihat detail penggunaan resource (ex CPU, memory) untuk setiap container yg sedang berjalan : <b>docker container stats</b><br>

# CONTAINER RESOURCE LIMIT

saat membuat container baiknya kita membatasi penggunaan sumber daya (resource limit) agar container tdk memakan banyak resource yang diberikan dari server/laptop/pc ke docker, ex kita alokasikan CPU dari pc (4GB) untuk docker itu 2 GB lalu kita buat 1 container baru & agar container baru tdk memakan banyak resource (resource docker adlh 2 GB) kita batasi container baru ini resource CPU nya adalah 500MB dari total 2 GB. dg kata lain 2 GB adlh resource CPU docker, 4 GB adlh CPU pc, dan 500MB adlh CPU container baru. hal ini dilakukan agar performa dari container lain tdk jd lemot akibat resource terlalu banyak digunakan oleh 1 buah container baru td.<br>
set limit memory when make new a container add argument <u>--memory</u> followed value limit, untuk satuan ukurannya m = mega byte, b = byte<br>
for CPU add argument <u>--cpus</u> when set 1.5 mean container can use 1 & 0.5 CPU core<br>
ex => docker container create --name smallnginx --publish 8080:80 --memory 100m --cpus 0.5 nginx:latest
