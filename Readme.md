Техническое задание
1. Общая концепция
   Интернет-аукцион «YetiCave» — это простой сервис, позволяющий пользователям продать свои личные вещи по максимально выгодной для них цене — на основе аукциона.

Пользователь, предложивший максимальную цену получает вещь по этой стоимости.

Основные сценарии использования сайта:

Просмотр списка активных лотов;
Публикация лота;
Добавление ставок;
Поиск лотов по категориям и названиям;
Определение победителя на основе максимальной ставки.
2. Техническое описание
   Для разработки сайта предлагается уже готовая вёрстка, от программиста требуется лишь написать бэкенд сайта, т. е. сделать сайт динамическим, реализовать возможности по добавлению, просмотру лотов и приему ставок.

Разработка бэкенда должна вестись на языке программирования PHP 7 и выше, база данных — MySQL 5.7 и выше. Не предполагается использование языка JavaScript для клиентского программирования.

При разработке схемы базы данных необходимо принимать во внимание какие сущности необходимо хранить, их поля и возможные связи друг с другом.

3. Описание процессов
   Процесс — это автономная операция, выполняющая полезное действие. К примеру: добавление нового комментария или регистрация аккаунта.

3.1. Регистрация нового аккаунта
Выполняется после заполнения формы на странице «Регистрация аккаунта».

Последовательность действий:

Проверить, что отправлена форма.
Убедиться, что заполнены все обязательные поля.
Проверить, что указанный email уже не используется другим пользователем.
Если есть ошибки заполнения формы, то сохранить их в отдельном массиве.
Если ошибок нет, то сохранить данные формы в таблице пользователей.
Если были допущены ошибки (не заполнены все поля, email занят и т. д.), то не добавлять данные в БД, а показать ошибки в форме под соответствующими полями.

Если данные были сохранены успешно, то переадресовать пользователя на страницу входа.

Примечания
Необходимо проверять, что значение из поля «email» действительно является валидным E-mail адресом. Для этих целей использовать встроенную функцию filter_var и фильтр FILTER_VALIDATE_EMAIL.

Для хранения пароля в БД, его предварительно нужно обработать встроенной функцией password_hash.

3.2. Публикация нового лота
Страница доступна только аутентифицированным пользователям.

Выполняется после заполнения формы на странице «Добавление лота». Все поля формы обязательны к заполнению.

Последовательность действий:

Проверить, что отправлена форма.
Убедиться, что заполнены все поля.
Выполнить все проверки.
Если есть ошибки заполнения формы, то сохранить их в отдельном массиве.
Если ошибок нет, то сохранить новый лот в таблице лотов, а файл изображения перенести в публичную директорию и сохранить ссылку.
При успешном сохранении формы, переадресовывать пользователя на страницу просмотра лота.

Список проверок

Проверка изображения

Обязательно проверять MIME-тип загруженного файла;
Допустимые форматы файлов: jpg, jpeg, png;
Для проверки сравнивать MIME-тип файла со значением «image/png», «image/jpeg»;
Чтобы определить MIME-тип файла, использовать функцию mime_content_type.
Проверка начальной цены

Содержимое поля «начальная цена» должно быть числом больше нуля.
Проверка даты завершения

Содержимое поля «дата завершения» должно быть датой в формате «ГГГГ-ММ-ДД»;
Проверять, что указанная дата больше текущей даты, хотя бы на один день.
Проверка шага ставки

Содержимое поля «шаг ставки» должно быть целым числом больше ноля.
3.3. Вход на сайт
Выполняется после заполнения формы на странице «Вход на сайт».

Последовательность действий:

Проверить, что отправлена форма.
Убедиться, что заполнены все поля.
Если есть ошибки заполнения формы, то сохранить их в отдельном массиве.
Найти в БД пользователя с переданным email.
Проверить, что переданный в форме пароль совпадает с сохраненным.
Если пользователь найден и пароли совпадают, то открыть новую сессию, в которую записать идентификатор пользователя.
После успешного входа пользователь попадает на главную страницу.

Примечания
Для проверки пароля использовать встроенную функцию password_verify.

3.4. Добавление ставки
Выполняется после заполнения формы на странице просмотра лота.

Последовательность действий:

Проверить, что пользователь залогинен.
Убедиться, что заполнено поле «ваша сумма».
Выполнить проверку значения поля «ваша сумма».
Добавить ставку в таблицу ставок с привязкой к лоту.
Проверка ставки

Значение поля «ваша сумма» должно отвечать следующим требованиям:

