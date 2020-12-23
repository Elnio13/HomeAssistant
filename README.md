# HomeAssistant

## Прибор учета

![ ](https://github.com/Elnio13/HomeAssistant/blob/master/1.jpg)

Такой прибор учета у меня установлен. Меркурий 201.5

![ ](https://github.com/Elnio13/HomeAssistant/blob/master/2.jpg)

Для подсчета количесва импульсов использовал схему c фотодиодом U2 BPW34 с компаратором LM393. Выход компаратора обязательно подтягиваем вверх R2 3-10кОм. Обеспечиваем гистерезис с помощью R1 1МОм, иначе возможен дребезг. С1 подпирает питание, лишним не будет. С2 0,01мкФ сглаживает выходной сигнал. Больше лучше не ставить, будет завал переднего фронта.

![ ](https://github.com/Elnio13/HomeAssistant/blob/master/3.jpg)

Первый полностью рабочий вариант поделки.

![ ](https://github.com/Elnio13/HomeAssistant/blob/master/4.BMP )

Вот такую лажу наблюдаем когда светодиод просто горит! Это БЕСПОЛЗНЫЙ для нас сигнал - на светодиоде 50Гц! Нам нужны паузы среди этого сигнала длиной 170-180мс. Длина паузы не фиксирована, нужно иметь это в виду.

![ ](https://github.com/Elnio13/HomeAssistant/blob/master/6.BMP)

Вот полезный сигнал

![ ](https://github.com/Elnio13/HomeAssistant/blob/master/5.jpg)

Так выглядит домашняя АИИСКУЭ
____

~~Для считывания импульсов с прибора учета использую готовый фотомодуль с компаратором LM393:~~

https://arduinomaster.ru/datchiki-arduino/photorezistor-arduino-datchik-sveta/

https://robotchip.ru/obzor-modulya-osveshchennosti-lm393/

Однако, такой фотомодуль с LM393 имеет серъезный недостаток и не один. Это не измерительный инструмент со всеми вытекающими. 

## RC фильтр можно применить на входе DIO. 

http://forum.amperka.ru/threads/%D0%94%D1%80%D0%B5%D0%B1%D0%B5%D0%B7%D0%B3-%D0%BA%D0%BD%D0%BE%D0%BF%D0%BA%D0%B8.6607/

https://chipenable.ru/index.php/programming-avr/162-prostoy-cifrovoy-filtr.html

https://ru.howto-wp.com/55588-Arduino-RC-Circuit-PWM-to-analog-DC-42

RC 10мкФ (керамика) + 1кОм. Фильтр ставим как можно ближе к ноге DIO.  Использование RC фильтра в моем случае свело погрешность измерений к приемлемым значениям.

Я потерял очень много времени, возлагая надежды на аппаратный фильтр в ESP 8266/W32. Он практически не влияет на ситуацию с дребезгом. В моем случае он делал измерения совсем непригодными.

## Это нужно знать при использовании импульсного счетчика:

https://habr.com/ru/post/465445/

## Взаимодействие ESP и HA
Значения имульсного счетчика передаются в HA, после включения ESP значение сенсора снова передается на нее. Количество посчитанных импульсов, в случае недоступности HA, сохраняется на внутренней влешке ESP и складываются с полученными от HA значениями. Т.о. при перезагрузке HA или ESP значения не теряются (точнее не все теряются).

~~python_scripts нужен для корректировки значений прибора учета, он напрямую изменяет значение переменной, кот хранит показания.~~
