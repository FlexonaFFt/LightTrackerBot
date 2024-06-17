Чат-бот " Бот для получения уведомлений по отключениям" 

Предусмотрена только роль пользователя, который будет взаимодействовать с ботом

**1.	Структура бота**

Чтобы использовать функционал бота при первом входе пользователь должен пройти регистрацию. Сценарий регистрации описан в таблице.


| Действие пользователя | Действие программы/условия |
| ------ | ------ |
| 1. Первое открытие бота |  Вывод сообщения: “Этот бот предназначен для подписки на уведомления по отключениям водоснабжения” |
| 2. Кнопка Начало работы (/start) |	Нажатие кнопки Начало работы (/start)	Вывод сообщения “Просим предоставить номер телефона” |
| 3. Кнопка Предоставить номер |	Нажатие кнопки Предоставить номер	Получение номера телефона |

После предоставления номера бот продолжает диалог с пользователем и выдает сообщение о том, что нужно зарегистрировать адрес, по которому будут приходить уведомления. Текст "Благодарим за предоставление номера телефона."

	
|Вопрос	|Формат ответа|
|------|-----------|
|4.Укажите адрес, по которому необходимо присылать уведомления по отключениям | Набор текст, обязательное |
| | Если пользователь вписывает текст, то необходимо проводить проверку по адресной базе, полученной из Датум АВР. Если адрес введен верно, то давать возможность подтверждение сохранения данных (пункт 7). Если адрес с ошибкой, то выводить пример правильного написания и давать предупреждение о неверном формате адреса*. *Адрес [адрес, который ввел пользователь] введен неверно. Пример, г. Ростов-на-Дону, ул. Садовая, д. 4. «Адрес [город Ростов-на-дону Чадовая 4] введен неверно. Пример, г. Ростов-на-Дону, ул. Садовая, д. 4» Тут должна быть валидация адресов (?).	
| 5.Адрес указан верно: [адрес]? _«Адрес указан верно: [г. Ростов-на-Дону, ул. Садовая, д. 4] ?_»	| две кнопки, да/нет.|
| |Если пользователь указывает, что ввел данные верно, то выдать текст. Благодарим за регистрацию в боте, теперь вы будете получать уведомления об отключениях по адресу [адрес]. «Благодарим за регистрацию в боте, теперь вы будете получать уведомления об отключениях по адресу г. Ростов-на-Дону, ул. Садовая, д. 4»Если пользователь указывает, что ввел данные неверно, то повторить пункт 6. |

Данные о пользователе (телефон) и его ответах (адрес, на который подписан), а также его активность (активный пользователь или архивный, по умолчанию все пользователи активные) вносятся в базу данных с присвоением уникального номера.


**2. Оповещение в боте**

После регистрации, Бот должен отсылать уведомления пользователям при создании события (аварийного отключения или планового) по указанному адресу.

Например, если по адресу, указанному пользователем, будет событие – плановое отключение или аварийная заявка, то Бот отсылает уведомление в чат.

Текст уведомления, настраиваемый один раз при разворачивании бота.


<details><summary>Например,Уважаемый абонент, по вашему адресу [адрес] будет произведено [событие] с [время фактического отключения дата/время] по [время планового включения дата/время].</summary>

Если аварийное отключение из заявки с указанием только фактического отключения–
Уважаемый абонент, по вашему адресу [г. Ростов-на-Дону, ул. Садовая, д. 4] будет произведено [аварийное отключение ХВС] с [14.05.2024 20:40].

Если аварийное отключение из заявки с указанием только фактическим отключением и плановым включением -
Уважаемый абонент, по вашему адресу [г. Ростов-на-Дону, ул. Садовая, д. 4] будет произведено [аварийное отключение ХВС] с [14.05.2024 20:40] по [14.05.2024 22:40].

Если плановое отключение с указанием только фактического отключения- Уважаемый абонент, по вашему адресу [г. Ростов-на-Дону, ул. Садовая, д. 4] будет произведено [плановое отключение ХВС] с [14.05.2024 20:40].

Если плановое отключение с указанием фактическим отключением и плановым включением- 
Уважаемый абонент, по вашему адресу [г. Ростов-на-Дону, ул. Садовая, д. 4] будет произведено [плановое отключение ХВС] с [14.05.2024 20:40] по [14.05.2024 22:40].
</details>


**3. Меню Бота**

Пользователь может взаимодействовать с ботом через меню 
| Действие пользователя |Действие бота |
| ------ | ------ |
| 6.	Начало работы (/start)| Сбрасывается процесс авторизации (указанных данных пользователем ранее) и переход к подглаве 2.1 |
| 7.	Отписаться (/unsubscribe) | Отписывается от адресов, но   авторизация по телефону остается. Вывод сообщения вы отписались от уведомлений по событиям отключений по адресу [адрес]. «Вывод сообщения вы отписались от уведомлений по событиям по адресу [г. Ростов-на-Дону, ул. Садовая, д. 4].» Если пользователь пишет любой текст, то переводить его к пункту 6,  и выводить текст «Укажите местоположение, по которому необходимо присылать события» |	

- Если пользователь использовал кнопку отписаться и не ввел адрес, то пользователь становится архивным и он не получает уведомления.
- Если пользователь использовал кнопку отписаться и ввел адрес, то он остается активным и меняется его адрес в БД на новый и он получает уведомления.
- Если пользователь использовал кнопку начало работы, то создается новый пользователь с статусом активный, старый пользователь помечается, как архивный. 


**Примечание**

Список адресов необходимо получить по средствам интеграции с «Датум АВР» 


Список событий отключений необходимо получить по средствам интеграции с «Датум АВР», классифицировать отключение по аварийности необходимо исходя из процесса создания отключения: плановая работа или заявка. 


Пользователи без заполненных полей [адрес] не сохраняются в БД. Предполагаю, чтобы не дублировались записи пользователей в бд, если будут нажимать постоянно /start.