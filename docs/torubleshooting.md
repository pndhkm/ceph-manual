## Troubleshooting

### AUTH_INSECURE_GLOBAL_ID_RECLAIM_ALLOWED: mons are allowing insecure global_id reclaim

Jika `AUTH_INSECURE_GLOBAL_ID_RECLAIM_ALLOWED` diatur sebagai true, maka daemon monitor akan mengizinkan proses pengklaiman global ID tanpa memeriksa keamanan yang ketat. Ini berarti entitas yang memiliki global ID yang sama dengan entitas yang sebelumnya dihapus dapat mengklaim kembali global ID tersebut.

Namun, penting untuk dicatat bahwa menggunakan pengklaiman global ID yang tidak aman dapat memiliki konsekuensi keamanan. Jika entitas yang tidak sah atau tidak diotorisasi dapat mengklaim kembali global ID yang sebelumnya digunakan, ini dapat menyebabkan masalah keamanan atau kekacauan dalam sistem Ceph.

Jika keamanan merupakan kekhawatiran yang serius, disarankan untuk menjaga `AUTH_INSECURE_GLOBAL_ID_RECLAIM_ALLOWED` diatur sebagai false untuk mencegah pengklaiman global ID yang tidak aman. Dengan demikian, hanya entitas yang memiliki izin dan otoritas yang tepat yang dapat menggunakan kembali global ID yang sebelumnya digunakan.

langkah-langkah untuk menonaktifkannya:
1. Pergi ke menu `Configuration`
1. Cari & edit `auth_allow_insecure_global_id_reclaim`
1. Ubah nilai dari kolom global menjadi `false`
1. Update

