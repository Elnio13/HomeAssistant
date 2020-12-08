# HomeAssistant

## Прибор учета

![Такой прибор учета установлен у меня](https://github.com/Elnio13/HomeAssistant/blob/master/1.BMP)


![](https://github.com/Elnio13/HomeAssistant/blob/master/2.jpg)



![](https://github.com/Elnio13/HomeAssistant/blob/master/3.jpg)


![](https://github.com/Elnio13/HomeAssistant/blob/master/4.jpg)


![](https://github.com/Elnio13/HomeAssistant/blob/master/5.jpg)

____

~~Для считывания импульсов с прибора учета использую готовый фотомодуль с компаратором LM393:

https://arduinomaster.ru/datchiki-arduino/photorezistor-arduino-datchik-sveta/

https://robotchip.ru/obzor-modulya-osveshchennosti-lm393/

Однако, такой фотомодуль с LM393 имеет серъезный недостаток и не один. Это не исмерительный инструмент со всеми вытекающими. 

## RC фильтр можно применить на входе DIO. 

http://forum.amperka.ru/threads/%D0%94%D1%80%D0%B5%D0%B1%D0%B5%D0%B7%D0%B3-%D0%BA%D0%BD%D0%BE%D0%BF%D0%BA%D0%B8.6607/

https://chipenable.ru/index.php/programming-avr/162-prostoy-cifrovoy-filtr.html

https://ru.howto-wp.com/55588-Arduino-RC-Circuit-PWM-to-analog-DC-42

RC 10мкФ (керамика) + 1кОм. Фильтр ставим как можно ближе к ноге DIO.  Использование RC фильтра в моем случае свело погрешность измерений к приемлемым значениям.

Я потерял очень много времени, возлагая надежды на программный фильтр в ESP 8266/W32. Он практически не влияет на ситуацию с дребезгом. В моем случае он делал измерения совсем непригодными.

## Это нужно знать при использовании импульсного счетчика:

https://habr.com/ru/post/465445/

## Взаимодействие ESP и HA
Значения имульсного счетчика передаются в HA, после включения ESP значение сенсора снова передается на нее. Количество посчитанных импульсов, в случае недоступности HA, сохраняется на внутренней влешке ESP и складываются с полученными от HA значениями. Т.о. при перезагрузке HA или ESP значения не теряются.

python_scripts нужен для корректировки значений прибора учета, он напрямую изменяет значение переменной, кот хранит показания.
