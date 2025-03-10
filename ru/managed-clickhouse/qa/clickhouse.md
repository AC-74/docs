# Вопросы о {{ CH }}

#### Почему стоит использовать {{ CH }} в {{ mch-short-name }}, а не собственную установку на виртуальной машине? {#clickhouse-advantages-vm}

{{ mch-short-name }} автоматизирует рутинное обслуживание БД:

* быстрое развертывание БД с необходимыми доступными ресурсами;

* резервное копирование данных;

* регулярное обновление ПО;

* обеспечение отказоустойчивости кластеров БД;

* мониторинг и статистика использования БД.

#### Когда стоит использовать {{ CH }} вместо {{ PG }}? {#clickhouse-advantages-pg}

{{ CH }} поддерживает только добавление и чтение данных, так как предназначен прежде всего для аналитики (OLAP). В остальных случаях, скорее всего, удобнее использовать {{ PG }}.

#### Как загружать данные в {{ CH }}? {#load-data}

Используйте запрос `INSERT`, описанный в [документации {{ CH }}]({{ ch.docs }}/sql-reference/statements/insert-into/).

#### Как загрузить в {{ CH }} очень большое количество данных? {#loadalot}

Используйте [CLI]({{ ch.docs }}/interfaces/cli/) для эффективного сжатия данных при передаче (рекомендуемая частота — не больше 1 команды `INSERT` в секунду).

Перенос данных с физических носителей пока не поддерживается.

#### Что случится с кластером, если выйдет из строя одна из нод? {#node-out}

Кластеры БД состоят минимум из 2 реплик, поэтому при потере одной ноды кластер продолжит работу.

Данные могут потеряться только если вышла из строя нода с [нереплицируемой таблицей]({{ ch.docs }}/engines/table-engines/mergetree-family/replication/).

#### Можно ли развернуть кластер БД {{ CH }} в нескольких зонах доступности? {#multiple-az}

Да. Кластер БД может состоять из хостов, расположенных как в разных зонах, так и в разных регионах доступности.

#### Как устроена репликация для {{ CH }}? {#zookeeper-access}

Кластеры {{ mch-short-name }} используют репликацию с помощью {{ CK }} или {{ ZK }}. В первом случае никаких дополнительных настроек не требуется — репликация и отказоустойчивость включены по умолчанию. Во втором для каждого кластера {{ CH }} создается кластер {{ ZK }} минимум из трех хостов.

Для пользователей {{ yandex-cloud }} доступ к {{ ZK }} и его настройка недоступны.

#### Почему кластер {{ CH }} занимает на 3 хоста больше, чем должен? {#why-does-a-cluster-take-up-3-hosts-more-than-it-should}

При создании кластера {{ CH }} из 2 и более хостов {{ mch-short-name }} автоматически создает кластер из 3 хостов {{ ZK }} для управления репликацией и отказоустойчивостью, если не включена поддержка {{ CK }}. Эти хосты учитываются в расчете использованной [квоты ресурсов]({{ link-console-quotas }}) в облаке и в расчете стоимости кластера. По умолчанию хосты {{ ZK }} создаются с минимальным [классом хостов](../concepts/instance-types.md).

Подробнее об использовании {{ ZK }} см. [документацию ClickHouse]({{ ch.docs }}/engines/table-engines/mergetree-family/replication).

#### Как происходит удаление данных по TTL в {{ CH }} {#how-ttl-data-processing-works}

Удаление данных по [TTL]({{ ch.docs }}/engines/table-engines/mergetree-family/mergetree/#mergetree-table-ttl) выполняется не построчно, а либо целыми [кусками]({{ ch.docs }}/engines/table-engines/mergetree-family/mergetree/#table_engine-mergetree-multiple-volumes) (data parts), либо при операциях слияния.

Удаление целыми кусками работает эффективней и потребляет меньше ресурсов сервера, но для этого значение выражения TTL и [ключ партиционирования]({{ ch.docs }}/engines/table-engines/mergetree-family/custom-partitioning-key/) для всех строк куска данных TTL должны совпадать или хотя бы быть одного порядка.

Удаление при операциях слияния потребляет больше ресурсов и выполняется либо вместе с обычными фоновыми операциями слияния, либо во время внеплановых слияний. Периодичность операций слияния определяется значением параметра `merge_with_ttl_timeout`. Этот параметр задается при [создании]({{ ch.docs }}/engines/table-engines/mergetree-family/mergetree/#table_engine-mergetree-creating-a-table) таблицы и указывает минимальное время в секундах перед повторным слиянием для обработки данных с истекшим TTL. По умолчанию — 14400 секунд (4 часа).

Рекомендуется организовывать обработку данных по TTL так, чтобы старые данные всегда удалялись целыми кусками. Для этого при создании таблиц установите для настройки [ttl_only_drop_parts]({{ ch.docs }}/operations/settings/settings/#ttl_only_drop_parts) значение `true`.
