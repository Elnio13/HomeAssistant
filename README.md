# HomeAssistant

## Прибор учета

![ ](https://github.com/Elnio13/HomeAssistant/blob/master/1.jpg)

Такой прибор учета у меня установлен. Меркурий 201.5

![ ](https://github.com/Elnio13/HomeAssistant/blob/master/2.jpg)

Для подсчета количесва импульсов использовал схему c фотодиодом U2 BPW34 с компаратором LM393. Выход компаратора обязательно подтягиваем вверх R2 3-10кОм. Обеспечиваем гистерезис с помощью R1 1МОм, иначе возможен дребезг. С1 подпирает питание, лишним не будет. С2 0,01мкФ сглаживает выходной сигнал. Больше лучше не ставить, будет завал переднего фронта.

Можно купить готовый датчик: OPT101, TSL12S (13,14), TSL257(*), MLX75305. Думаю это хороший вариант, я не смог его найти в свободной продаже.

![ ](https://github.com/Elnio13/HomeAssistant/blob/master/3.jpg)

Первый полностью рабочий вариант поделки.

![ ](https://github.com/Elnio13/HomeAssistant/blob/master/4.BMP )

Вот такую лажу наблюдаем когда светодиод просто горит! Это БЕСПОЛЗНЫЙ для нас сигнал - на светодиоде 50Гц! Причем 50Гц на светодиоде может присутствовать, может нет (зависимость я не определил, но очевидно, счетчик собран из веток и палок!). Нам нужны паузы среди этого сигнала длиной 170-180мс. Длина паузы не фиксирована, нужно иметь это в виду.

![ ](https://github.com/Elnio13/HomeAssistant/blob/master/6.BMP)

Вот полезный сигнал

![ ](https://github.com/Elnio13/HomeAssistant/blob/master/5.jpg)

Так выглядит домашняя АИИСКУЭ
____

~~Для считывания импульсов с прибора учета использую готовый фотомодуль с компаратором LM393:~~
Все советуют использовать это дешевое говно. В нем вообще не предусмотрен гистерезис, а значит будет наблюдаться дикий дребезг!

https://arduinomaster.ru/datchiki-arduino/photorezistor-arduino-datchik-sveta/

https://robotchip.ru/obzor-modulya-osveshchennosti-lm393/

Такой фотомодуль - не измерительный инструмент со всеми вытекающими. Это простой датчик, не счетчик импульсов!

## RC фильтр можно применить на входе DIO. 

http://forum.amperka.ru/threads/%D0%94%D1%80%D0%B5%D0%B1%D0%B5%D0%B7%D0%B3-%D0%BA%D0%BD%D0%BE%D0%BF%D0%BA%D0%B8.6607/

https://chipenable.ru/index.php/programming-avr/162-prostoy-cifrovoy-filtr.html

https://ru.howto-wp.com/55588-Arduino-RC-Circuit-PWM-to-analog-DC-42

RC 10мкФ (керамика) + 1кОм. Фильтр ставим как можно ближе к ноге DIO.  

Я потерял очень много времени, возлагая надежды на аппаратный фильтр в ESP 8266/W32. Он практически не влияет на ситуацию с дребезгом. В моем случае он делал измерения совсем непригодными.

## Это нужно знать при использовании импульсного счетчика:

https://habr.com/ru/post/465445/

## Взаимодействие ESP и HA
Количество импульсов заданной длины суммируется в течении минуты и  передаются в HA. Количество посчитанных импульсов, в случае недоступности HA, сохраняется на внутренней влешке ESP и передаются при появлении связи. Т.о. при перезагрузке HA или ESP значения не теряются (точнее не все теряются). Сенсор ESPhome будет тротлить значения, если они будут повторяться подряд. Поэтому **force_update: true** обязательно для передающего сенсора.

~~python_scripts нужен для корректировки значений прибора учета, он напрямую изменяет значение переменной, кот хранит показания.~~
