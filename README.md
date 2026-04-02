# Мультидоменная сеть с OSPF, MPLS, DHCP и автоматизацией (GNS3)

## 📌 Описание проекта
Смоделирована сеть, состоящая из двух OSPF-доменов (каждый с Area 0 + 2 доп зонами).  
Анонсирование маршрутов- NAT и редистрибуция статических маршрутов через route-map.  
В LAN-сегменте поднят DHCP-сервер для двух VLAN.  
На всех маршрутизаторах настроен MPLS с LDP.  
С помощью Netmiko собран #show running-config# и информация об OSPF-соседях.


<img width="1919" height="951" alt="Screenshot_8" src="https://github.com/user-attachments/assets/cca70d79-455f-450c-a5a3-a4b495b902f7" />

## 🔧 Использованные технологии
- OSPF (multi-area, redistribution static)
- ACL
- NAT 
- DHCP (ip helper / server per VLAN)
- MPLS + LDP
- VLAN
- Netmiko (Python автоматизация)

## ⚙️ Конфигурация
Все стартовые конфиги в папке  Конфигурация

## 🐍 Автоматизация
Скрипт подключается ко всем роутерам по SSH и сохраняет:
- `show run`
- `show ip ospf neighbor`
- `show mpls ldp neighbor`

## 🧠 Чему я научился
- Редистрибуция статики через #route-map# 
- DHCP relay на субинтерфейсах VLAN
- LDP-соседство в среде с несколькими OSPF-процессами
- Автоматизация сбора конфигураций с помощью Netmiko
