# Copy Files from node1 to node2
## Ceph Monitor Commands
To copy the files from `node1` to `node2`, follow these steps:

Copy the following files from `node1` to `node2`:

- `/etc/ceph/ceph.client.admin.keyring`
- `/var/lib/ceph/bootstrap-osd/ceph.keyring`
- `/opt/monkey`
- `/opt/monmap`

```
scp /etc/ceph/ceph.client.admin.keyring root@node2:/etc/ceph/
scp /var/lib/ceph/bootstrap-osd/ceph.keyring root@node2:/var/lib/ceph/bootstrap-osd/
scp /opt/monkey root@node2:/tmp/
scp /opt/monmap root@node2:/tmp/
```

Buat monitor in `node2`:
```
sudo -u ceph ceph-mon --mkfs -i n2 --monmap /tmp/monmap --keyring /tmp/monkey 
```

enable msgr2
```
ceph mon enable-msgr2
systemctl enable ceph-mon@n2
systemctl enable ceph-mon.service
```


## Ceph Manager (MGR) Command
Membuat atau mengambil keyring untuk manajer `mgr.n2` dengan memberikan kemampuan (capability) mon `'allow profile mgr'` dan osd `'allow *'`

```
ceph auth get-or-create mgr.n2 mon 'allow profile mgr' osd 'allow *'
```

buat direktori mgr2
```
sudo -u ceph mkdir /var/lib/ceph/mgr/ceph-n2/
```

Output dari perintah ceph `auth get-or-create` simpan di `/var/lib/ceph/mgr/ceph-n2/keyring`:
```
sudo -u ceph  nano /var/lib/ceph/mgr/ceph-n2/keyring
```

Aktifkan mgr
```
ceph mon enable-msgr2
systemctl start ceph-mgr@n2
systemctl enable ceph-mgr@n2
```