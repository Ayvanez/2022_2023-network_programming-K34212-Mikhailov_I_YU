###### University: [ITMO University](https://itmo.ru/ru/)
###### Faculty: [FICT](https://fict.itmo.ru)
###### Course: [Network programming](https://github.com/itmo-ict-faculty/network-programming)
###### Year: 2022/2023
###### Group: K34212
###### Author: Mikhailov Ivan Yuryevich
###### Lab: Lab4
###### Date of create: 18.11.2022
###### Date of finished: .11.2022
***
# Базовая 'коммутация' и туннелирование используя язык программирования P4

### Задачи работы:
1. Поднять сконфигурированную виртуальную машину от создателей P4.
2. Implementing Basic Forwarding.
3. Implementing Basic Tunneling.

### Ход Работы
1. Установка виртуальной машины.
Склонирован репозиторий p4lang/tutorials официального репозитория разработчиков p4.
Устаноdkty Vagrant и VirtualBox. В папке tutorials\vm-ubuntu-20.04 находится конфигурауионный файл виртулальной машины в Vagrantфайле. С помощью команд vagrant up и vagrant provision развернута тестовая среда.

 <img src="https://user-images.githubusercontent.com/56927592/204037155-3e0f1870-ad7b-4894-8461-65252bfe921c.png" width="300" height="250" />
 
 2. В папке tutorials/exercises/basic находится описание 1 части работы. Здесь есть скелет программы p4 в файле basic.p4. В нем есть основные разделы программы, наша задача прописать логику отправки сообщений между компьютерами.

Скомпилирован незаконченных файл командой make run. Выполнена проверка что связи нет командами ping и pingall. Для остановки mininet выполнена команда make stop.

<img src="https://user-images.githubusercontent.com/56927592/202663461-0328d850-1383-4c83-8a5b-9973554c6922.png" width="400" height="200" />
 
 Файл дополнен логикой. 
 
 Прописан парсер для заголовков Ethernet и IPv4.
 
 <img src="https://user-images.githubusercontent.com/56927592/202717070-fef0dc35-539a-4003-9560-ddb2bfa42586.png" width="400" height="350" />

Прописано действие отправки пакета IPv4. Указан порт, на который трафик менеджер должен отправлять пакет. Обновляетcя адрес назначения Ethernet адресом следующего прыжка, обновляется исходный адрес Ethernet адресом коммутатора и время жизни пакета уменьшается на 1.

 <img src="https://user-images.githubusercontent.com/56927592/202717911-498ca8c7-6e1e-4798-8af2-b46fd83edd61.png" width="400" height="200" />
 
 Вся эта логика применяется только если пакет имеет заголовок IPv4.

 <img src="https://user-images.githubusercontent.com/56927592/202719061-8384c0e2-1b1f-4e34-9018-3325b169fc21.png" width="400" height="200" />

Выполнен депарсер для каждого заголовка.

 <img src="https://user-images.githubusercontent.com/56927592/202716990-341056d6-28b9-4add-958e-9adbec90456e.png" width="350" height="100" />
 
 Выполнена проверка сообщениями между компьютерами h1 и h2.

 <img src="https://user-images.githubusercontent.com/56927592/202720400-2787817c-5db0-45d3-af80-9f226bb4e773.png" width="700" height="400" />

3. Перешли ко второму упражнению - настройки туннелирования. Используется законценный файл basic.p4, но в него добавлен новый заголовок отвечающий за vpn инкапсуляцию.

 <img src="https://user-images.githubusercontent.com/56927592/202724309-df51302d-62bf-43ce-8d6c-02249f097da6.png" width="400" height="400" />

Добавлено новое состояния для парсинга пакета с заголовком myTunnel. etherType, соответствующий заголовку myTunnel, равен 0x1212. Парсер также извлекает заголовок ipv4 после заголовка myTunnel, если proto_id = TYPE_IPV4 (т.е. 0x0800). Для парсинга пакета ethernet добавлено новое состояние обработки на случай наличия нового заголовка. 

 <img src="https://user-images.githubusercontent.com/56927592/202725547-ea4fffa5-3ac7-4593-a8aa-75d2d4bd4558.png" width="400" height="400" />
 
В логику управления входом добавлена новая таблица myTunnel_exact, определяющая варианты поведения с пакетом с заголовком vpn. Действия отправки пакета создано с нуля и называется myTunnel_exact. Оно обновляет номер порта на который нужно отправить сообщение.
Также обновлена логика управления потоком данных. Если есть заголовок с туннелем, то применяется только что созданная таблица myTunnel_exact, если его нет, а заголовок ipv4 есть, но действие не меняется, применятеся существовавшая таблица ipv4_lpm.

 <img src="https://user-images.githubusercontent.com/56927592/202728732-53661e09-0329-47c4-ac2e-dc50b0686ddd.png" width="400" height="400" />

Логика депарсинга изменена на случай если пакет будет с vpn заголовком.

 <img src="https://user-images.githubusercontent.com/56927592/202725991-fb5e38e5-b292-4ded-b9c3-1b498325ceef.png" width="400" height="200" />

Проверена передача сообщения по ip адресу между 1 и 2 компьютером.

 <img src="https://user-images.githubusercontent.com/56927592/202732324-4bc4d9f2-72d2-4893-b88a-f501884ab8ad.png" width="800" height="500" />
 
 Проверено отправление сообщение с неправильным ip адресом, но с указанием заголовка mytunnel --dst_id.

 <img src="https://user-images.githubusercontent.com/56927592/202732747-d1a0c8b1-539e-4b86-bec1-d93df1c08f69.png" width="600" height="300" />
 
### Вывод
В ходе выполнения лабортаорной работы выполнены 2 упражнения по настройке сети с помощью языка программирования сети P4. Была настроена адресация по ipv4 протоколу а так же реализовано базовое туннелирования с помощью инкапсуляции сообщения дополнительным заголовком.
