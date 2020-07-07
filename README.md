# HomeAssistant
My Home automations

Для считывания импульсов с прибора учета использую готовый фотомодуль с компаратором LM393:
https://arduinomaster.ru/datchiki-arduino/photorezistor-arduino-datchik-sveta/
https://robotchip.ru/obzor-modulya-osveshchennosti-lm393/


Однако, такой фотомодуль с LM393 имеет серьезный недостаток - дребезг. Посему требуется от него избавляться с использованием простейшего RC фильтра. Некоторые изобретатели утверждают, что в таких фотомодулях фильтр уже установленю. Его там нет. В этом легко убедиться, взяв в руки осциллограф.
http://forum.amperka.ru/threads/%D0%94%D1%80%D0%B5%D0%B1%D0%B5%D0%B7%D0%B3-%D0%BA%D0%BD%D0%BE%D0%BF%D0%BA%D0%B8.6607/
https://chipenable.ru/index.php/programming-avr/162-prostoy-cifrovoy-filtr.html
https://ru.howto-wp.com/55588-Arduino-RC-Circuit-PWM-to-analog-DC-42

Я использую RC 10мкФ + 1кОм. Использование RC фильтра в моем случае свело погрешность измерений к приемлемым значениям (инфикация прибора учета и HA совпадают в конце месяца).

Это нужно знать при использовании импульсного счетчика:
https://habr.com/ru/post/465445/