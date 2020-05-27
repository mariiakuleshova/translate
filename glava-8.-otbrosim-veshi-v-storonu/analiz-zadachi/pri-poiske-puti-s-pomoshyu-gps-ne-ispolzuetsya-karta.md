# При поиске пути с помощью GPS не используется карта!

Я бы хотел иметь возможность, поговорить немного о планировщиках топологических путей, раз уж мы зашли так далеко. Это метод, альтернативный методам на основе сетки, которые мы использовали в предыдущих разделах. Есть типы проблем и типы навигации, где основанный на сетке подход не подходит или потребует астрономического количества подробных данных, которые могут быть недоступны для маленького робота.

В качестве примера я хотел рассказать о том, как ваш GPS в вашем автомобиле находит маршрут по улицам до пункта назначения. Вы, должно быть, задавались вопросом о том, как эта коробка имеет достаточно информация в ее крошечном мозге, чтобы обеспечить пошаговые инструкции передвижения от одного места к другому. Вы могли вообразить, что GPS использует ту же карту, которую вы просматривали на LCD-экране, чтобы определить, куда вам нужно идти. А также вы могли подумать, что происходит какой-то поиск по сетке , как в алгоритме A \*, который мы обсуждали в деталях. И вы были неправы.

Данные, которые использует GPS для планирования маршрута, совсем не похожи на карту.

Вместо карты используется топологическая сеть, которая показывает, как улицы взаимосвязаны. По формату это больше похоже на базу данных векторов, нежели на растровую карту с пикселями и цветами. Формат базы данных также занимает гораздо меньше места во внутренней памяти GPS. Улицы делятся на узлы или точки, где они пересекаются или изменяются. Каждый узел показывает, какие улицы связаны друг с другом. Узлы связаны ссылками, которые позволяют связать данные от узла к узлу. Ссылки представляют дороги, имеют длину и данные о затратах на качество дороги. Данные о затратах используются для расчета желательности маршрута. Шоссе с ограниченным доступом или с ограничением скорости будет иметь низкую стоимость, а небольшой переулок или грунтовая дорога с большим количеством знаков остановки будут иметь высокую стоимость, поскольку эта ссылка и менее желательна, и медленнее.

Мы используем точно такие же процедуры с базой данных дорожной сети GPS, как и работа процесса А \* на карте сетки. Мы оцениваем на каждом узле прогресс насколько мы продвинулись от нашего начального узла, выбирая путь, который ведет нас ближе всего к нашему пункту назначения. Многие системы GPS также одновременно пытаются оттолкнуться от конечной точки - цели или пункта назначения - и попытаться встретиться где-то посередине. Невероятно много работы ушло на то, чтобы сделать наш текущий вид систем GPS маленьким, легким, и надежным. Конечно, они зависимы от актуальности информации в базе данных:

![](../../.gitbook/assets/image%20%2810%29.png)


