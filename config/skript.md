#!/usr/bin/env python3
from netmiko import ConnectHandler
from datetime import datetime
import os
import subprocess

# Список роутеров
routers = [
    {'name': 'router1', 'ip': '10.0.0.1', 'username': 'admin', 'password': 'cisco', 'secret': 'cisco'},
    {'name': 'router2', 'ip': '10.0.0.2', 'username': 'admin', 'password': 'cisco', 'secret': 'cisco'},
    {'name': 'router3', 'ip': '10.0.0.6', 'username': 'admin', 'password': 'cisco', 'secret': 'cisco'},
    {'name': 'router4', 'ip': '10.0.0.14', 'username': 'admin', 'password': 'cisco', 'secret': 'cisco'},
    {'name': 'router5', 'ip': '10.0.0.18', 'username': 'admin', 'password': 'cisco', 'secret': 'cisco'},
    {'name': 'router6', 'ip': '10.0.0.22', 'username': 'admin', 'password': 'cisco', 'secret': 'cisco'},
    {'name': 'router7', 'ip': '10.0.0.26', 'username': 'admin', 'password': 'cisco', 'secret': 'cisco'},
    {'name': 'router8', 'ip': '10.0.0.30', 'username': 'admin', 'password': 'cisco', 'secret': 'cisco'},
    {'name': 'router9', 'ip': '10.0.0.42', 'username': 'admin', 'password': 'cisco', 'secret': 'cisco'},
    {'name': 'router10', 'ip': '10.0.0.41', 'username': 'admin', 'password': 'cisco', 'secret': 'cisco'},
]

# Проверяем и добавляем маршрут если нужно
print("Проверка маршрута к сети 10.0.0.0/24...")
result = subprocess.run(['ip', 'route', 'show', '10.0.0.0/24'], capture_output=True, text=True)
if result.returncode != 0:
    print("Добавляю маршрут через 192.168.2.1...")
    subprocess.run(['ip', 'route', 'add', '10.0.0.0/24', 'via', '192.168.2.1', 'dev', 'eth1'])
    print("Маршрут добавлен")
else:
    print("Маршрут уже существует")

# Создаем папку для вывода
output_dir = f"ospf_mpls_{datetime.now().strftime('%Y%m%d_%H%M%S')}"
os.mkdir(output_dir)
print(f"\nСоздана папка: {output_dir}")
print("=" * 50)

# Команды для сбора (исправлено: добавлена запятая и show running-config)
commands = [
    'show ip ospf neighbor',
    'show mpls forwarding-table',
    'show running-config'          # <-- добавленная команда (можно 'sh run')
]

# Собираем информацию
for r in routers:
    print(f"\nПодключаюсь к {r['name']} ({r['ip']})...")
    
    # Проверяем доступность по ICMP
    response = subprocess.run(['ping', '-c', '1', '-W', '1', r['ip']], capture_output=True)
    if response.returncode != 0:
        print(f"  ✗ Хост {r['ip']} не отвечает на ping")
        continue
    
    try:
        device = {
            'device_type': 'cisco_ios',
            'ip': r['ip'],
            'username': r['username'],
            'password': r['password'],
            'secret': r['secret'],
            'timeout': 10,
        }
        conn = ConnectHandler(**device)
        conn.enable()
        
        # Создаем файл для этого роутера
        filename = f"{output_dir}/{r['name']}_ospf_mpls.txt"
        with open(filename, 'w') as f:
            f.write(f"# Информация с роутера {r['name']}\n")
            f.write(f"# Дата: {datetime.now()}\n")
            f.write(f"# IP: {r['ip']}\n")
            f.write("-" * 50 + "\n\n")
            
            # Выполняем каждую команду
            for cmd in commands:
                print(f"  Выполняю: {cmd}")
                output = conn.send_command(cmd)
                
                # Пишем в файл
                f.write(f"=== {cmd} ===\n")
                f.write(output)
                f.write("\n\n" + "-" * 50 + "\n\n")
                
                # Показываем краткий результат в консоли
                lines = output.strip().split('\n')
                if len(lines) > 1:
                    print(f"    ✓ Получено {len(lines)} строк")
                else:
                    print(f"    ✓ Команда выполнена")
        
        print(f"  ✓ OK - результат сохранен в {filename}")
        conn.disconnect()
       
    except Exception as e:
        print(f"  ✗ ОШИБКА: {e}")

print("\n" + "=" * 50)
print(f"Готово! Результаты в папке {output_dir}")
