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
  
  Проброшен порт из VirtualBox в компьютер для доступа в сеть.
  
  <img src="https://user-images.githubusercontent.com/56927592/191825617-e6747e47-8d5a-47d3-a110-109d708cd98e.png" width="350" height="100" />

3. На Ubuntu установлен VPN сервер openVPN. На просторах интеренета был найден готовый скрипт, устанавливающий сервер на убунту. Всё что нужно было сделать это      скачать файл с ним командой wget, дать ему разрешение на запуск командой chmod +x и запустить.

  <img src="https://user-images.githubusercontent.com/56927592/191912299-cc544060-2040-4127-b6e0-e1a668552a73.png" width="450" height="180" />

  Ниже представлен код всего скрипта. 

  ![image](https://user-images.githubusercontent.com/56927592/191913673-159c3885-c844-4b7b-8a7d-ac65f815f3e4.png)
  ![image](https://user-images.githubusercontent.com/56927592/191913773-7a9fdb5d-8301-41a2-8ea1-0ed4f6dc5d47.png)
  ![image](https://user-images.githubusercontent.com/56927592/191913837-7d859874-fbcd-4585-ba29-e7bdd8a88ebc.png)

4. Для установки CHR в качестве клиента OpenVPN сгенерированный сертификат и ключ клиента из файлов `etc/openvpn/mikrotik-ssl/client-20605.crt` и `etc/openvpn/mikrotik-ssl/client-20605.key` соответсвенно скопированы в видимое окружение winBox.

  <img src="https://user-images.githubusercontent.com/56927592/191916662-5acea3c1-60b9-416b-9245-bbacb655cda9.png" width="450" height="120" />

  После этого сертификат был подключёт в RouterOs.

  <img src="https://user-images.githubusercontent.com/56927592/191917657-5b1d8c86-71fd-43cd-87e8-05d54f982f6f.png" width="450" height="90" />

  Далее создан openVPN интерфейс подключающийся к публичному IP адресу сервера, по порту 20605.

  <img src="https://user-images.githubusercontent.com/56927592/191918124-a7d7b84e-c016-4b4c-8f93-e11366458a3f.png" width="310" height="400" />

  Для проверки установенного соединения интерфейс сервера сметрящий в VPN тоннель был пропингован клиентом. Результат успешный.

  <img src="https://user-images.githubusercontent.com/56927592/191918910-baf1c414-cd8f-4adc-8813-2ab317ef8b3c.png" width="470" height="120" />


### Вывод
В ходе выполнения лабораторной работы linux система, находящуюся в облаке, была настроена в качесвте openVPN сервера, а RouterOS, находящийся в WirtualBox, был настроен в качестве openVPN клиента, и между ними был установлен vpn туннель.  
  
  
