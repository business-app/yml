# Документация выгрузок в формате YML

[XSD схема](./schema.xsd)

## Shop

Корневым тэгом в YML всегда является `yml_catalog`. Внутри него всегда должен быть ровно один тэг `shop`  
```xml
<yml_catalog>
<shop>
<!-- CONTENT -->
</shop>
</yml_catalog>
```

### Category

Каждый тэг `category` отвечает за одну категорию товаров в каталоге.  
```xml
<yml_catalog>
<shop>
<categories>
    <category id='1'>Обувь</category>
    <category id='2' parentId='1'>Кеды</category>
</categories>
</shop>
</yml_catalog>
```
Описание полей:  
`Содержание тэга` (String, Обязательное). Название категории
`id` (String, Обязательное). Уникальный идентификатор категории. У двух категорий не может быть одинакового `id`  
`parentId` (String, Необязательное). `id` категории, в которую данная категория входит. В примере у категории `Кеды` в поле `parentId` указан `id` категории `Обувь`. Это означает, что `Кеды` является подкатегрией категории `Обувь`  

#### Важно!
На данный момент, категория не может содержать и подкатегории, и товары. Если категория содержит подкатегории, то все товары из неё будут отображаться в подкатегории `Другое`. Например, если в категории `Обувь` есть подкатегория `Кеды` и товар `Кроссовки Converse`, то в приложении в категории `Обувь` появтся подкатегория `Другое` и товар `Кроссовки Converse` будет отображаться в ней.  

### Offer

