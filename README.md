# 1c_create_test_zone
Создание тестовой ноды 1С в VMWare

1.	Клонирование средствами VMware или восстановление из бэкапа.
При первом включении, до старта NetworkManager, запускается скрипт set_addr.sh. Скрипт проверяет MAC-адрес виртуальной машины. Если текущего MAC-адреса нет в конфигурационном файле mac_list.txt, то виртуальной машине присваивается новый IP-адрес и имя хоста, согласно роли шаблонного сервера. 
Так же, в зависимости от роли сервера, скрипт производит дополнительные настройки:
         
         Сервер баз данных 1c-db1-test: в pg_hba.conf изменяется адрес , с которого к нему можно подключится.
         Сервер приложений 1c-app1-test: удаляются все настройки кластера 1С и установленные программные лицензии. Включается возможность отладки серверных вызовов 1С.
         Веб-сервер 1c-db1-test: в файлах *.vrd заменен адрес сервера приложений.

После первого старта сервера приложений следует скопировать настройки кластера через менеджер сервиса, в который встроено соответствующее расширении. Команда находится в Администрирование – Тестовая зона – Синхронизация тестовой ноды. Кнопку Синхронизировать следуют нажать два раза: в первый раз создаются базы и рабочие сервера, второй раз назначаются требования функциональности для сервера лицензий.  
 
2.	Обновление баз данных на сервере СУБД. 
Для этого в расширение для менеджера сервиса встроена обработка. Команда запуска находится в Администрирование – Тестовая зона – Синхронизация баз данных. 
По сути она выполняет команды операционной системы и PostgresSQL: ssh, scp, pg_dump, psql и pg_restore. Причем pg_dump, psql и pg_restore выполняются удаленно на серверах СУБД. Для этого на них имеется пользователь 1cfresh, ssh-авторизация происходит по ключу. Т.е. пользователь usr1cv8@1c-fresh1 подключается по ssh или scp к 1cfresh@1c-db1 и 1cfresh@1c-db1-test.


