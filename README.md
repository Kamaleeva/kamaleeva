Виртуальную машину настроила на 2 ядра процессора и 2 ГБ оперативной памяти. Установлен оракл линукс. Такой конфигурации достаточно для базовых задач, которые я планировала выполнять, и при этом система не будет перегружена. 🖥️
________________________________________
Установка Wget 
Первым делом я установила Wget — это утилита для загрузки файлов из интернета прямо из терминала. Она очень удобна, когда нужно быстро скачать что-то без использования браузера или графического интерфейса. Например, если требуется загрузить архив, скрипт или конфигурационный файл.
`sudo yum install wget`

![image](https://github.com/user-attachments/assets/34b1dd32-9946-41cc-9233-28a712f26f24)


Зачем?
Wget экономит время и упрощает работу, особенно когда доступ к графическому интерфейсу ограничен или недоступен. 
________________________________________
Установка Curl 
Следом я установила Curl — это мощная утилита для работы с различными сетевыми протоколами, такими как HTTP, HTTPS, FTP и другими. Она позволяет отправлять запросы, загружать данные и тестировать API прямо из терминала.
`sudo yum install curl`

![image](https://github.com/user-attachments/assets/682a381a-80fe-4341-acd2-775054ade98f)


Зачем?
Curl незаменим для тестирования веб-сервисов, работы с API или просто для загрузки данных. Например, с её помощью можно быстро проверить, доступен ли сервер, или скачать файл с удалённого ресурса.
________________________________________
Добавление репозитория Docker 
Для установки Docker мне нужно было добавить его официальный репозиторий. Поскольку Oracle Linux основан на CentOS, я использовала репозиторий для CentOS. Это гарантирует, что я получу последнюю стабильную версию Docker.
`sudo wget -P /etc/yum.repos.d/ https://download.docker.com/linux/centos/docker-ce.repo`

![image](https://github.com/user-attachments/assets/ce62dffe-2984-4d14-aab5-82c107ab350a)


Зачем?
Официальный репозиторий обеспечивает доступ к актуальным и проверенным версиям Docker. Это важно для безопасности и стабильности системы. Без него пришлось бы вручную скачивать и устанавливать пакеты, что неудобно и рискованно. 🛡️
________________________________________
Установка Docker 
Теперь самое интересное — установка Docker. Я установила основные компоненты: docker-ce (Community Edition), docker-ce-cli (Command Line Interface) и containerd.io (среда для управления контейнерами).
`sudo yum install docker-ce docker-ce-cli containerd.io`

![image](https://github.com/user-attachments/assets/21b98c25-638a-4b1c-8c05-a00b952a521b)
![image](https://github.com/user-attachments/assets/699969e9-cf18-48b8-8f04-94d9086f206e)


Зачем?
Docker позволяет запускать приложения в изолированных контейнерах, что упрощает разработку, тестирование и развёртывание. Это как виртуальная машина, но легче и быстрее. С Docker можно легко управлять зависимостями и настройками приложений. 🚀
________________________________________
Автозапуск Docker 
Чтобы Docker запускался автоматически при старте системы, я выполнила команду:
`sudo systemctl enable docker --now`

![image](https://github.com/user-attachments/assets/cc4c6ea5-2990-4dab-85f8-3f689f325993)


Зачем?
Это избавляет от необходимости вручную запускать Docker после каждой перезагрузки системы. Теперь он всегда готов к работе, и я могу сосредоточиться на своих задачах, а не на настройке. 
________________________________________
Установка Docker Compose 🛠️
Для управления несколькими контейнерами одновременно необходим Docker Compose. Сначала я определила последнюю версию через API GitHub, чтобы быть уверенной, что устанавливаю актуальную версию.
`COMVER=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | grep 'tag_name' | cut -d\" -f4)`

![image](https://github.com/user-attachments/assets/255ad511-7f59-411d-90f9-23d8f2d1261f)


Затем скачала и установила Docker Compose:
`sudo curl -L "https://github.com/docker/compose/releases/download/$COMVER/docker-compose-$(uname -s)-$(uname -m)" -o /usr/bin/docker-compose`

![image](https://github.com/user-attachments/assets/1409f880-8d41-4ee5-9495-555f74f86a44)


Зачем?
Docker Compose позволяет управлять несколькими контейнерами как единым приложением. Это особенно полезно для сложных проектов, где нужно одновременно запускать базы данных, веб-серверы и другие сервисы. Всё описывается в одном файле, и Docker Compose делает всю работу за вас. 
________________________________________
Настройка прав и проверка версии Docker Compose 
Чтобы Docker Compose можно было использовать, я дала права на выполнение файлу:
`sudo chmod +x /usr/bin/docker-compose`

![image](https://github.com/user-attachments/assets/1049eed7-721e-4b72-bbfc-91723e103428)


Затем проверила версию, чтобы убедиться, что всё установилось правильно:
`docker-compose --version`

Зачем?
Права на выполнение необходимы для того, чтобы система могла запускать Docker Compose как программу. Проверка версии помогает убедиться, что установка прошла успешно и всё работает как надо. Всё под контролем! 👍

Перейдем в директорию grafana_stack_for_docker, где расположены все необходимые файлы для настройки Grafana.

`cd grafana_stack_for_docker`
Для обеспечения порядка и структурированности, создадим необходимые директории для Grafana и её компонентов. Используем команду mkdir -p, которая создаст все промежуточные каталоги, если они отсутствуют.

`sudo mkdir -p /mnt/common_volume/swarm/grafana/config`
Далее создадим дополнительные директории для хранения конфигурации Grafana, её данных, а также данных Prometheus.

`sudo mkdir -p /mnt/common_volume/grafana/{grafana-config,grafana-data,prometheus-data}`
Для обеспечения безопасности и корректной работы, изменим владельца созданных директорий на текущего пользователя. Это выполняется с помощью команды chown.

`sudo chown -R $(id -u):$(id -g) {/mnt/common_volume/swarm/grafana/config,/mnt/common_volume/grafana}`
Для работы Grafana требуется конфигурационный файл grafana.ini. Если файл отсутствует, команда touch создаст пустой файл. Если файл уже существует, команда обновит его временные метки.

`touch /mnt/common_volume/grafana/grafana-config/grafana.ini`
Скопируем все файлы из директории config в созданную директорию для Grafana.

`cp config/* /mnt/common_volume/swarm/grafana/config/`
Переименуем файл grafana.yaml в docker-compose.yaml, чтобы Docker Compose мог его использовать.

`mv grafana.yaml docker-compose.yaml`

![image](https://github.com/user-attachments/assets/a5b2b939-7054-45a8-af84-3630a53fd4a6)


Для завершения настройки и запуска контейнеров в фоновом режиме с использованием Docker Compose, выполним следующую команду:

`sudo docker compose up -d`

![image](https://github.com/user-attachments/assets/5104ce5e-c22d-4840-bf32-8b1a8eb5256f)


Данная команда запускает контейнеры, описанные в файле docker-compose.yml, в фоновом режиме (демонизированном режиме). Ключ -d (от англ. detached) обеспечивает запуск контейнеров без блокировки терминала, что позволяет продолжить работу в командной строке. Это особенно полезно при работе с долго работающими сервисами, такими как веб-серверы или базы данных.

Остановка контейнеров
Для временной остановки всех активных контейнеров, связанных с проектом, используется команда:

`sudo docker compose stop`

![image](https://github.com/user-attachments/assets/da5626bb-b2ca-4637-92a9-ea2df8e324a5)

Эта команда приостанавливает выполнение контейнеров, не удаляя их. Это полезно, например, для технического обслуживания или обновления конфигурации.

Удаление контейнеров и связанных ресурсов
Для полной остановки и удаления контейнеров, сетей и других ресурсов, созданных в рамках проекта, выполните команду:

`sudo docker compose down`

![image](https://github.com/user-attachments/assets/2202585a-b90b-47d6-a377-7bcb137bee4d)

По умолчанию команда не удаляет тома (volumes) и образы (images), если они явно не указаны в параметрах. Это полезно для полной очистки проекта, например, при завершении работы или пересоздании окружения.

Просмотр состояния контейнеров
Для мониторинга состояния контейнеров, относящихся к текущему проекту, используйте команду:

`sudo docker compose ps`

![image](https://github.com/user-attachments/assets/31f4ba06-34bf-4940-b01d-33cbd4971b77)

В выводе отображается состояние каждого контейнера (работает, остановлен и т.д.), а также дополнительная информация, такая как порты и имена контейнеров.

Клонирование репозитория
Для начала работы с проектом, размещённым на GitHub, выполните команду клонирования репозитория:

`git clone https://github.com/kamaleeva/kamaleeva.git`

![image](https://github.com/user-attachments/assets/1d09fb99-1530-4ee0-bbd0-c6ead810c45f)

Эта команда создаёт локальную копию репозитория, включая все файлы и историю изменений, в директории с именем grafana_college.

Определение текущей рабочей директории
Для определения текущей рабочей директории используйте команду:

`pwd`
Команда pwd (от англ. print working directory) выводит полный путь к текущей директории. Это полезно при работе с множеством директорий или выполнении сложных скриптов.

![image](https://github.com/user-attachments/assets/0b531433-e7a2-4940-9466-9207b4e1cc7f)

Меняю с помощью vim файл docker-compose.yaml. Добавляю node-exporter

![image](https://github.com/user-attachments/assets/3803c2bf-6e16-453a-9f28-360f267888fe)

![image](https://github.com/user-attachments/assets/5b356788-130e-40e3-860a-0fe93c3d3df9)



![image](https://github.com/user-attachments/assets/c42b8265-44de-40b6-8b48-771bf2b82720)


Ввожу admin admin и попадаю в графану

![image](https://github.com/user-attachments/assets/ca087021-bdd9-4ccb-b9d5-3d4b7dece0da)

Создаю dashboard

![image](https://github.com/user-attachments/assets/881e7c34-50b0-4300-8ee7-d940d1c6d746)

configure a new data source

![image](https://github.com/user-attachments/assets/f58cdb3d-0bee-40d1-a6ae-d946dba070c6)


prometheus

![image](https://github.com/user-attachments/assets/2805ff2b-8675-4abb-9a87-8ec5c0b82cd6)

![image](https://github.com/user-attachments/assets/30c9bdaf-4c09-4f77-9f9a-12189f309e0c)

![image](https://github.com/user-attachments/assets/68af875a-a05b-40ac-aa03-5c35feb229c0)

![image](https://github.com/user-attachments/assets/f2045bbf-2f0c-4c7a-b2f3-b38a47324f9a)

![image](https://github.com/user-attachments/assets/dd4dc29b-f0de-4193-b78c-ec0ef162e0df)

![image](https://github.com/user-attachments/assets/804f2e0b-6905-4e2e-abeb-80b27dec03c4)

![image](https://github.com/user-attachments/assets/884f9fde-a8f6-49a2-9199-2ef67ea7baa6)
![image](https://github.com/user-attachments/assets/ba5ddb49-c85d-4567-a1c9-0c43689d6b7c)