Тэг `offer` отвечает за один товар в каталоге  
```xml
<yml_catalog>
<shop>
<offers>
    <offer id="1" group_id="2">
        <name>Кеды Converse</name>
        <description>Подробное описание кед</description>
        <sales_notes>Краткое описание кед</sales_notes>
        <categoryId>1</categoryId>
        <categoryId>2</categoryId>
        <picture>https://site.com/image.png</picture>
        <picture>https://site.com/image2.png</picture>
        <price>100.00</price>
        <measure>unit</measure>
        <vendorCode>AFSA123AFS</vendorCode>
        <param name="Бренд">Converse</param>
        <param name="Пол">Унисекс</param>
        <store id="1" amount="12"/>
        <store id="2" amount="14"/>
    </offer>
</offers>
</shop>
</yml_catalog>
```
Описание полей:  
`id` (String, Обязательное). Уникальный идентификатор товара. У двух товаров не может быть одинакового `id`.  
`group_id` (String, Необязательное). Используется для реализации выборных свойств. Каждый товар с выборными свойствами - это объединение нескольких товаров (отдельный товар для каждого возможного набора свойств). Для того, чтобы выгрузить такой товар, нужно объединить несколько товаров в группу, указав у них одинаковый `group_id`. Все отличающиеся аттрибуты товаров группы (см. [Аттрибут](#Param)) станут выборными свойствами. Если в группе только один товар, то он будет отображаться как обычный товар без выборных свойств.  
`name` (String, Должно быть указано ровно один раз). Название товара в каталоге. Отображается в списке товаров и в карточке товара.  
`description` (String, Необязательное). Подробное описание товара. Отображается в карточке товара. Может содержать произвольный markdown, а также тэги `p`, `br`, `b`, `i`, `h1`, `h2`, `h3`, `h4`, `h5`, `h6` из HTML. Поддерживает локализацию.  
`sales_notes` (String, Необязательное). Краткое описание товара. Отображается в каталоге и карточке товара.  
`categoryId` (String, Необязательное, Можно указать несколько раз). Идентификатор категории, в которой находится товар. Товар может находиться сразу в нескольких категориях (см. [Категория](#Category))  
`picture` (String, Необязательное, Можно указать несколько раз). Картинки товаров. Первая будет отображаться в списке товаров в каталоге. Также первая и все остальные картинки отобразятся в карточке товара.  
`price` (Float, Должно быть указано ровно один раз). Цена товара в рублях. Может быть целым или вещественным числом.
`measure` (Необязательное, Может быть `unit`, `kg`, `gramm`, `lit`, `mlit`, `sec`, `day` или `hour`). Единица измерения, в которых измеряется товар. Например, килограммы (`kg`) для картофеля и штуки (`unit`) – для пакетов молока.  
`vendorCode` (String, Необязательное, Можно указать только один раз). Артикул товара. Приходит как артикул в письме о том, что куплен новый товар.  
`param` ([Param](#Param), Необязательное, Можно указать несколько раз). Аттрибут товара. Список отображается в карточке товара. Так же, по аттрибутам можно фильтровать товары в каталоге.  
`store` ([StoreInOffer](#StoreInOffer), Необязательное, Можно указать несколько раз). Количество товаров в указанном складе (см. [Остатки товара](#Store))  

### Param
Аттрибут товара. Список отображается в карточке товара. Так же, по аттрибутам можно фильтровать товары в каталоге.

Описание полей:  
`name` (String, Обязательное). Название аттрибута. У одного товара не может быть двух аттрибутов, которые одинаково назывваются.  
`Содержание тэга` (String, Обязательное). Значение аттрибута.

### StoreInOffer
Описание полей:  
`id` (String, Обязательное). Идентификатор склада (см. [Склад](#Store))  
`amount` (Int, Обязательное). Количество данного товара в указанном складе.  

### Store
Склад или магазин. Отображается на карте, а так же может использоваться для выгрузки остатков для доставки или самовывоза. 
```xml
<yml_catalog>
<shop>
<sklads>
    <store
        id='1' 
        useForDelivery='true' 
        phone='+7 999 222 33 44'
        address='ул. Пушкина, дом 1'
        lat='56.004456' 
        lon='52.241732' 
        city='1'>
        <work-week>
            <work-day week-day='mo' start='09:00' end='19:00'/>
            <work-day week-day='tu' start='09:00' end='19:00'/>
            <work-day week-day='we' start='09:00' end='19:00'/>
            <work-day week-day='th' start='09:00' end='19:00'/>
            <work-day week-day='fr' start='09:00' end='19:00'/>
            <work-day week-day='sa' start='10:00' end='18:00'/>
            <work-day week-day='su' start='10:00' end='18:00'/>
        </work-week>
    </store>
</sklads>
</shop>
</yml_catalog>
```
Описание полей:  
`id` (String, Обязательное). Уникальный идентификатор склада. У двух складов не может быть одинакового `id`.  
`useForDelivery` (Boolean, Необязательное, По-умолчанию - true). Может ли склад использоваться для валидации остатков когда пользователь выбрал доставку при оформлении заказа.  
`phone` (String, Необязательное). Номер телефона в произвольном формате.  
`address` (String, Обязательное). Адрес магазина. Отображается на карте, а также как точка самовывоза/доставки при оформлении заказа.  
`lat`, `lon` (Float, Обязательные). Широта и долгота магазина. Для отделения дробной части можно использовать символы "." или ",". Указывают местоположение магазина на карте. Если не заданы, то магазин не будет отображаться на карте.  
`city`. (String, Необязательное, По-умолчанию `default`). Идентификатор города (см. [Город](#City)). Если город не указан ни в одном магазине, то пользователю не нужно будет выбирать его при оформлении заказа.  
`work-time`. ([WorkTime](#WorkTime), Необязательное). Время работы магазина.  

### WorkTime
Время работы магазина в течении недели.

Описание полей:  
`work-day` ([WorkDay](#WorkDay), Необязательное, Может быть указано несколько раз). Время работы магазина в конкретный день недели.  

### WorkDay
Время работы магазина в конкретный день недели.
Описание полей:  
`week-day` (String, Обязательное). Идентификатор дня недели. Может быть `mo` (Понедельник), `tu` (Вторник), `we` (Среда), `th` (Четверг), `fr` (Пятница), `sa` (Суббота), `su` (Воскресенье).  
`start` (String, Обязательное). Время начала работы магазина в формате `HH:mm`.  
`end` (String, Обязательное). Время конца работы магазина в формате `HH:mm`.  

### City
Город. Если не указано ни одного города, то пользователю не нужно будет выбирать его при оформлении заказа.
```xml
<yml_catalog>
<shop>
<city id='1'>Москва</city>
<city id='2'>Санкт-Петербург</city>
</shop>
</yml_catalog>
```
Описание полей:  
`id` (String, Обязательное). Уникальный идентификатор города. У двух городов не может быть одинакового `id`.  
`Содержание тэга` (String, Обязательное). Название города.  
