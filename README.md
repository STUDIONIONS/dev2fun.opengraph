# Open Graph - быстро, легко и автоматически
Модуль который добавляет поддержку протокола [Open Graph](http://ogp.me/) для 1С-Битрикс.

## Что делает модуль

##### Модуль автоматически добавляет:
* og:title - из title страницы
* og:description - из description страницы
* og:url - текущая страница
* og:site_name - название сайта
любое другое свойство вам нужно указать в настройках

##### Также модуль:
* Выводит у элементов и разделов Open Graph поля, для заполнения.
* Позволяет задать общую картинку для Open Graph.
* Кэширует данные, тем самым не нагружая сайт лишними запросами.
* Позволяет добавить страницы-исключения.
* Позволяет задать максимальный размер для og:image и ресайзить картинки "на лету"
* Позволяет задать порядок применения алгоритмов присваивания значений в Open Graph
* Позволяет легко настроить Open Graph

##### Присутствуют настройки
* время жизни кэша
* страницы исключений
* порядок применения алгоритмов
* вывод таба с Open Graph полями в элементах
* вывод таба с Open Graph полями в разделах
* установка картинки, по умолчанию
* включение/выключение авторесайза
* установка ресайза ширины и высоты изображения
* установка алгоритма для ресайза

**Внимание!** *Чтобы увидеть изменения нужно сбрасывать кэш*

## Как пользоваться

* Добавляем нужные нам og поля. Все параметры берутся с офсайта OpenGraph;
* Добавляем страницы исключения;
* Выбираем нужную сортировку алгоритмов;
* Чтобы увидеть изменения нужно сбрасывать кэш.

### Как добавить og:image

1. Добавляем "image" в настройках модуля и сохраняем;
1. Указываем картинку по умолчанию;
1. Включаем ресайз и указываем размер (рекомендуем ширину 1024, а высоту 99999);
1. Добавить вызов модуля в коде (ниже)

### Порядок применения алгоритмов
Порядок применения алгоритмов - вы можете указать какой порядок присваивания данных для вас является наиболее приемлемым.

##### По умолчанию порядок таков:
* og_fields (Данные из полей OpenGraph элементов/разделов) - поиск значений в полях во вкладке OpenGraph у элементов и разделов
* iblock_fields (Данные из полей элементов/разделов) - пустые значения заполняются данными из полей iblock у элементов и разделов
* prop_fields (Данные из SetPageProperty/SetDirProperty) - пустые значения заполняются данными из указанных свойствах страниц (SetPageProperty или SetDirProperty)
* default (Данные из алгоритма "по умолчанию") - оставшиеся пустые значения заполняются данными по старому алгоритму

### Уменьшение размеров (resize)
Уменьшение размеров (resize) - вы можете включить автоматическое уменьшение размеров картинок. Также вы можете указать максимальную ширину/высоту и выбрать 1 из 3 стандартных алгоритмов ресайза.

### Картинка по умолчанию
Картинка по умолчанию - указывается в админке. Выводится при отсутствии картинки в свойства og:image.

### Страницы исключений
Страницы исключений - пути до страниц от корня сайта, на которых мета-данные Open Graph выводиться не должны. Указывать со всеми расширениями, без GET-параметров. Если нужно исключить главную страницу сайта, то прописываем index.

### Поля свойств
Обозначение свойств в админке у элементов и разделов - на странице редактирования элементов тебе можно заполнять Open Graph поля. (Вкладка "Настройки Open Graph")

* og:title: Если не задан, то подставится значение из "Названия элемента" (NAME).
* og:description: Если не задан, то подставится значение из "Краткое описание" (PREVIEW_TEXT), если оно путо, то значение из "Детальное описание" (DETAIL_TEXT). Внимание: Автоматическое ограничение кол-ва символов в 121 символ.
* og:image: Если у вас указана активность параметра og:image и вы не укажите картинку для og:image, то по умолчанию подставится картинка из PREVIEW_PICTURE, если она не указана, то подставится картинка из DETAIL_PICTURE.

Свойства, которые не доступны для пользовательского редактирования:
* og:url
* og:site_name
* og:image:type
* og:image:width
* og:image:height

Все остальные свойства указываются в настройках и выводятся автоматически.

## Интеграция

### Как вывести OpenGraph у элементов:
Вам необходимо прописать в result_modifier.php шаблона используемого компонента следующее:
```php
// подгружаем все модуль и все его классы
\Bitrix\Main\Loader::includeModule('dev2fun.opengraph');
\Dev2fun\Module\OpenGraph::Show($arResult['ID'],'element'); 
// где arResult['ID'] - идентификатор элемента
// где element - тип, т.к. мы выводим для элемента, поэтому element
```

### Как вывести OpenGraph у разделов:
Вам необходимо прописать в result_modifier.php шаблона используемого компонента следующее:
```php
// подгружаем все модуль и все его классы
\Bitrix\Main\Loader::includeModule('dev2fun.opengraph');
\Dev2fun\Module\OpenGraph::Show($arResult['ID'],'section'); 
// где arResult['ID'] - идентификатор раздела
// где section - тип, т.к. мы выводим для раздела, поэтому section
```

### Поддерживаемые события

|  название события | передаваемые переменные  | описание 
|---|---|---| 
| OnBeforeAddOpenGraph |  &$arSettings - массив настроек<br>&$cache_id - идентификатор кэша  | Событие запускается перед запуском всех алгоритмов  |
 | OnAfterAddOgFields |  &ogValues - массив полей со значениями  | Событие запускается после алгоритма  og_fields  |
 | OnAfterAddIBlockFields |  &ogValues - массив полей со значениями  | Событие запускается после алгоритма  iblock_fields  |
 | OnAfterAddPropFields |  &ogValues - массив полей со значениями  | Событие запускается после алгоритма  prop_fields  |
 | OnAfterAdd |  &ogValues - массив полей со значениями  | Событие запускается после всех алгоритмов  |
 | OnBeforeOutput |  &$arData - массив полей со значениями  | Событие запускается перед выводом свойств (<b>не кэшируется!</b>)  |

## Шаги первой установки

1. Узнать кодировку своего сайта (utf8 или win1251)
1. Скопировать папку dev2fun.opengraph из папки нужной кодировки в папку bitrix/modules
1. Перейти в админку сайта на страницу "Решения Маркетплейс" (/bitrix/admin/partner_modules.php)
1. Установить модуль
1. Прочитать сообщение
1. Зайти в папку migrations и поочередно запустить каждую миграцию
1. Настроить модуль
1. Сбросить кэш
1. Поблагодарить автора :)
1. Использовать

