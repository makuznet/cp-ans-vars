# Confluent Platform: cp-ansible: vars
> Изменение параметров при установке Confluent Platform средствами дистрибутива [cp-ansible](https://github.com/confluentinc/cp-ansible).

## Переменные
Согласно [руководству для разработчиков](https://github.com/confluentinc/cp-ansible/blob/6.2.0-post/DEVELOPMENT_GUIDE.md#variables), параметры установки определяются как переменные и делятся на ролевые и глобальные. Переменные располагаются соответственно в `roles/role/default/main.yml` и `roles/confluent.variables`.

Именование переменных следует осуществлять, добавляя имя после названий компонентов кластера через нижнее подчеркивание.  
```
### Boolean to enable TLS encryption on Kafka jolokia metrics
kafka_broker_jolokia_ssl_enabled: "{{ ssl_enabled }}"
```

Разработчикам предписано помещать глобальные переменные,   
которые пользователи могут изменить в:  
[confluent.variables/defaults/main.yml](https://github.com/confluentinc/cp-ansible/blob/6.2.0-post/roles/confluent.variables/defaults/main.yml)  

которые пользователи не должны менять в:  
[confluent.variables/vars/main.yml](https://github.com/confluentinc/cp-ansible/blob/6.2.0-post/roles/confluent.variables/vars/main.yml)  

После добавления изменяемых ролевых и глобальных переменных разработчикам предписано запускать [doc.py](https://github.com/confluentinc/cp-ansible/blob/6.2.0-post/doc.py) — скрипт, который обходит все роли и создает документ [VARIABLES.md](https://github.com/confluentinc/cp-ansible/blob/6.2.0-post/VARIABLES.md), содержащий все изменяемые переменные и их значения по-умолчанию.

## Изменение переменных
Согласно [руководству по использованию cp-ansible](https://docs.confluent.io/ansible/current/ansible-configure.html#configure-ansible), переменные, изменение которых необходимо, рекомендуется располагать в inventory-файле — hosts.yml или создавать файлы в папках `group_vars` или `host_vars` согласно [Ansible: Directory Layout](https://docs.ansible.com/ansible/2.8/user_guide/playbooks_best_practices.html#directory-layout).  
```yaml
all:
  vars:
   ssl_enabled: true
```
```yaml
kafka_broker:
  hosts:
    ip-192-24-10-207.us-west.compute.internal:
      broker_id: 0
```
В репозитории есть файл [hosts_example.yml](https://github.com/confluentinc/cp-ansible/blob/6.2.0-post/hosts_example.yml), в котором закомментированы часто используемые переменные, включая, например, выбор метода установки — свой репозиторий или архив.

Если переменной нет в файле [VARIABLES.md](https://github.com/confluentinc/cp-ansible/blob/6.2.0-post/VARIABLES.md), можно воспользоваться изменением параметров конфигурационных файлов компонентов кластера через словари `custom properties`, добавляя их в `hosts.yml`.  

Пример:  
```yaml
all:
  vars:
    zookeeper_custom_properties:
      initLimit: 6
      syncLimit: 3
    kafka_broker_custom_properties:
      num.io.threads: 15
```
Словари:  
- zookeeper_custom_properties  
- kafka_broker_custom_properties  
- schema_registry_custom_properties  
- kafka_rest_custom_properties  
- kafka_connect_custom_properties  
- ksql_custom_properties    
- kafka_connect_replicator_custom_properties  
- kafka_connect_replicator_consumer_custom_properties  
- kafka_connect_replicator_producer_custom_properties  
- kafka_connect_replicator_monitoring_interceptor_custom_properties  

## Дополнительно
Чтобы установить community версию кластера, необходимо добавить:  
```yaml
all:
  vars:
    confluent_server_enabled: false
```    

 ## Ссылки
 - [Confluent: Configure Ansible Playbooks for Confluent Platform](https://docs.confluent.io/ansible/current/ansible-configure.html)  
 - [Ansible: Directory Layout](https://docs.ansible.com/ansible/2.8/user_guide/playbooks_best_practices.html#directory-layout)  
 - [GitHub: cp-ansible repo](https://github.com/confluentinc/cp-ansible)  
 - [Confluent: Migrate to Confluent Server](https://docs.confluent.io/platform/current/installation/migrate-confluent-server.html#migrate-to-cs)  
