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

3. На Ubuntu установлен VPN сервер openVPN, с использованием официальной инструкции по установке openVPN access server.

<img src="https://user-images.githubusercontent.com/56927592/192845544-1fecfa75-6845-429b-a77e-1efc1d7f264e.png" width="520" height="120" />

По окончанию установки были получены данные для входа в веб интерфейс сервера.

<img src="https://user-images.githubusercontent.com/56927592/192847019-4ebf838d-a3d1-4a46-8523-33b4e2d5aa64.png" width="420" height="120" />

В настройках сервера был отключен сертификат TLS и переключена передача по протоколу TCP, потому что иначе микротик не может поключиться к серверу.

  <img src="https://user-images.githubusercontent.com/56927592/192848574-68dc74f1-3c08-4f36-b86b-02b3b40d87dc.png" width="650" height="230" />

4. После входа был создан новый пользователь с логином и паролем и сгенерирован для него профиль.

  <img src="https://user-images.githubusercontent.com/56927592/192848486-840834d0-7e9c-449b-810d-f365e34f9b5a.png" width="550" height="150" />
  
В winbox в окружение машины вставлен сгенерированный файл, в нём хранится сертификат, пибличный ключ и настройки для конфига.

  <img src="https://user-images.githubusercontent.com/56927592/192849146-01d9e7ac-772f-43bb-b862-c12d4e9ff7ea.png" width="400" height="100" />
  
  Далее был выбран сертификат безопасности.
  
  <img src="https://user-images.githubusercontent.com/56927592/192849655-ed20351a-9ee4-4389-abdd-fcc2ba69c242.png" width="400" height="100" />
  
  Последним шагом была настройка интерфейса, который бы смотрел в VPN туннель.
  
  <img src="https://user-images.githubusercontent.com/56927592/192849940-cb4bad1e-2b05-4417-8290-6c8c2f5d02cb.png" width="400" height="300" />
  
  Ни рисунке ниже представлена проверка установившегося соединения между сервером на убунту и клиентом на микротике

  <img src="https://user-images.githubusercontent.com/56927592/192850454-42240316-6f9d-4db4-ba62-3cb4ed1be7dc.png" width="600" height="100" />

### Вывод
В ходе выполнения лабораторной работы linux система, находящуюся в облаке, была настроена в качесвте openVPN сервера, а RouterOS, находящийся в WirtualBox, был настроен в качестве openVPN клиента, и между ними был установлен vpn туннель. 
