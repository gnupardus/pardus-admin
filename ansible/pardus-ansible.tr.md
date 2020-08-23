# Pardus & Ansible

Ansible’ı yüklemek için(Debian,Pardus);

```
sudo apt update -y

sudo apt install ansible -y

```

daha sonra bu bilgisayar ssh-key üretip istemcilere bu ssh-key’i ileteceğiz.

```
ssh-keygen -t rsa

Enter file in which to save the key (/home/pardus/.ssh/id_rsa):
/home/pardus/.ssh/id_rsa already exists.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/pardus/.ssh/id_rsa.
Your public key has been saved in /home/pardus/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:EgUoPrRq9lFTNoWfVnm9MC+gGPOQnXHhJ4voCOiEq+s pardus@ansible-server
The key’s randomart image is:
+—[RSA 2048]—-+
| …*oooo . |
| o . @ oo+ + . |
| o o + O +oo.+ .|
|. = o o.*. +. o |
|.+ o. o.S. . . |
|++ .. o. |
|+.. .. . |
|. . |
|+E |
+—-[SHA256]—–+
```

“/home/pardus/.ssh/id_rsa” ile ssh-key’in oluşturulacağı yer belirlenir, standardına dokunmadan ilerliyoruz. “passphrase” sorusuna herhangi bir parola girmeden devam edelim, key göndereceğimiz için parola ile bağlantı yapmayacağız.

Daha sonra bu oluşturulan keyi istemciye göndermemiz gerekiyor, bunu için;

```
ssh-copy-id istemci@ip_adresi
```

ile gönderim sağlanır.

Not: Daha önce gönderim yapıldı ve yenilenmesi isteniyorsa -f parametresi kullanılır, yani;

```
ssh-copy-id -f istemci@ip_adresi
```

şeklinde kullanılır.

Ansible standart dosyalarını /etc/ansible/ dizini altında tutar, biz ise ile çalışmak için bir dizin oluşturacağız;

```
cd ~

mkdir ansible && cd ansible

cp -rf /etc/ansible/* .

pardus@ansible-server:~/ansible$ ls -la
total 32
drwxr-xr-x 2 root root 4096 Aug 13 18:58 .
drwxr-xr-x 6 pardus pardus 4096 Aug 14 06:05 ..
-rw-r–r– 1 root root 20265 Aug 13 18:54 ansible.cfg
-rw-r–r– 1 root root 1022 Aug 13 18:58 hosts
```

ansible.cfg dosyasında “inventory” değerini;

```
inventory = ./hosts
```

şeklinde değiştiriyoruz.

Daha sonra “hosts” dosyasına;

```
[parduslar]
192.168.122.86

```

şeklinde hostları ekliyoruz. Birden fazla istemci alt alta ip veya hostname şeklinde eklenebilir. Aynı şekilde benzer istemciler gruplanabilir. Örneğin CentOS istemciler;

```
[centos]
192.168.122.42
192.168.122.14
192.168.122.57
```

şeklinde eklenebilir. Bu sayede gönderilecek görev bu gruplamaya göre iletilebilir. Yani CentOS istemcilere ayrı, Ubuntu istemcilere ayrı veya all seçeneği ile tüm istemcilere aynı görev gönderilebilir.

Gelelim görev göndermeye, bunun için Ansible dizininde iken;

```
ansible -i hosts -m ping all
```

komutu ile ping atabiliriz. Bu komutun çıktısı;

```
192.168.122.86 | SUCCESS => {
“changed”: false,
“ping”: “pong”
}
```

şeklinde olacaktır.

```
ansible -i hosts -a “ip a” all
```

şeklinde “-a” parametresi ile karşıdaki istemcilerde komut çalıştırışabilir. “all” parametresi yerine;

```
ansible -i hosts -a “ip a” parduslar
```

şeklinde sadece parduslar grubunda olan istemcilere uygulanır.
