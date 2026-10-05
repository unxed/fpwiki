# IDE Window: Debug History

│ **[English (en)](<../en/IDE_Window__Debug_History.md>)** │  **русский (ru)** │

## Contents

  * 1 Важно
  * 2 История отладки
  * 3 Ограничения
  * 4 Отображаемые данные
  * 5 Автоматические записи в сравнении со снапшотами (сделанные пользователем)
  * 6 Интерфейс
  * 7 См.также



# Важно

Вы должны [ настроить отладчик](<Debugger_Setup.md> "Debugger Setup/ru") и запустить проект для его отладки. Только тогда это окно будет полезно. 

# История отладки

[![History.png](https://wiki.freepascal.org/images/b/bf/History.png)](</File:History.png>)

В окне «История» отображается список мест, где приложение было ранее остановлено или приостановлено (например, Hit Breakpoint, остановлено после выполнения пошаговой отладки). Записи также могут быть добавлены с помощью неразрывной [ Точки останова](<IDE_Window__Breakpoints.md> "IDE Window: Breakpoints/ru"). 

Каждый раз, когда добавляется запись, сохраняются локальные переменные, список наблюдения, стек и потоки, которые отображаются/оцениваются в данный момент. Окно истории позволяет выбрать каждую запись и просмотреть сохраненные значения. Сохраненные значения можно просмотреть в обычных окнах просмотра (например, в окне просмотра), которые будут следовать за выбором окна истории. 

  


# Ограничения

  * История влияет только на окна Список наблюдения, Локальные переменные, Потоки и Стек.



    Другие окна **не** затрагиваются. Не затронутые окна продолжают отображать текущие данные, пока выбрана запись в окне истории отладки

  * Автоматически оцениваются только окна Список наблюдения и Локальные переменные в верхнем стеке текущего потока (доступно, даже если соответствующие окна просмотра были закрыты). Остальные данные сохраняются только в том случае, если они отображались в это же время (например, если пользователь вручную выбрал другой фрейм и просмотрел окно Списка наблюдления в окне просмотра.
  * Оценка данных может быть прервана, если пользователь запускает или выполняет шаги приложения до того, как данные будут готовы. В этом случае они недоступны в окне истории отладки



# Отображаемые данные

  * Время, когда было приняты входные данные. Это не указывает, сколько времени работало приложение, так как включает время паузы.
  * Местоположение: если доступно, имя метода (формат зависит от типа отладочной информации) и исходная строка. Это относится к потоку, который был активен в то время.



# Автоматические записи в сравнении со снапшотами (сделанные пользователем)

Запись добавляется в основной список автоматически каждый раз, когда отладчик приостанавливает работу приложения. Этот список также удалит старые записи и сохранит только последние n записей. 

Второй список содержит только записи, выбранные пользователем. Эти записи хранятся до окончания сеанса отладки. Записи могут быть добавлены в этот список либо кнопкой снимка, либо свойством точки останова "take snapshot" (сделать снимок). 

# Интерфейс

Двойной щелчок
    выбор или отмена выбора записи. Если запись выбрана, то [Watch list](<IDE_Window__Watch_list.md> "IDE Window: Watch list/ru"), [Local variables](<IDE_Window__Local_Variables.md> "IDE Window: Local Variables/ru"), [Stack](<IDE_Window__Call_Stack.md> "IDE Window: Call Stack/ru") и [Thread](<IDE_Window__Threads.md> "IDE Window: Threads/ru") показывают содержание окна истории отладки.
[![debugger power.png](https://wiki.freepascal.org/images/b/ba/debugger_power.png)](</File:debugger_power.png>) Power
    если питание выключено, теперь будут создаваться записи истории.
[![debugger enable.png](https://wiki.freepascal.org/images/7/76/debugger_enable.png)](</File:debugger_enable.png>) Enable
    указывает/переключает, если история отображается в других окнах. Использует последнюю запись в истории, установленную двойным щелчком
[![clock.png](https://wiki.freepascal.org/images/8/8b/clock.png)](</File:clock.png>)/[![camera.png](https://wiki.freepascal.org/images/d/de/camera.png)](</File:camera.png>) Choose list
    выбор списка снимков. [![clock.png](https://wiki.freepascal.org/images/8/8b/clock.png)](</File:clock.png>) Автоматически создаваемые записи, создаваемые на каждом шаге/паузе (если питание включено). Содержит до 25 последних записей. [![camera.png](https://wiki.freepascal.org/images/d/de/camera.png)](</File:camera.png>) Пользователь выбирал снимки или снимки по Списку наблюдения с опцией «сделать снимок». Список неограничен.
[![camera add.png](https://wiki.freepascal.org/images/1/17/camera_add.png)](</File:camera_add.png>) Add to selected snapshot list
    Добавляет текущую запись в список, выбранный пользователем. Запись остается в автоматическом списке до тех пор, пока не будет заменена более новыми записями.
[![laz delete.png](https://wiki.freepascal.org/images/6/63/laz_delete.png)](</File:laz_delete.png>) Remove
    Удаляет одну запись из текущего списка.
[![menu clean.png](https://wiki.freepascal.org/images/7/74/menu_clean.png)](</File:menu_clean.png>) Delete all
    Удаляет все записи из текущего списка.
[![laz save.png](https://wiki.freepascal.org/images/c/c1/laz_save.png)](</File:laz_save.png>)/[![laz open.png](https://wiki.freepascal.org/images/5/5f/laz_open.png)](</File:laz_open.png>) Export/Import
    Экспорт / Импорт всех записей, включая значения для часов, локальных переменных, стека и потоков

# См.также

  * [Watch list](<IDE_Window__Watch_list.md> "IDE Window: Watch list/ru")
  * [Local variables](<IDE_Window__Local_Variables.md> "IDE Window: Local Variables/ru")
  * [Call Stack](<IDE_Window__Call_Stack.md> "IDE Window: Call Stack/ru")
  * [Thread](<IDE_Window__Threads.md> "IDE Window: Threads/ru")


  * [Объявление в блоге разработчиков](<http://lazarus-dev.blogspot.co.uk/2011/05/remember-remember-history-of-debugging.html>)(англ.)

---

_Source: [https://wiki.freepascal.org/IDE_Window%3A_Debug_History/ru](https://web.archive.org/web/20221205004459/https://wiki.freepascal.org/IDE_Window%3A_Debug_History/ru)_
