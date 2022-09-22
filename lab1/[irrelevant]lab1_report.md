###### University: [ITMO University](https://itmo.ru/ru/)
###### Faculty: [FICT](https://fict.itmo.ru)
###### Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)
###### Year: 2022/2023
###### Group: K34212
###### Author: Mikhailov Ivan Yuryevich
###### Lab: Lab1
###### Date of create: 20.09.2022
###### Date of finished: 31.09.2022
***
# Установка CHR и Ansible, настройка VPN
### Задачи работы:
1. Развертывание виртуальной машины на базе платформы Yandex Cloud с установленной системой контроля конфигураций Ansible
2. Установка CHR(RouterOS) в VirtualBox
3. Организация VPN сервера в сервере автоматизации где была установлена система контроля конфигураций Ansible
4. Поднятие VPN туннель между VPN сервером на Ubuntu и VPN клиентом на RouterOS

### Ход Работы
1. Создан аккаунт в яндекс облаке с ОС Ubuntu 20 и с параметрами, данными Рисунке ниже  


  ![Screenshot_61](https://user-images.githubusercontent.com/56927592/191528801-74aa7c2b-6636-4b69-9bc5-6c2d15f9be6e.png)

  Далее были установлены Python и Ansible

  <img src="https://user-images.githubusercontent.com/56927592/191530178-79433b01-ab7a-4a97-9d64-a4b5641fd1ec.png" width="500" height="140" />

2. Скачен образ RouterOS 7.5 и успешно установлен на WirtualBox


  <img src="https://user-images.githubusercontent.com/56927592/191533001-da0ae13c-8c31-4b57-a6e6-f77e22bbc552.png" width="350" height="230" />

3. Для организации VPN сервера был установлен протокол WireGuard и сгенерированы публичный и секретный ssh- ключи


  <img src="https://user-images.githubusercontent.com/56927592/191534321-f138d8a4-867d-493a-82ba-98de7003e5d4.png" width="650" height="550" />

  Далее в конфиге написан протокол сервера, в котором, а именно интерфейс, через который будет происходить взаимодействие с клиентами 
  
  <img src="https://user-images.githubusercontent.com/56927592/191535678-27a48256-a967-416f-b7c7-ab282f6c4afb.png" width="650" height="150" />
  
  Address — это адрес сервера при соединении по VPN. ListenPort - порт, который слушает протокол.  PostUp и PostDown команды которые будут выполняться при активации и    деактивации сетевого интерфейса wg0. Это команды для файрволла включающие форвардинг пакетов.


  В файле sysctl.conf разкомментировали строчку net.ipv4.ip_forward=1 для того чтобы включить поддержку IP форвардинга.

  <img src="https://user-images.githubusercontent.com/56927592/191536543-75753358-c575-43eb-8723-c4d451c5de7d.png" width="400" height="320" />
  
  Далее через systemctl включен и запущен демон WireGuard а так же для приватного ключа и конфига выставлены настройки доступа только для чтения.
  
  <img src="https://user-images.githubusercontent.com/56927592/191540410-1871b91b-cd95-4644-80b2-efd4915553c8.png" width="550" height="250" />

4. На RouterOS, выступающем в роли клиента так же создан новый интерфейс и присвоин адрес этому интерфейсу


  <img src="https://user-images.githubusercontent.com/56927592/191543180-615adf7c-4090-4b14-abc1-185773dfe3a3.png" width="550" height="150" />
  
  Далее настроен пир со следующими параметрами: Public Key - публичный ключ респондера, Endpoint и Endpoint Port - адрес респондера и его порт, Allowed Address - разрешён любой трафик в туннеле. Persistent Keepalive добавлен на случай нахождения за NAT. 
  
  Помимо этого для смены IP адреса включен маскарадинг для созданного интерфейса (action = masquerade)
  
  <img src="https://user-images.githubusercontent.com/56927592/191546060-50c50b20-99cc-4afc-aca5-5bbaa51edbf7.png" width="550" height="100" />
  
  В последнем шаге на VPN сервере в конфиг wireguard-a добавлен пир клиента.
  
  <img src="https://user-images.githubusercontent.com/56927592/191564780-eb627a22-be29-49e4-bd75-23968730a315.png" width="550" height="120" />
