## Q & A

### Berapa perkiraan sumber daya CPU dan RAM yang digunakan untuk MON dan MGR saat menghandle 24 x OSD?
Perkiraan sumber daya CPU dan RAM yang digunakan untuk MON (Monitor) dan MGR (Manajer) dalam menghandle 24 OSD dapat bervariasi tergantung pada faktor-faktor seperti ukuran data yang diolah, tingkat I/O, dan konfigurasi jaringan. Namun, berikut adalah perkiraan umum yang dapat Anda gunakan sebagai referensi:

#### MON (Monitor):

- CPU: Monitor biasanya tidak membutuhkan sumber daya CPU yang tinggi. Secara umum, CPU dengan kecepatan clock moderat dan beberapa core biasanya cukup untuk menjalankan monitor dalam cluster Ceph.
- RAM: Monitor membutuhkan jumlah RAM yang relatif rendah. Sejumlah 1-2 GB RAM biasanya cukup untuk menjalankan monitor dalam cluster Ceph.


#### MGR (Manajer):

- CPU: Manajer juga membutuhkan sumber daya CPU yang moderat. Dalam pengaturan yang lebih besar, dengan 24 OSD, dianjurkan menggunakan CPU multi-core dengan kecepatan clock yang lebih tinggi. CPU dengan 4-8 core biasanya cukup untuk menjalankan manajer dalam cluster Ceph.
- RAM: Manajer membutuhkan lebih banyak RAM daripada monitor. Dalam lingkungan dengan 24 OSD, dianjurkan memiliki setidaknya 4-8 GB RAM untuk menjalankan manajer dengan lancar. Namun, jika Anda mengharapkan beban kerja yang lebih tinggi atau pertumbuhan di masa depan, mungkin diperlukan RAM yang lebih besar.

#### Berapa yang dibutukan sumber daya 8 x OSD dalam 1 node?
Perkiraan sumber daya yang dibutuhkan untuk menjalankan 8 OSD (Object Storage Daemon) dalam satu node Ceph dapat bervariasi tergantung pada beberapa faktor, seperti ukuran data yang diolah, kecepatan I/O, dan konfigurasi jaringan. Namun, berikut ini adalah perkiraan umum untuk sumber daya yang diperlukan:

- CPU: Meskipun OSD dapat memproses operasi I/O yang intensif, kebutuhan CPU biasanya tidak terlalu tinggi. Sebuah CPU dengan beberapa core (misalnya, 4-8 core) dan kecepatan clock moderat (misalnya, 2,0-3,0 GHz) biasanya cukup untuk menjalankan 8 OSD dalam satu node.

- RAM: Kebutuhan RAM dapat beragam, tergantung pada ukuran data yang diolah oleh OSD dan jenis beban kerja. Secara umum, disarankan untuk memiliki setidaknya 2-4 GB RAM per OSD. Jadi, untuk menjalankan 8 OSD, Anda mungkin membutuhkan 16-32 GB RAM.

- Penyimpanan: Kebutuhan penyimpanan untuk OSD tergantung pada kapasitas total data yang akan disimpan dalam cluster. Pastikan node tersebut memiliki kapasitas penyimpanan yang cukup untuk menampung data yang diinginkan. Dalam hal ini, 8 OSD akan menggunakan ruang penyimpanan yang signifikan.

- Jaringan: Pastikan node tersebut memiliki konektivitas jaringan yang memadai, terutama bandwidth yang cukup untuk mentransfer data antara OSD, monitor, dan klien. Bandwidth jaringan gigabit (1 Gbps) atau lebih tinggi sangat disarankan untuk performa yang optimal.

### Topologi yang disarankan untuk ceph cluster
1. Minimal tiga monitor (mon): Direkomendasikan untuk memiliki setidaknya tiga monitor dalam cluster Ceph. Monitor bertanggung jawab untuk menyimpan metadata cluster dan mengatur keseluruhan cluster. Dengan memiliki tiga monitor, Anda dapat mencapai toleransi kesalahan dan ketersediaan yang baik.

