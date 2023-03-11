# Домашнее задание к занятию "8.4 Работа с Roles"

## Подготовка к выполнению
1. Создайте два пустых публичных репозитория в любом своём проекте: vector-role и lighthouse-role.
2. Добавьте публичную часть своего ключа к своему профилю в github.

## Основная часть

Наша основная цель - разбить наш playbook на отдельные roles. Задача: сделать roles для clickhouse, vector и lighthouse и написать playbook для использования этих ролей. Ожидаемый результат: существуют три ваших репозитория: два с roles и один с playbook.

1. Создать в старой версии playbook файл `requirements.yml` и заполнить его следующим содержимым:

   ```yaml
   ---
     - src: git@github.com:AlexeySetevoi/ansible-clickhouse.git
       scm: git
       version: "1.11.0"
       name: clickhouse 
   ```

2. При помощи `ansible-galaxy` скачать себе эту роль.
3. Создать новый каталог с ролью при помощи `ansible-galaxy role init vector-role`.
4. На основе tasks из старого playbook заполните новую role. Разнесите переменные между `vars` и `default`. 
5. Перенести нужные шаблоны конфигов в `templates`.
6. Описать в `README.md` обе роли и их параметры.
7. Повторите шаги 3-6 для lighthouse. Помните, что одна роль должна настраивать один продукт.
8. Выложите все roles в репозитории. Проставьте тэги, используя семантическую нумерацию Добавьте roles в `requirements.yml` в playbook.
9. Переработайте playbook на использование roles. Не забудьте про зависимости lighthouse и возможности совмещения `roles` с `tasks`.
10. Выложите playbook в репозиторий.

