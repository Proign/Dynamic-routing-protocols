# Настройка RIP
Сначала удалим маршрут по умолчанию - должны быть только непосредственно подключенные сети.

Для **2600A** выполним:
```
2600A#config t
2600A(config)#no ip route 0.0.0.0 0.0.0.0 172.16.20.1
2600A(config)#exit
```

То же выполним на **2600B**:
```
2600B#config t
2600B(config)#no ip route 0.0.0.0 0.0.0.0 172.16.30.1
2600B(config)#exit
```

Далее удалим статические маршруты на **2600C**:
```
2600C#config t
2600C(config)#no ip route 172.16.40.0 255.255.255.0 172.16.20.2
2600C(config)#no ip route 172.16.50.0 255.255.255.0 172.16.30.2
2600C(config)#exit
```

Теперь перейдем к настройке RIP, для **2600A** необходимо выполнить:
```
2600A#config t
2600A(config)#router rip
2600A(config-router)#network 172.16.0.0
```

Выполним аналогичную настройку маршрутизатора **2600B**:
```
2600B#config t
2600B(config)#router rip
2600B(config-router)#network 172.16.0.0
```

Произведем аналогичные действия для **2600C**:
```
2600C#config t
2600C(config)#router rip
2600C(config-router)#network 172.16.0.0
```

Перейдем к проверке RIP маршрутизации.

# Проверка RIP маршрутизации
Выведем IP-таблицы, обращая внимания на записи, помеченные R. Они - результат работы протокола RIP.

## 2600A
![2600A ip route](https://github.com/Proign/Dynamic-routing-protocols/blob/main/screenshots/2600a-ip-route.PNG)
## 2600B
![2600B ip route](https://github.com/Proign/Dynamic-routing-protocols/blob/main/screenshots/2600b-ip-route.PNG)
## 2600C
![2600C ip route](https://github.com/Proign/Dynamic-routing-protocols/blob/main/screenshots/2600c-ip-route.PNG)

## Отладка
На 2600A используем команду `debug ip rip` чтобы увидеть апдейты принимаемые и посылаемые маршрутизатором:
![2600A debug ip rip](https://github.com/Proign/Dynamic-routing-protocols/blob/main/screenshots/2600a-debug-ip-rip.PNG)

Выключите отладку с помощью `no debug ip rip` или `undebug all`.

## Состояние таймеров протокола
Состояние таймеров протокола можно увидеть с помощью `show ip protocols`:
![2600A ip protocols](https://github.com/Proign/Dynamic-routing-protocols/blob/main/screenshots/2600a-ip-protocols.PNG)

Обратим внимание, что RIP посылает обновления каждые 30 сек, а его административное расстояние равно 120.

## Конфигурирование RIPv2 маршрутизации
Для включения возможностей второй версии RIP, нужно выполнить команду `version 2`.

Настройка RIPv2 на **2600A**:
```
2600A#config t
2600A(config)#router rip
2600A(config-router)#version 2
```

Настройка RIPv2 на **2600B**:
```
2600B#config t
2600B(config)#router rip
2600B(config-router)#version 2
```

Настройка RIPv2 на **2600C**:
```
2600C#config t
2600C(config)#router rip
2600C(config-router)#version 2
2600C(config-router)#^z
```

# Настройка OSPF
Для настройки OSPF вместо RIP на всех маршрутизаторах в Area 0, выполним следующие шаги на каждом маршрутизаторе:

**2600A**:
```
2600A#config t
2600A(config)#no router rip
2600A(config)#exit
```

**2600B**:
```
2600B#config t
2600B(config)#no router rip
2600B(config)#exit
```

**2600C**:
```
2600C#config t
2600C(config)#no router rip
2600C(config)#exit
```

Теперь перейдем к настройке OSPF.

Настройка OSPF на маршрутизаторе **2600A**:
```
2600A#config t
2600A(config)#router ospf 1
2600A(config-router)#network 172.16.0.0 0.0.255.255 area 0
2600A(config-router)#exit
2600A(config)#exit
```

Настройка OSPF на маршрутизаторе **2600B**:
```
2600B#config t
2600B(config)#router ospf 1
2600B(config-router)#network 172.16.0.0 0.0.255.255 area 0
2600B(config-router)#exit
2600B(config)#exit
```

Настройка OSPF на маршрутизаторе **2600C**:
```
2600C#config t
2600C(config)#router ospf 1
2600C(config-router)#network 172.16.0.0 0.0.255.255 area 0
2600C(config-router)#exit
2600C(config)#exit
```

Проверим таблицу маршрутов на каждом маршрутизаторе:

**2600A**:

![2600A ip protocols](https://github.com/Proign/Dynamic-routing-protocols/blob/main/screenshots/2600a-ospf-route.PNG)

**2600B**:

![2600B ip protocols](https://github.com/Proign/Dynamic-routing-protocols/blob/main/screenshots/2600b-ospf-route.PNG)

**2600C**:

![2600C ip protocols](https://github.com/Proign/Dynamic-routing-protocols/blob/main/screenshots/2600c-ospf-route.PNG)