целое положительное число;
значение должно быть больше или равно, чем текущая цена лота + шаг ставки.
Текущая цена лота — это значение последней сделанной ставки, либо, если ставок нет, начальная цена лота

3.5. Определение победителя
Главная задача сервиса — это определять победившую ставку по истечению времени размещения лота.

Код для этого процесса можно разместить в отдельном сценарии и подключить к показу главной страницы, таким образом он будет выполняться при каждом посещении пользователем главной страницы.

Алгоритм работы:

Найти все лоты без победителей, дата истечения которых меньше или равна текущей дате.
Для каждого такого лота найти последнюю ставку.
Записать в лот победителем автора последней ставки.
Отправить победителю на email письмо — поздравление с победой.
3.6. Поиск
Выполняется после отправки формы поиска с любой страницы.

Последовательность действий:

Проверить, что была отправлена форма.
Получить текст из поля поиска и убрать пробелы функцией trim.
Проверить, что получившаяся строка не пустая.
Выполнить поиск в таблице лотов по полям «имя» и «описание», используя полнотекстовый поиск (операторы «MATCH... AGAINST»).
Показать результаты поиска на соответствующей странице.
Примечания
Искать следует как по описанию, так и по и названию.

4. Список всех страниц
   Сайт состоит из следующих страниц:

Главная страница
Регистрация аккаунта;
Вход на сайт;
Лоты по категории;
Результаты поиска;
Добавление нового лота;
Просмотр лота;
Список моих ставок.
4.0. Главная страница
Главная страница показывает последние открытые лоты.

Список лотов состоит только из обьявлений, которые еще не истекли. Сами обьявления отсортированы по дате публикации, от новых к старым.

Клик по названию лота вызывает переход на страницу просмотра лота.

Над списком лотов располагаеется список категорий. Клик по элементу списка вызывает переход на страницу «Лоты по категории»

4.1. Регистрация аккаунта
Чтобы пользователь имел возможность добавить на сайт свой лот, ему необходимо пройти процедуру регистрации на этой странице.

Форма состоит из четырёх обязательных полей и кнопки «регистрация». Форма отправляется методом «POST».

Описание полей:

Лейбл	Тип поля	Обязательность
Email	Простое текстовое поле	Да
Пароль	Поле типа «password»	Да
Имя	Простое текстовое поле	Да
Контактные данные	Поле типа «textarea»	Да
Пользователю надлежит заполнить все обязательные поля, указав свой действительный email, пароль для входа на сайт, имя, под которым будут публиковаться его лоты, контактные данные.

После заполнения формы, пользователь нажимает кнопку «Регистрация» для отправки данных формы на сервер.

Сама процедура регистрации описана в отдельном разделе — «Регистрация нового аккаунта».

Если по итогам выполнения процесса регистрации возникли ошибки заполнения формы, то эти ошибки должны быть показаны красным текстом под необходимыми полями.

4.2. Вход на сайт
Форма состоит из двух полей:

Email. Тип: текстовое поле;
Пароль. Тип: поле типа password.
Процедура входа описана в отдельном разделе — «Вход на сайт».

Если по итогам выполнения процесса входа на сайт возникли ошибки заполнения формы, то эти ошибки должны быть показаны красным текстом под необходимыми полями.

Если пользователь ввёл неверные данные аккаунта (несуществующий пользователь или неверный пароль), то в сообщении над формой должен быть текст «Вы ввели неверный email/пароль».

4.3. Лоты по категории
Страница состоит из заголовка с текстом: «Все лоты в категории %имя_категории%», где %имя_категории% — это название выбранной категории.

Под заголовком находится список из девяти активных лотов, принадлежащих к выбранной категории. Лоты отсортированы по дате публикации, от новых к старым. Если в категории больше лотов, то внизу страницы появляется блок постраничной навигации.

4.4. Результаты поиска
Страница состоит из заголовка с текстом: «Результаты поиска по запросу %поисковый_запрос%», где %поисковый_запрос% — это текст, который ввёл пользователь в строку поиска.

Под заголовком находится список из девяти активных лотов, найденных по поисковому запросу. Лоты отсортированы по дате публикации, от новых к старым. Если в категории больше девяти лотов, то внизу страницы появляется блок постраничной навигации.

4.5. Добавление нового лота
Авторизованный пользователь может опубликовать на сайте свой лот.

Под заголовком располагается форма из семи обязательных полей и кнопки «Опубликовать».

