# BIND MOUNTS
Sharing/mounting file/folder inside host (pc/laptop) to container in docker. useful when send config from outside container, save data has been made in container app into folder in host<br>
use param <u>--mount</u> when make a new container<br>
rule in param mount<br>
1. type => bind / volume<br>
2. source => lokasi file / folder di sistem host<br>
3. destination => lokasi file / folder di container<br>
4. readonly => file/folder hanya dpt dibaca di container & tdk bisa menulis (edit)<br>
* <b>docker container create --name {nama container} --mount "type={bind atau volume},source={lokasi file/folder},destination={lokasi file/folder inside container},readonly" {image}={tag}</b><br>
ex <b>docker run -d -it --name broken-container --mount "type=bind,source=/usr/store,target=/usr/data" nginx:latest</b><br>
di document image ada store maka itu merupakan bind mount, file/folder dihost hrs dibuat dulu baru bisa memakai source<br>
ketika kita menghapus container maka data yang telah disimpan akan tetap ada