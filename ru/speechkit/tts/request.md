# Описание метода API v1

Генерирует речь по переданному тексту.

{% note info %}

API v1 поддерживает не все возможности синтеза {{ speechkit-name }}. Сравнение версий API см. в разделе [Возможности синтеза](index.md#features).

{% endnote %}

## HTTP-запрос {#http_request}

```
POST https://tts.{{ api-host }}/speech/v1/tts:synthesize
```

### Параметры в теле запроса {#body_params}

Для всех параметров обязательно используйте [URL-кодирование](https://datatracker.ietf.org/doc/html/rfc3986#section-2). Максимальный размер тела POST-запроса 15 КБ.

Параметр | Описание
----- | -----
text | **string**<br>Текст, который нужно озвучить, в кодировке UTF-8.<br>Можно использовать только одно из полей `text` и `ssml`.<br>Для управления произношением (расстановки пауз, акцентов и ударений) используйте [TTS-разметку](tts-markup.md).<br>Ограничение на длину строки: 5000 символов.
ssml | **string**<br>Текст, который нужно озвучить, в [формате SSML](ssml.md).<br>Можно использовать только одно из полей `text` и `ssml`.
lang | **string**<br>Язык.<br/>Допустимые значения: `ru-RU` (по умолчанию) — русский язык.
voice | **string**<br>Желаемый голос для синтеза речи из [списка](voices.md). 
emotion | **string**<br>Амплуа или эмоциональная окраска голоса. Поддерживается только при выборе русского языка (`ru-RU`). Допустимые комбинации голоса и эмоциональной окраски см. в разделе [{#T}](voices.md).
speed | **string**<br>Скорость (темп) синтезированной речи.<br/>Скорость речи задается дробным числом в диапазоне от `0.1` до `3.0`. Где:<ul><li>`3.0` — самый быстрый темп;</li><li>`1.0` (по умолчанию) — средняя скорость человеческой речи;</li><li>`0.1` — самый медленный темп.</li></ul>
format | **string**<br>Формат синтезируемого аудио.<br/>Допустимые значения:<ul><li>`lpcm`</li><li>`oggopus` (по умолчанию)</li> <li>`mp3`</li></ul>
sampleRateHertz | **string**<br>Частота дискретизации синтезируемого аудио.<br/>Применяется, если значение `format` равно `lpcm`. Допустимые значения:<ul><li>`48000` (по умолчанию) — частота дискретизации 48 кГц;</li><li>`16000` — частота дискретизации 16 кГц;</li><li>`8000` — частота дискретизации 8 кГц.</li></ul>
folderId | **string**<br><p>[Идентификатор каталога](../../resource-manager/operations/folder/get-id.md), к которому у вас есть доступ. Требуется для авторизации с пользовательским аккаунтом (см. ресурс [{#T}](../concepts/auth.md)). Не используйте это поле, если вы делаете запрос от имени сервисного аккаунта.</p> <p>Максимальная длина строки в символах — 50.</p>

## Ответ {#response}

Если синтез прошел успешно, в ответе будет бинарное содержимое аудиофайла. Формат выходных данных зависит от значения параметра `format`.

Подробнее о формате и кодах ответов см. на странице [{#T}](../concepts/response.md). 