Лейбл	Тип поля	Обязательность
Наименование	Простое текстовое поле	Да
Описание	Поле типа «textarea»	Да
Изображение	Поле типа «file»	Да
Категория	Выпадающий список	Да
Начальная цена	Простое текстовое поле	Да
Дата окончания торгов	Простое текстовое поле	Да
Шаг ставки	Простое текстовое поле	Да
Выпадающий список поля «Категория» содержит все доступные на сайте категории. Пользователь обязательно должен выбрать одну из них.

После заполнения формы, пользователь нажимает кнопку «Добавить лот» для отправки данных формы на сервер.

Сама процедура добавления описана в отдельном разделе — «Публикация нового лота».

Если по итогам выполнения процесса отправки возникли ошибки заполнения формы, то эти ошибки должны быть показаны красным текстом под необходимыми полями, а под самой формой появится сообщение «Пожалуйста, исправьте ошибки в форме».

4.6. Просмотр лота
Основные функции этой страницы:

Просмотр информации лота;
Добавление своей ставки.
Страница делится вертикально на две части в пропорции 2/1: основной контент и блок для показа информации торгов.

Основной контент

Начинается с заголовка из названия просматриваемого лота. Под заголовком, на всю ширину блока располагается изображение — фото товара.

Под фотографией блок с информацией:

название категории;
описание.
Информация торгов

Блок с информацией о текущем состоянии торгов по этому лоту:

сколько осталось времени до окончания;
текущая цена лота;
минимальная ставка.
Время, оставшееся до окончания торгов, отображается в формате ЧЧ: ММ (часы: минуты).

Текущая цена рассчитывается как максимальная ставка по этому лоту, либо, если ставок нет, начальная цена лота.

Блок добавления ставки

Минимальная ставка должна быть равна текущей цене плюс шаг торгов. Далее следует текстовое поле с лейблом «Ваша ставка» и кнопкой «Сделать ставку». Рядом с полем подпись с указанием минимально возможной ставки.

Если торги по данному лоту закончились, то данный блок не показывается.

Ограничения

Блок добавления ставки не показывается если:

пользователь не авторизован;
срок размещения лота истёк;
лот создан текущим пользователем;
последняя ставка сделана текущим пользователем.
Блок «История ставок»

Показывает историю размещения ставок: от самых новых к самым старым. Информация о каждой ставке в этом блоке состоит из имени пользователя, суммы и времени размещения ставки в человеческом формате (5 минут назад, час назад и т. д.).

4.7. Мои ставки
Страница, где пользователь видит все свои ставки на различные лоты.

Каждая ставка имеет следующую информацию:

название лота (ссылка на страницу);
сумма ставки;
дата/время.
Выигравшие ставки должны выделяться отдельным цветом, а также там должны быть размещены контакты владельца лота.

5. Основные сущности
   Список всех сущностей:

Категория;
Лот;
Ставка;
Пользователь.
5.1. Категория
Поля:

название;
символьный код.
Каждый лот должен быть привязан к одной категории. Символьный код нужен, чтобы назначить правильный класс в меню категорий.

Список категорий с их символьными кодами: Доски и лыжи (boards), Крепления (attachment), Ботинки (boots), Одежда (clothing), Инструменты (tools), Разное (other).

5.2. Лот
Центральная сущность всего сайта.

Поля:

дата создания: дата и время, когда этот лот был создан пользователем;
название: задается пользователем;
описание: задается пользователем;
изображение: ссылка на файл изображения;
начальная цена;
дата завершения;
шаг ставки.
Связи:

автор: пользователь, создавший лот;
победитель: пользователь, выигравший лот;
категория: категория объявления.
5.3. Ставка
Ставка — это зафиксированное намерение пользователя приобрести товар, указанный в лоте по фиксированной стоимости.

Поля:

дата: дата и время размещения ставки;
сумма: цена, по которой пользователь готов приобрести лот.
Связи:

пользователь;
лот.
5.4. Пользователь
Представляет зарегистрированного пользователя.

Поля:

дата регистрации: дата и время, когда этот пользователь завёл аккаунт;
email;
имя;
пароль: хэшированный пароль пользователя;
контакты: контактная информация для связи с пользователем.
Связи:

созданные лоты;
ставки.

6. Роли пользователей
   Посетители сайта поделены на две группы: анонимные пользователи и зарегистрированные.

Анонимные пользователи имеют доступ только к просмотру лотов, поиску, навигации по категориям.

Пользователи, прошедшие регистрацию, дополнительно получают возможность публиковать свои лоты и делать ставки.
