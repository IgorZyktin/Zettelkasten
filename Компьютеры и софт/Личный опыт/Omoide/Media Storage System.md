# Media Storage System

Мой проект по хранению и навигации по архиву с картинками.



- [{{ компьютеры }}](../../../__tags/kompytery.md)
- [{{ программное обеспечение }}](../../../__tags/programmnoe_obespechenie.md)
- [{{ python }}](../../../__tags/python.md)
- [{{ Личный опыт }}](../../../__tags/lichnyy_opyt.md)
- [{{ Разработка }}](../../../__tags/razrabotka.md)
- [{{ Media Storage System }}](../../../__tags/media_storage_system.md)
- [{{ Omoide }}](../../../__tags/omoide.md)


## Как всё начиналось

Проект [Media Storage System](https://github.com/IgorZyktin/MediaStorageSystem)
задумывался как простой способ быстрой навигации по большому объёму медиа
контента. Он продумывался в связке с проектом
[Image Match](https://github.com/EdjoLabs/image-match). Я вытащил оттуда
большой кусок кода для построения сигнатур изображений. Предполагалось, что
система будет хранить сигнатуры всего внутри хранилища и не позволит дважды
загрузить одно и то же.

Я сформулировал цель как:

**Получить такое хранилище данных, в котором я смогу собрать все существующие
медиа материалы по Bubblegum Crisis и Blade Runner без повторений.**

На начальном этапе сигнатуры не были реализованы. Я запаковал приложение в
исполняемый файл и предлагал пользователям запускать его локально. Из-за такого
формата запуска не могло быть речи об использовании сторонних баз данных,
поэтому хранение метаинформации было организовано в JSON файлах. Одновременно с
этим предполагалось, что все данные можно будет скачать в отдельном zip архиве,
так что пользователь не был привязан к приложению для просмотра контента.

## Как продолжалось

![cover](Media%20Storage%20System%202.png)

Очень быстро пришло понимание, что пользователям не интересно что-то там
ставить и как то пытаться запускать. Поэтому я сделал упор на web версию.
Технически это было ровно то же самое приложение, запущенное на моей Raspberry
pi4 и подключенной через домашний роутер. Запросы шли на IP адрес роутера и
через NAT пробрасывались на малинку. На удивление, адрес роутера оставался
неизменным месяцами подряд, поэтому пользователи стабильно могли получать
доступ к странице.

Я не стал фокусироваться на сборе фанарта и вместо этого грузил артбуки и
комиксы. На 8 мая 2021 года в архиве лежит 4,8 ГиБ данных в виде 3871 картинок.
Ещё столько же картинок представляют собой уменьшенные версии для пред
просмотра и ещё столько же — совсем мелкие версии для группового отображения.
При этом метаинформация в JSON весит примерно 27 мегабайт. До видео, аудио и
анимированных картинок дело так и не дошло. Скорость доступа к произвольным
результатам поиска составляет порядка 0,01 секунды. Поиск осуществлялся просто
по словарю, висевшему в памяти. Для локализации нужных записей применялись
операции на множествах.

Если считать, что в среднем артбуке 150 страниц, то в хранилище сейчас ~25
артбуков.

## Как могло продолжиться

Я начал прикидывать схемы масштабирования архитектуры для увеличения объёма
хранения данных. Морально был готов потратить ~10к или около того рублей, чтобы
получить несколько raspberry pi4. Одна выступала бы в качестве головной машины,
там крутился бы входной NGINX, питоновское приложение и какое-нибудь хранилище
типа Redis для сбора статистики по просмотрам. На всех ведомых машинах
достаточно было бы установить NGINX и они могли бы отдавать картинки с флешек,
воткнутых в USB разъёмы. Путём проксирования этих запросов, пользователь
получал бы страницу с картинками на ней, не зная с каких машин они собраны.

Минимально возможное количество софта, почти бесконечный ресурс флешек в режиме
только чтения, возможность иметь библиотеку размером в сотни гигабайт
практически бесплатно. Не уверен, что бы я делал при росте нагрузки хотя бы до
десятков запросов в секунду. Плюс висел вопрос — как дать пользователю
возможность скачать в один заход все файлы в его поисковой выборке? С учётом
жёстких лимитов на все виды памяти, я не смог придумать простого способа это
реализовать.

Чтобы сильно не забивать себе голову проектом, я решил выпускать обновления
один раз в месяц.

## Как закончилось

![cover](Media%20Storage%20System%201.png)

Практика показала, что оно никому не нужно. Основной аудиторией, с которой я
взаимодействовал были фанаты Bubblegum crisis и Blade runner соответственно.
Оба фэндома очень старые, старше 30 лет, хотя я изначально не придавал этому
значения. До меня это начало лучше доходить, когда я начал пытаться
продвигаться и в других сообществах, в основном ориентированных на старое
аниме. Собственно, большинство артбуков было посвящено именно ему, поэтому мне
казалось, что такая аудитория мне и нужна. Очень быстро прояснилось, что
аудитория не особо то и заинтересовано в чём-то новом. Раньше я крутился в
более молодых сообществах и не видел особой проблемы, но тут быстро стало ясно,
что фанаты как таковые это довольно пассивные люди. Это не проявляется в случае
со свежим контентом, который находится на острие прогресса. Но потом тренды
меняются и самые активные пользователи убегают вслед за ними, остаются же люди,
которым просто нравится тема, но их вполне устраивает по кругу гонять один и
тот же контент. Через это я наконец понял смысл фразы о том, что все фанфики —
отстой. Они по умолчанию вторичны, а если там будет что-то новое, то они
оторвутся от оригинального произведения и фанфиками быть перестанут.

У меня было подозрение, что по сути нет никакой практики работы с артбуками.
Из-за неудобства ворочания сотнями и тысячами файлов, люди просто скачивают их,
бегло смотрят несколько картинок и закрывают, никогда больше не открывая. Я
рассчитывал дать инструмент, позволяющий быстро производить поиск по архиву и
выводить интересующие пользователя артбуки. По факту я получал меньше лайков,
чем любой пост с картинкой из оригинального произведения. На словах люди
говорили, что идея прикольная и искать удобно, но по факту, за всё время
существования проекта, я не получил ни единого отзыва. У меня например есть
подозрение, что поиск не очень интуитивен и требует времени, чтобы в нём
разобраться. Но мне никто ничего не сказал об этом.

На последнем этапе я зарегистрировался в пачке групп в ВК и FB, в которых
выкладывался контент по ретро аниме. Я добился создания нескольких постов в
сообществах, но отклик был фактически нулевым. Людям просто не нужна такая
система навигации по контенту. Во всяком случае фанатам различных
ретро-произведений. Да, ставят несколько лайков, но фактически не пользуются.

Так идея собрать самую полную в мире коллекцию материалов по BGC/BR с одной
стороны удалась, но с другой оказалась никому не нужна. В итоге я решил, что
затраченные усилия себя не окупают, продукт никому не нужен и вместо него лучше
сосредоточиться на способах зарабатывания денег. Не так романтично, зато более
практично. По факту систему скорее всего можно было бы раскрутить, потратив
больше сил на рекламу. Но продукт изначально задумывался как некоммерческий,
так что особого смысла в этом я не видел.

## Какие есть перспективы

Всё-ещё рассчитываю, что мог бы использовать систему для хранения личного
архива фотографий. Я не объявлял свои планы по поводу проекта, так что если
вдруг что-то изменится, его снова можно будет переключить в активную фазу.