1. Pisahkan mon, mgr, dan osd: Lebih baik untuk memisahkan komponen mon, mgr, dan osd ke dalam node yang berbeda. Ini membantu mencegah risiko kegagalan tunggal yang dapat mempengaruhi seluruh cluster dan juga memungkinkan alokasi sumber daya yang lebih baik serta meningkatkan kinerja.

1. Distribusikan OSD secara merata: OSD (Object Storage Daemon) bertanggung jawab untuk menyimpan data pada node penyimpanan fisik. Penting untuk mendistribusikan OSD secara merata di seluruh node dalam cluster. Ini membantu dalam mendistribusikan beban kerja, memaksimalkan kinerja, dan memberikan toleransi kesalahan yang baik.

1. Gunakan jaringan yang terpisah: Direkomendasikan untuk menggunakan jaringan yang terpisah untuk komunikasi antara komponen Ceph, seperti jaringan jasa data, jaringan cluster, dan jaringan penelusuran. Menggunakan jaringan yang terpisah membantu menghindari persaingan sumber daya dan memperbaiki kinerja serta keamanan.

1. Pertimbangkan skala dan pertumbuhan: Saat merancang cluster Ceph, pertimbangkan skala dan pertumbuhan yang mungkin terjadi di masa depan. Anda harus dapat dengan mudah menambahkan lebih banyak OSD, monitor, atau manajer ke dalam cluster tanpa terlalu banyak mengubah arsitektur atau menghancurkan cluster yang ada.

1. Pertimbangkan keandalan perangkat keras: Cluster Ceph mengandalkan perangkat keras yang andal dan stabil. Pastikan perangkat keras yang Anda gunakan sesuai dengan spesifikasi dan rekomendasi Ceph. Pertimbangkan penggunaan disk dengan performa tinggi, koneksi jaringan gigabit, dan sumber daya yang memadai untuk memenuhi kebutuhan cluster.

### Bagaimana jika mon, mgr dan osd digabung tapi ada 3x node?

Jika Anda memiliki tiga node dan ingin menggabungkan mon (monitor), mgr (manajer), dan osd (Object Storage Daemon) dalam setiap node, Anda dapat membuat konfigurasi seperti itu. Namun, tetap ada beberapa pertimbangan yang perlu Anda perhatikan:

Toleransi Kesalahan: Menggabungkan semua komponen dalam satu node meningkatkan risiko kegagalan tunggal yang dapat menyebabkan ketidaktersediaan cluster. Jika satu node mengalami masalah, seluruh komponen di dalamnya terpengaruh. Oleh karena itu, untuk mencapai toleransi kesalahan yang lebih baik, direkomendasikan untuk mendistribusikan komponen mon, mgr, dan osd ke dalam node yang terpisah.

Kinerja dan Sumber Daya: Menggabungkan semua komponen dalam satu node dapat mempengaruhi kinerja keseluruhan cluster. Mon dan mgr membutuhkan sumber daya yang relatif rendah, sementara osd memerlukan sumber daya yang lebih besar. Jika Anda menggabungkan semuanya dalam satu node, ada kemungkinan terjadi persaingan sumber daya yang dapat membatasi kinerja cluster. Penting untuk memastikan bahwa node tersebut memiliki sumber daya yang cukup untuk menjalankan semua komponen dengan lancar.

Skalabilitas: Menggabungkan semua komponen dalam satu node dapat membatasi skalabilitas cluster Ceph Anda di masa depan. Jika Anda berencana untuk menambahkan lebih banyak OSD atau mengembangkan cluster lebih lanjut, pengaturan ini mungkin menjadi kendala. Memiliki node terpisah untuk setiap komponen memungkinkan Anda dengan mudah menambahkan lebih banyak node dan memperluas cluster sesuai kebutuhan.

Jadi, sementara mungkin memungkinkan untuk menggabungkan mon, mgr, dan osd dalam tiga node yang berbeda, disarankan untuk mempertimbangkan faktor-faktor di atas untuk memastikan toleransi kesalahan, kinerja, dan skalabilitas yang optimal dalam desain dan operasi cluster Ceph Anda.

