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
  


