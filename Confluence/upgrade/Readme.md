## Обновление версии Confluence DC

* **Скачайте новую подходящую версию:** https://www.atlassian.com/software/confluence/download-archives
* **Необходимо сделать след. обязательные действия:**
   1. сделать бэкап базы данных
   2. сделать бэкап файлов /var/atlassian,/opt/atlassian,/opt/jdk <br>
     ````mkdir /var/atlassian-backup```` <br>
     ````cp -r /var/atlassian/ /var/atlassian-backup```` <br>
     ````mkdir /opt/atlassian-confluence-backup```` <br>
     ````cp -r /opt/atlassian-confluence/ /opt/atlassian-confluence-backup```` <br>
     ````mkdir /opt/jdk-backup```` <br>
     ````cp -r /opt/jdk /opt/jdk-backup````
* **Скачайте новую версию jdk:** https://www.oracle.com/apac/java/technologies/downloads/#java21
* **Распаковка файлов:**
   1. Распакуйте приложение: <br>
      ````unzip atlassian-confluence.zip````
   2. Распакуйте jdk:<br>
      ````tar -xvzf jdk21.tar.gz````

* **Переносим файлы, туда где расположено у нас приложение:** <br>
  ````cp -r /home/adm_afatikhov/jdk/ /opt/```` <br>
  ````cp -r /home/adm_afatikhov/atlassian-confluence/ /opt/````

* **Перенос cacerts:** Если у вас в пред версии джавы есть важные сертификаты, то их можно просто перенести изменив в новой версии <br>
  ``rm -rf /opt/jdk17/lib/security/cacerts`` <br>
  ``rm -rf /opt/jdk21/lib/security/cacerts`` <br>
  ``cp -r /opt/jdk/17/lib/security/cacerts /opt/jdk21/lib/security/cacerts`` <br>
* **Поработайте с файлами:**
   1. Необходимо сравнить файл setenv.sh с пред. версией:
  ``diff setenv.sh /opt/atlassian-confluence-backup/confluence/bin/setenv.sh``
   2. Посмотрите разницу файла /opt/atlassian-confluence/confluence/conf/server.xml
  ``dif server.xml /opt/atlassian-confluence/confluence/conf/server.xml``
   3. Скопируйте файл confluence/WEB-INF/classes/confluence-init.properties из старой версии в новую (чтобы сохранить путь к confluence.home).
   4. Если у вас были настроены jmx-exporter and zabbix-jmx, то посмотрите изменения в файле start-confluence.sh
   
* **Выдайте права на файлы, которые были изменены:**
   1./opt/atlassian-confluence <br>
   ````
     chown -R confluence:confluence /opt/atlassian-confluence
   ````
   ````
     chmod -R 770 /opt/atlassian-confluence 
   ````
   2./opt/jdk21 <br>
   ````
     chown -R confluence:confluence /opt/jdk21
    ```` 
   ````
     chmod -R 770 /opt/jdk21
   ```` 
* **экспортируйте переменную для использования java:**
   ````
       nano ~/.bashrc
   ````
   ````export JAVA_HOME=/usr/local/java/jdk-версия
       export PATH=$PATH:$JAVA_HOME/bin
   ````