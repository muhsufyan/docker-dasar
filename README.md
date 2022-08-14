# PRUNE
menghapus hal" yg tdk digunakan lagi oleh docker ex container, image, volume yg sdh tdk gunakan lagi akan dihapus. untuk menghapus scra otomatis gunakan prune<br>
* menghapus semua container yang sudah stop, gunakan : <b>docker container prune</b><br> 
* menghapus semua image yang tidak digunakan container, gunakan : <b>docker image prune</b><br> 
* menghapus semua network yang tidak digunakan container, gunakan : <b>docker network prune</b><br> 
* menghapus semua volume yang tidak digunakan container, gunakan : <b>docker volume prune</b><br> 
* Atau kita bisa menggunakan satu perintah untuk menghapus container, network dan image yang sudah tidak digunakan menggunakan perintah : <b>docker system prune</b><br>
