###### University: [ITMO University](https://itmo.ru/ru/)
###### Faculty: [FICT](https://fict.itmo.ru)
###### Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)
###### Year: 2022/2023
###### Group: K34212
###### Author: Mikhailov Ivan Yuryevich
###### Lab: Lab3
###### Date of create: 30.10.2022
###### Date of finished: .10.2022
***
# Развертывание Netbox, сеть связи как источник правды в системе технического учета Netbox
### Задачи работы:
1. Поднять Netbox на дополнительной VM.
2. Заполнить всю возможную информацию о ваших CHR в Netbox.
3. Используя Ansible и роли для Netbox в тестовом режиме сохранить все данные из Netbox в отдельный файл, результат приложить в отчёт.
4. Написать сценарий, при котором на основе данных из Netbox можно настроить 2 CHR, изменить имя устройства, добавить IP адрес на устройство.
5. Написать сценарий, позволяющий собрать серийный номер устройства и вносящий серийный номер в Netbox.

### Ход Работы
1. Установка и настройка netbox.
 Перед непосредственной установкой netbox необходимо сначала установить базу данных PostgreSQL. В ней создана база netbox и юзер.
 
 <img src="https://user-images.githubusercontent.com/56927592/198874933-de628acc-d94f-4352-b1da-0ad9d66e4a12.png" width="400" height="200" />
 
В целом вся настройка проходила по гайду: https://www.vultr.com/docs/install-and-configure-netbox-on-ubuntu-20-04/. 

 <img src="https://user-images.githubusercontent.com/56927592/198881832-2cfde3b2-3019-43d9-960a-63380c9c5cd5.png" width="700" height="560" />
 
2. Заполнениие данных в Netbox.
 Подключение происходит по IP виртуальной машины по порту 8001.
 Вручную введены данные о наших 2 устройствах, включая все существующие интерфейсы и один лишний, который потребуется в 4 задании.

<img src="https://user-images.githubusercontent.com/56927592/199251259-6bde7f95-c2ae-4f1f-8194-3986ad7b320b.png" width="700" height="250" />

<img src="https://user-images.githubusercontent.com/56927592/199725795-7b77650f-2595-4512-bf51-7e9a666fddd7.png" width="700" height="250" />

3. Сохранение данных из Netbox в виртуальной машине.
Для получения информации создан netbox.yml файл, использующий для доступа к данным плагин netbox.netbox.nb_inventory.

<img src="https://user-images.githubusercontent.com/56927592/199725367-7585ffd1-418e-4ccd-9acb-bb0b7dae1dbd.png" width="400" height="150" />

С его помощью мы получаем данные, а с помощью файла get_info_playbook.yml сохраняем данные о каждом девайсе в отдельные файлы.

<img src="https://user-images.githubusercontent.com/56927592/199725064-24ff1e62-aa3d-4079-aef2-0413dfa7f664.png" width="500" height="250" />

Данные успешно спарсены.

<img src="https://user-images.githubusercontent.com/56927592/199725473-43607483-a5d1-4e2d-919f-44c6ca9ba10e.png" width="700" height="350" />

4. Изменение имени и добавление IP адреса  на устройствах, использую только что спарсенные данные.

Для каждого устройства в host_vars создана перменная filename с путём до файла с его данными.

<img src="https://user-images.githubusercontent.com/56927592/199741322-b623055c-b6b6-4273-8ff4-2823c33cf612.png" width="300" height="100" />

Написан плейбук устанавливающий данные. В нём создаётся глобальная переменная input, в которую с помощью команды lookup записывается данные из переменной filename. Далее таски разделены на геты и сеты. Доступ к данным происходит через параметры переменной input.

<img src="https://user-images.githubusercontent.com/56927592/199725585-5e329d6c-f9cc-419a-b06e-e00ddb791cbf.png" width="500" height="350" />

На клиентах можно увидеть соответсвующие изменения.

<img src="https://user-images.githubusercontent.com/56927592/199724727-a6eb74ac-d1bc-4406-8790-46a0d7cf388b.png" width="500" height="250" />

5. Передача данных из устройств в нетбокс.

С помощью gather_subset из плагина facts получена информация о серийных номерах роутеров.

<img src="https://user-images.githubusercontent.com/56927592/199730469-765afd07-ebd5-459c-88fc-f1055180d91c.png" width="300" height="100" />

К сожалению, виртуальные Mikrotik не имеют серийного номера.
Поэтому, для добавления информации в NetBox была выбрана версия ПО (ansible_net_version). 
Для этого в нетбокс был создан Custom field OS_version.
Передача данных происходит по плагину netbox_device.

<img src="https://user-images.githubusercontent.com/56927592/199736746-cc2904b1-7fa1-4431-9844-7d777e01fb26.png" width="500" height="500" />

Плейбук успешно выполнился и в нетбокс появилась новая строка с заполненной информацией о версии ОС.

<img src="https://user-images.githubusercontent.com/56927592/199736577-9ca8cb7e-df04-4818-85b0-f7b55d7535d2.png" width="800" height="500" />

### Вывод
В ходе выполнения лабораторной работы был поднят нетбокс - гибкий инструмент для документирования сетей. Выполнена демонстративная документация нашей маленькой сети, а так же мы глубже погрузились в инструментарий ансибла.
