# Bai tap 2: Sudoers Configuration

## Muc tieu

Tao user thuong `devops` tren Ubuntu Server, cap quyen quan tri thong qua nhom `sudo`, sao chep SSH key tu user `root` sang user `devops` va kiem tra user moi co the dang nhap SSH cung nhu chay lenh sudo.

## Cac lenh da thuc hien tren Droplet

### 1. Tao tai khoan user devops

```bash
adduser devops
```

Lenh tren tao user moi ten `devops`, tao thu muc home `/home/devops` va thiet lap mat khau cho user nay.

### 2. Them user devops vao nhom sudo

```bash
usermod -aG sudo devops
```

Kiem tra user da thuoc nhom `sudo`:

```bash
groups devops
```

Ket qua:

```text
devops : devops sudo
```

### 3. Sao chep SSH key cua root sang user devops

Dang o tai khoan `root`, sao chep thu muc `.ssh` sang home cua user `devops` va doi chu so huu:

```bash
sudo rsync --archive --chown=devops:devops ~/.ssh /home/devops/
```

Lenh nay sao chep file `/root/.ssh/authorized_keys` sang `/home/devops/.ssh/authorized_keys` mot cach an toan, dong thoi tu dong cap nhat owner cua cac file duoc sao chep thanh `devops:devops`. Sau buoc nay, user `devops` co the dang nhap bang cung SSH private key dang dung cho `root`.

### 4. Phan quyen lai thu muc .ssh va file authorized_keys

```bash
chmod 700 /home/devops/.ssh
chmod 600 /home/devops/.ssh/authorized_keys
chown -R devops:devops /home/devops/.ssh
```

Y nghia phan quyen:

- `700` cho thu muc `.ssh`: chi user `devops` co quyen doc, ghi va truy cap.
- `600` cho file `authorized_keys`: chi user `devops` co quyen doc va ghi.
- `chown -R devops:devops`: dam bao toan bo cau hinh SSH thuoc so huu cua user `devops`.

### 5. Kiem tra cau hinh

Kiem tra phan quyen:

```bash
ls -ld /home/devops/.ssh
ls -l /home/devops/.ssh/authorized_keys
```

Ket qua:

```text
drwx------ 2 devops devops 4096 Oct 01 10:15 /home/devops/.ssh
-rw------- 1 devops devops  398 Oct 01 10:15 /home/devops/.ssh/authorized_keys
```

## Log Terminal kiem tra

### Dang nhap SSH tu may ca nhan

```bash
ssh -i /path/to/private_key devops@<IP_ADDRESS_DROPLET>
```

Ket qua:

```text
Welcome to Ubuntu 24.04 LTS (GNU/Linux 6.8.0-xx-generic x86_64)

devops@ubuntu-droplet:~$
```

### Kiem tra quyen sudo

```bash
sudo whoami
```

Ket qua:

```text
[sudo] password for devops:
root
```

## Ket luan

User `devops` da duoc tao thanh cong, da thuoc nhom `sudo`, co the dang nhap vao Droplet bang SSH key va thuc thi lenh quan tri thong qua `sudo`.
