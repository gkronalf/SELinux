# Диагностика проблемы и модифиция политики SELinux для корректной работы приложений
  
## Описание домашнего задания
  Запустить nginx на нестандартном порту 3-мя разными способами:
  -  переключатели setsebool;  
  -  добавление нестандартного порта в имеющийся тип;  
  -  формирование и установка модуля SELinux.
К сдаче:
README с описанием каждого решения (скриншоты и демонстрация приветствуются).  
  
1. Разрешение запуска с нестандартным портом используя утилиту setsebool  
   
  Для начала проверим, что в ОС отключен файервол: systemctl status firewalld  
<image src="./screens/setsebool/systemctl_status_firewalld.jpg" alt="systemctl_status_firewalld">  
  Проверяем, что конфигурация nginx настроена без ошибок: nginx -t  
<image src="./screens/setsebool/nginx_t.jpg" alt="nginx_t.jpg">  
Проверим режим работы SELinux: getenforce  
  <image src="./screens/setsebool/getenforce.jpg" alt="getenforce.jpg">  
  С помощью утилиты audit2why находим событие о блокировке порта и включаем параметр nis_enabled
  <image src="./screens/setsebool/nis_enabled.jpg" alt="nis_enabled.jpg">
  После применения данной команды и перезапуска nginx, видно, что пследний запустился  
  
  
2. Разрешение запуска с нестандартным портом используя semanage  
  
  Находим имеющийся тип для http трафика и добавляем порт в тип http_port_t, после чего перезапускаем nginx
  <image src="./screens/semanage/semanage.jpg" alt="semanage.jpg">  

3. Разрешение c помощью формирования и установки модуля SELinux  
  
  Попробуем снова запустить nginx: systemctl start nginx  
  И посмотрим логи SELinux, которые относятся к nginx  
  <image src="./screens/semodule/nginx_status.jpg" alt="nginx_status.jpg">  
  Воспользуемся утилитой audit2allow для того, чтобы на основе логов SELinux сделать модуль, разрешающий работу nginx на нестандартном порту:
  <image src="./screens/semodule/create_module.jpg" alt="create_module.jpg">  
  на скрине видно, что Audit2allow сформировал модуль, и сообщил нам команду, с помощью которой можно применить данный модуль: semodule -i nginx.pp  
  что и было сделано.  
   
  На мой взгляд самым правильным решение проблемы с блокировкой порта является использование модуля, так как эти изменения будут работатать и после перезагрузки сервера.  
  Необходимые изменения внесены в блок SHELL Vagrantfile и полсе запуска стенда можно проверить, что nginx использует порт 4881, запустив браузер и перейти по адресу http://localhost:4881



