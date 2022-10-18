ИНСТРУКЦИЯ
blah-blah


установка клиента openstack
```
sudo apt install python3-openstackclient
```

настройка проекта - api ключи - скачать "Openrc версии 3"
скаченный файл вида *openrc.sh использовать следующим образом:
```
source openrc.sh
```

Получить список доступных image, flavor, etc например
```
openstack image list | grep -i ubuntu
openstack flavor list | grep -i basic
```

Для прописывания ключа ssh в разворачиваемую машину для начала копируем открытую часть ключа в нужную папку, например ~/keys/
```
cp ~/.ssh/id_rsa.pub ~/projects/vm/keys/
```

Затем для добавления скопированного ключа в instance.tf добавляем:
```
resource "vkcs_compute_keypair" "test-keypair" {
  name       = "user52-terraform"
  public_key = file("./keys/id_rsa.pub")
}
```
а также:
```
key_pair = vkcs_compute_keypair.test-keypair.name
```
в раздел resource "vkcs_compute_instance" "compute" (например после строки ...GZ1)

Применяем настройки
```
terraform apply
```
На развернутую машину теперь можно зайти:
```
ssh ubuntu@ip
```