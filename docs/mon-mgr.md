# MON & MGR
## Ceph Monitor Commands
Membuat keyring baru dengan nama `/tmp/monkey` untuk entitas `mon.` dan memberikan kemampuan (capability) mon `'allow *'`.

```
ceph-authtool --create-keyring /tmp/monkey --gen-key -n mon. --cap mon 'allow *'
```

Membuat keyring baru dengan nama `/etc/ceph/ceph.client.admin.keyring` untuk entitas `client.admin` dan memberikan kemampuan (capability) mon `'allow *'`, osd `'allow *'`, dan mgr `'allow *'`.
```
ceph-authtool --create-keyring /etc/ceph/ceph.client.admin.keyring --gen-key -n client.admin --cap mon 'allow *' --cap osd 'allow *' --cap mgr 'allow *'
```

Membuat keyring baru dengan nama `/var/lib/ceph/bootstrap-osd/ceph.keyring` untuk entitas `client.bootstrap-osd` dan memberikan kemampuan (capability) mon `'profile bootstrap-osd'`.

```
ceph-authtool --create-keyring /var/lib/ceph/bootstrap-osd/ceph.keyring --gen-key -n client.bootstrap-osd --cap mon 'profile bootstrap-osd'
```

Mengimpor keyring dari `/var/lib/ceph/bootstrap-osd/ceph.keyring` ke keyring `/tmp/monkey`.
```
ceph-authtool /tmp/monkey --import-keyring /var/lib/ceph/bootstrap-osd/ceph.keyring
```

Mengimpor keyring dari `/etc/ceph/ceph.client.admin.keyring` ke keyring `/tmp/monkey`.
```
ceph-authtool /tmp/monkey --import-keyring /etc/ceph/ceph.client.admin.keyring /tmp/monkey
```
Berikan permision untuk ceph pada monkey
```
chown ceph:ceph /tmp/monkey 
```

Cadangkan keyring dan monmap
```
cp /tmp/{monmap,monkey} /opt
```

Membuat monmap baru dengan menambahkan monitor `n1` dengan alamat IP `192.168.1.3` dan FSID `ceeac5f2-1b18-4d3b-9e9f-439e16bc4b6a`, kemudian menyimpannya di `/tmp/monmap`.
```
sudo -u ceph monmaptool --create -add n1 192.168.1.3 --fsid ceeac5f2-1b18-4d3b-9e9f-439e16bc4b6a /tmp/monmap
```

Menambahkan monitor `n2` dengan alamat IP `192.168.1.4` dan FSID `ceeac5f2-1b18-4d3b-9e9f-439e16bc4b6a` ke monmap yang ada di `/tmp/monmap`.
```
sudo -u ceph monmaptool --add n2 192.168.1.4 --fsid ceeac5f2-1b18-4d3b-9e9f-439e16bc4b6a /tmp/monmap
```


Menjalankan perintah `ceph-mon` sebagai pengguna `ceph` untuk membuat filesystem untuk monitor dengan nama `n1`, menggunakan monmap dari `/tmp/monmap`, dan keyring dari `/tmp/monkey`.

```
sudo -u ceph ceph-mon --mkfs -i n1 --monmap /tmp/monmap --keyring /tmp/monkey
```

Aktifkan ceph-mon
```
systemctl restart ceph-mon@n1
systemctl enable ceph-mon@n1
systemctl enable ceph-mon.service
```

Menampilkan isi terakhir dari log monitor dengan nama `n1` yang terdapat di `/var/log/ceph/ceph-mon.n1.log`

```
tail /var/log/ceph/ceph-mon.n1.log
```

## Ceph Manager (MGR) Command
Membuat atau mengambil keyring untuk manajer `mgr.n1` dengan memberikan kemampuan (capability) mon `'allow profile mgr'` dan osd `'allow *'`

```
ceph auth get-or-create mgr.n1 mon 'allow profile mgr' osd 'allow *'
```

Output dari perintah di atas akan disimpan di:
```
/var/lib/ceph/mgr/ceph-n1/keyring
```

Aktifkan mgr
```
ceph mon enable-msgr2
systemctl start ceph-mgr@n1
systemctl enable ceph-mgr@n1
```


## Ceph Manager Dashboard Commands
install paket `ceph-mgr-dashboard` yang diperlukan untuk mengaktifkan dashboard pada Ceph Manager.

```
apt install ceph-mgr-dashboard
```


Mengaktifkan modul dashboard pada Ceph Manager.
```
ceph mgr module enable dashboard
```

Membuat sertifikat self-signed untuk digunakan oleh dashboard Ceph.
```
ceph dashboard create-self-signed-cert
```

Simpan password yang akan digunakan untuk akun administrator dashboard dalam file `password`.
```
echo y04M22nxA! > password
```

Membuat akun pengguna dengan nama `admin` sebagai administrator pada dashboard Ceph, dengan menggunakan password yang disimpan di file `password`.
```
ceph dashboard ac-user-create admin -i password administrator
```




