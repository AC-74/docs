Создайте [триггер для {{ container-registry-name }}](../../functions/concepts/trigger/cr-trigger.md), который будет вызывать [функцию](../../functions/concepts/function.md) {{ sf-name }} при создании и удалении [Docker-образов](../../container-registry/concepts/docker-image.md) {{ container-registry-name }} или их тегов.

## Перед началом работы {#before-you-begin}

{% include [trigger-before-you-begin](trigger-before-you-begin.md) %}

* [Реестр](../../container-registry/concepts/registry.md), при событиях с Docker-образами в котором триггер будет запускаться. Если у вас нет реестра, [создайте его](../../container-registry/operations/registry/registry-create.md).

## Создать триггер {#trigger-create}

{% include [trigger-time](trigger-time.md) %}

{% list tabs %}

- Консоль управления

    1. В [консоли управления]({{ link-console-main }}) перейдите в каталог, в котором хотите создать триггер.

    1. Выберите сервис **{{ sf-name }}**.

    1. На панели слева выберите ![image](../../_assets/functions/triggers.svg) **Триггеры**.

    1. Нажмите кнопку **Создать триггер**.

    1. В блоке **Базовые параметры**:

        * Введите имя и описание триггера.
        * В поле **Тип** выберите **{{ container-registry-name }}**.
        * В поле **Запускаемый ресурс** выберите **Функция**.

    1. В блоке **Настройки {{ container-registry-name }}**:

        * В поле **Реестр** выберите реестр, для событий с образами в котором необходимо создать триггер.
        * В поле **Типы событий** выберите [события](../../functions/concepts/trigger/cr-trigger.md#event), после наступления которых триггер будет запускаться.
        * (опционально) В поле **Имя Docker-образа** введите имя образа для [фильтрации](../../functions/concepts/trigger/cr-trigger.md#filter). Чтобы узнать имя Docker-образа, [получите список Docker-образов в реестре](../../container-registry/operations/docker-image/docker-image-list.md).
        * (опционально) В поле **Тег Docker-образа** введите [тег образа](../../functions/concepts/trigger/cr-trigger.md#filter) для фильтрации.

    1. В блоке **Настройки функции** выберите функцию и укажите:

        {% include [function-settings](function-settings.md) %}

    1. (опционально) В блоке **Настройки повторных запросов**:

        {% include [repeat-request.md](repeat-request.md) %}

    1. (опционально) В блоке **Настройки Dead Letter Queue** выберите очередь Dead Letter Queue и сервисный аккаунт с правами на запись в нее.

    1. Нажмите кнопку **Создать триггер**.

- CLI

    {% include [cli-install](../cli-install.md) %}

    {% include [default-catalogue](../default-catalogue.md) %}

    Чтобы создать триггер, который запускает функцию, выполните команду:

    ```bash
    yc serverless trigger create container-registry \
      --name <имя триггера> \
      --registry-id <идентификатор реестра> \
      --events 'create-image', 'delete-image', 'create-image-tag', 'delete-image-tag' \
      --invoke-function-id <идентификатор функции> \
      --invoke-function-service-account-id <идентификатор сервисного аккаунта> \
      --retry-attempts 1 \
      --retry-interval 10s \
      --dlq-queue-id <идентификатор очереди Dead Letter Queue> \
      --dlq-service-account-id <идентификатор сервисного аккаунта>
    ```

    Где:

    * `--name` — имя триггера.
    * `--registry-id` — [идентификатор реестра](../../container-registry/operations/registry/registry-list.md).
    * `--events` — [события](../../functions/concepts/trigger/cr-trigger.md#event), после наступления которых триггер запускается.
    
    {% include [trigger-cli-param](trigger-cli-param.md) %}

    Результат:

    ```
    id: a1s92agr8mpgeo3kjt48
    folder_id: b1g88tflru0ek1omtsu0
    created_at: "2020-09-08T06:26:22.651656Z"
    name: registry-trigger
    rule:
      container_registry:
        event_type:
        - CONTAINER_REGISTRY_EVENT_TYPE_CREATE_IMAGE
        - CONTAINER_REGISTRY_EVENT_TYPE_DELETE_IMAGE
        - CONTAINER_REGISTRY_EVENT_TYPE_CREATE_IMAGE_TAG
        - CONTAINER_REGISTRY_EVENT_TYPE_DELETE_IMAGE_TAG
        registry_id: crtlds4tdfg12kil77sdfg345ghj
        invoke_function:
          function_id: d4eofc7n0m03lmudsk7y
          function_tag: $latest
          service_account_id: aje3932acd0c5ur7drte
          retry_settings:
            retry_attempts: "1"
            interval: 10s
          dead_letter_queue:
            queue-id: yrn:yc:ymq:{{ region-id }}:aoek49ghmknnpj1ll45e:dlq
            service-account-id: aje3932acd0c5ur7dagp
    status: ACTIVE
    ```
- API

    Создать триггер для {{ container-registry-name }} можно с помощью метода API [create](../../functions/triggers/api-ref/Trigger/create.md).

{% endlist %}

## Проверить результат {#check-result}

{% include [check-result](check-result.md) %}

## См. также {#see-also}

* [Триггер для {{ container-registry-name }}, который запускает контейнер {{ serverless-containers-name }}](../../serverless-containers/operations/cr-trigger-create.md).