11. В ответ приведите ссылки на оба репозитория с roles и одну ссылку на репозиторий с playbook.
* https://github.com/ElenaSovetova/lighthouse-role
* https://github.com/ElenaSovetova/lighthouse-role/blob/main/README.md
* https://github.com/ElenaSovetova/vector-role
* https://github.com/ElenaSovetova/vector-role/blob/main/README.md
---
```yaml
vagrant@vagrant:~/lena/playbook$ ansible-playbook -i inventory/prod.yml site.yml

PLAY [Install nginx] ***************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************
ok: [lighthouse-01]

TASK [NGINX | Install epel-release] ************************************************************************************************************************
ok: [lighthouse-01]

TASK [NGINX | Install NGINX] *******************************************************************************************************************************
ok: [lighthouse-01]

TASK [NGINX | Create general config] ***********************************************************************************************************************
ok: [lighthouse-01]

PLAY [Install lighthouse] **********************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************
ok: [lighthouse-01]

TASK [Lighthouse | install dependencies] *******************************************************************************************************************
ok: [lighthouse-01]

TASK [lighthouse : Include OS Family Specific Variables] ***************************************************************************************************
ok: [lighthouse-01]

TASK [lighthouse : include_tasks] **************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/lighthouse/tasks/precheck.yml for lighthouse-01

TASK [lighthouse : Requirements check | Checking sse4_2 support] *******************************************************************************************
ok: [lighthouse-01]

TASK [lighthouse : Requirements check | Not supported distribution && release] *****************************************************************************
skipping: [lighthouse-01]

TASK [lighthouse : include_tasks] **************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/lighthouse/tasks/params.yml for lighthouse-01

TASK [lighthouse : Set clickhouse_service_enable] **********************************************************************************************************
ok: [lighthouse-01]

TASK [lighthouse : Set clickhouse_service_ensure] **********************************************************************************************************
ok: [lighthouse-01]

TASK [lighthouse : include_tasks] **************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/lighthouse/tasks/install/yum.yml for lighthouse-01

TASK [lighthouse : Install by YUM | Ensure clickhouse repo GPG key imported] *******************************************************************************
changed: [lighthouse-01]

TASK [lighthouse : Install by YUM | Ensure clickhouse repo installed] **************************************************************************************
changed: [lighthouse-01]

TASK [lighthouse : Install by YUM | Ensure clickhouse package installed (latest)] **************************************************************************
changed: [lighthouse-01]

TASK [lighthouse : Install by YUM | Ensure clickhouse package installed (version latest)] ******************************************************************
skipping: [lighthouse-01]

TASK [lighthouse : include_tasks] **************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/lighthouse/tasks/configure/sys.yml for lighthouse-01

TASK [lighthouse : Check clickhouse config, data and logs] *************************************************************************************************
ok: [lighthouse-01] => (item=/var/log/clickhouse-server)
changed: [lighthouse-01] => (item=/etc/clickhouse-server)
changed: [lighthouse-01] => (item=/var/lib/clickhouse/tmp/)
changed: [lighthouse-01] => (item=/var/lib/clickhouse/)

TASK [lighthouse : Config | Create config.d folder] ********************************************************************************************************
changed: [lighthouse-01]

TASK [lighthouse : Config | Create users.d folder] *********************************************************************************************************
changed: [lighthouse-01]

TASK [lighthouse : Config | Generate system config] ********************************************************************************************************
changed: [lighthouse-01]

TASK [lighthouse : Config | Generate users config] *********************************************************************************************************
changed: [lighthouse-01]

TASK [lighthouse : Config | Generate remote_servers config] ************************************************************************************************
skipping: [lighthouse-01]

TASK [lighthouse : Config | Generate macros config] ********************************************************************************************************
skipping: [lighthouse-01]

TASK [lighthouse : Config | Generate zookeeper servers config] *********************************************************************************************
skipping: [lighthouse-01]

TASK [lighthouse : Config | Fix interserver_http_port and intersever_https_port collision] *****************************************************************
skipping: [lighthouse-01]

TASK [lighthouse : Notify Handlers Now] ********************************************************************************************************************

RUNNING HANDLER [lighthouse : Restart Clickhouse Service] **************************************************************************************************
ok: [lighthouse-01]

TASK [lighthouse : include_tasks] **************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/lighthouse/tasks/service.yml for lighthouse-01

TASK [lighthouse : Ensure clickhouse-server.service is enabled: True and state: restarted] *****************************************************************
changed: [lighthouse-01]

TASK [lighthouse : Wait for Clickhouse Server to Become Ready] *********************************************************************************************
ok: [lighthouse-01]

TASK [lighthouse : include_tasks] **************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/lighthouse/tasks/configure/db.yml for lighthouse-01

TASK [lighthouse : Set ClickHose Connection String] ********************************************************************************************************
ok: [lighthouse-01]

TASK [lighthouse : Gather list of existing databases] ******************************************************************************************************
ok: [lighthouse-01]

TASK [lighthouse : Config | Delete database config] ********************************************************************************************************

TASK [lighthouse : Config | Create database config] ********************************************************************************************************

TASK [lighthouse : include_tasks] **************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/lighthouse/tasks/configure/dict.yml for lighthouse-01

TASK [lighthouse : Config | Generate dictionary config] ****************************************************************************************************
skipping: [lighthouse-01]

TASK [lighthouse : include_tasks] **************************************************************************************************************************
skipping: [lighthouse-01]

PLAY [Install Clickhouse] **********************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : Include OS Family Specific Variables] ***************************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : include_tasks] **************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/clickhouse/tasks/precheck.yml for clickhouse-01

TASK [clickhouse : Requirements check | Checking sse4_2 support] *******************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : Requirements check | Not supported distribution && release] *****************************************************************************
skipping: [clickhouse-01]

TASK [clickhouse : include_tasks] **************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/clickhouse/tasks/params.yml for clickhouse-01

TASK [clickhouse : Set clickhouse_service_enable] **********************************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : Set clickhouse_service_ensure] **********************************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : include_tasks] **************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/clickhouse/tasks/install/yum.yml for clickhouse-01

TASK [clickhouse : Install by YUM | Ensure clickhouse repo GPG key imported] *******************************************************************************
changed: [clickhouse-01]

TASK [clickhouse : Install by YUM | Ensure clickhouse repo installed] **************************************************************************************
changed: [clickhouse-01]

TASK [clickhouse : Install by YUM | Ensure clickhouse package installed (latest)] **************************************************************************
skipping: [clickhouse-01]

TASK [clickhouse : Install by YUM | Ensure clickhouse package installed (version 22.3.3.44)] ***************************************************************
ok: [clickhouse-01]

TASK [clickhouse : include_tasks] **************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/clickhouse/tasks/configure/sys.yml for clickhouse-01

TASK [clickhouse : Check clickhouse config, data and logs] *************************************************************************************************
ok: [clickhouse-01] => (item=/var/log/clickhouse-server)
changed: [clickhouse-01] => (item=/etc/clickhouse-server)
changed: [clickhouse-01] => (item=/var/lib/clickhouse/tmp/)
changed: [clickhouse-01] => (item=/var/lib/clickhouse/)

TASK [clickhouse : Config | Create config.d folder] ********************************************************************************************************
changed: [clickhouse-01]

TASK [clickhouse : Config | Create users.d folder] *********************************************************************************************************
changed: [clickhouse-01]

TASK [clickhouse : Config | Generate system config] ********************************************************************************************************
changed: [clickhouse-01]

TASK [clickhouse : Config | Generate users config] *********************************************************************************************************
changed: [clickhouse-01]

TASK [clickhouse : Config | Generate remote_servers config] ************************************************************************************************
skipping: [clickhouse-01]

TASK [clickhouse : Config | Generate macros config] ********************************************************************************************************
skipping: [clickhouse-01]

TASK [clickhouse : Config | Generate zookeeper servers config] *********************************************************************************************
skipping: [clickhouse-01]

TASK [clickhouse : Config | Fix interserver_http_port and intersever_https_port collision] *****************************************************************
skipping: [clickhouse-01]

TASK [clickhouse : Notify Handlers Now] ********************************************************************************************************************

RUNNING HANDLER [clickhouse : Restart Clickhouse Service] **************************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : include_tasks] **************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/clickhouse/tasks/service.yml for clickhouse-01

TASK [clickhouse : Ensure clickhouse-server.service is enabled: True and state: restarted] *****************************************************************
changed: [clickhouse-01]

TASK [clickhouse : Wait for Clickhouse Server to Become Ready] *********************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : include_tasks] **************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/clickhouse/tasks/configure/db.yml for clickhouse-01

TASK [clickhouse : Set ClickHose Connection String] ********************************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : Gather list of existing databases] ******************************************************************************************************
ok: [clickhouse-01]

TASK [clickhouse : Config | Delete database config] ********************************************************************************************************

TASK [clickhouse : Config | Create database config] ********************************************************************************************************

TASK [clickhouse : include_tasks] **************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/clickhouse/tasks/configure/dict.yml for clickhouse-01

TASK [clickhouse : Config | Generate dictionary config] ****************************************************************************************************
skipping: [clickhouse-01]

TASK [clickhouse : include_tasks] **************************************************************************************************************************
skipping: [clickhouse-01]

PLAY [Install Vector] **************************************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************************************
ok: [vector-01]

TASK [vector : Include OS Family Specific Variables] *******************************************************************************************************
ok: [vector-01]

TASK [vector : include_tasks] ******************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/vector/tasks/precheck.yml for vector-01

TASK [vector : Requirements check | Checking sse4_2 support] ***********************************************************************************************
ok: [vector-01]

TASK [vector : Requirements check | Not supported distribution && release] *********************************************************************************
skipping: [vector-01]

TASK [vector : include_tasks] ******************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/vector/tasks/params.yml for vector-01

TASK [vector : Set clickhouse_service_enable] **************************************************************************************************************
ok: [vector-01]

TASK [vector : Set clickhouse_service_ensure] **************************************************************************************************************
ok: [vector-01]

TASK [vector : include_tasks] ******************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/vector/tasks/install/yum.yml for vector-01

TASK [vector : Install by YUM | Ensure clickhouse repo GPG key imported] ***********************************************************************************
changed: [vector-01]

TASK [vector : Install by YUM | Ensure clickhouse repo installed] ******************************************************************************************
changed: [vector-01]

TASK [vector : Install by YUM | Ensure clickhouse package installed (latest)] ******************************************************************************
changed: [vector-01]

TASK [vector : Install by YUM | Ensure clickhouse package installed (version latest)] **********************************************************************
skipping: [vector-01]

TASK [vector : include_tasks] ******************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/vector/tasks/configure/sys.yml for vector-01

TASK [vector : Check clickhouse config, data and logs] *****************************************************************************************************
ok: [vector-01] => (item=/var/log/clickhouse-server)
changed: [vector-01] => (item=/etc/clickhouse-server)
changed: [vector-01] => (item=/var/lib/clickhouse/tmp/)
changed: [vector-01] => (item=/var/lib/clickhouse/)

TASK [vector : Config | Create config.d folder] ************************************************************************************************************
changed: [vector-01]

TASK [vector : Config | Create users.d folder] *************************************************************************************************************
changed: [vector-01]

TASK [vector : Config | Generate system config] ************************************************************************************************************
changed: [vector-01]

TASK [vector : Config | Generate users config] *************************************************************************************************************
changed: [vector-01]

TASK [vector : Config | Generate remote_servers config] ****************************************************************************************************
skipping: [vector-01]

TASK [vector : Config | Generate macros config] ************************************************************************************************************
skipping: [vector-01]

TASK [vector : Config | Generate zookeeper servers config] *************************************************************************************************
skipping: [vector-01]

TASK [vector : Config | Fix interserver_http_port and intersever_https_port collision] *********************************************************************
skipping: [vector-01]

TASK [vector : Notify Handlers Now] ************************************************************************************************************************

RUNNING HANDLER [vector : Restart Clickhouse Service] ******************************************************************************************************
ok: [vector-01]

TASK [vector : include_tasks] ******************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/vector/tasks/service.yml for vector-01

TASK [vector : Ensure clickhouse-server.service is enabled: True and state: restarted] *********************************************************************
changed: [vector-01]

TASK [vector : Wait for Clickhouse Server to Become Ready] *************************************************************************************************
ok: [vector-01]

TASK [vector : include_tasks] ******************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/vector/tasks/configure/db.yml for vector-01

TASK [vector : Set ClickHose Connection String] ************************************************************************************************************
ok: [vector-01]

TASK [vector : Gather list of existing databases] **********************************************************************************************************
ok: [vector-01]

TASK [vector : Config | Delete database config] ************************************************************************************************************

TASK [vector : Config | Create database config] ************************************************************************************************************

TASK [vector : include_tasks] ******************************************************************************************************************************
included: /home/vagrant/lena/playbook/roles/vector/tasks/configure/dict.yml for vector-01

TASK [vector : Config | Generate dictionary config] ********************************************************************************************************
skipping: [vector-01]

TASK [vector : include_tasks] ******************************************************************************************************************************
skipping: [vector-01]

PLAY RECAP *************************************************************************************************************************************************
clickhouse-01              : ok=25   changed=8    unreachable=0    failed=0    skipped=10   rescued=0    ignored=0
lighthouse-01              : ok=30   changed=9    unreachable=0    failed=0    skipped=10   rescued=0    ignored=0
vector-01                  : ok=25   changed=9    unreachable=0    failed=0    skipped=10   rescued=0    ignored=0

```
---

### Как оформить ДЗ?

Выполненное домашнее задание пришлите ссылкой на .md-файл в вашем репозитории.

---