## Шаги обновлений
Т.к. у не активных лицензии updater битрикса не доступен, то нужно проделать след. шаги.

1. Узнать кодировку своего сайта (utf8 или win1251)
1. Скопировать папку dev2fun.opengraph из папки нужной кодировки в папку bitrix/modules
1. Зайти в папку migrations и запустить нужные миграции
1. Сбросить кэш
1. Поблагодарить автора :)
1. Использовать

## Donate

|   |  |
| ------------- | ------------- |
| Yandex.Money  | 410011413398643  |
| Webmoney WMR (rub)  | R218843696478  |
| Webmoney WMU (uah)  | U135571355496  |
| Webmoney WMZ (usd)  | Z418373807413  |
| Webmoney WME (eur)  | E331660539346  |
| Webmoney WMX (btc)  | X740165207511  |
| Webmoney WML (ltc)  | L718094223715  |
| Webmoney WMH (bch)  | H526457512792  |
| PayPal  | [@darkfriend](https://www.paypal.me/darkfriend)  |
| Payeer  | P93175651  |
| Bitcoin  | 15Veahdvoqg3AFx3FvvKL4KEfZb6xZiM6n  |
| Litecoin  | LRN5cssgwrGWMnQruumfV2V7wySoRu7A5t  |
| Ethereum  | 0xe287Ac7150a087e582ab223532928a89c7A7E7B2  |
| BitcoinCash  | bitcoincash:qrl8p6jxgpkeupmvyukg6mnkeafs9fl5dszft9fw9w  |
