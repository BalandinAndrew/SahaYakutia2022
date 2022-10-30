# SahaYakutia2022
Russian Regional Championship (Saha-Jakutia Region) October 2022

В состав репозитория включены файлы кода и входные данные для решения в три этапа.

1-й этап - предобработка и основная модель. Файл "Якутия разбор и объединение данных.lgp" - модуль lgp аналитической платформы Логином в бесплатной общедоступной версии Community Edition v.6.5.1 (дистрибутив можно свободно получить по адресу https://loginom.ru/download). Входные данные - приложенные в условиях задачи, без каких-либо добавлений (в последней версии исходных наборов задачи). Выходные данные предобработки - трейн (файл "train_united.csv"), тест (файл "test_united.csv"), в готовом виде для обработки основной моделью. Основная модель - ноутбук Jupyter "Якутия_бинарные_разделения_540941.ipynb", целиком воспроизводимый код, дающий промежуточный результат по прогнозу (на публичном лидерборде 0.540941). Приложен итог работы модели - сабмиссия в файле "Якутия воспроизводимый результат_540941.csv".
Модель выполняет две бинарные классификации - по классу 2 и по классу 0 и, в заключении - одну мультиклассовую (все - с помощью библиотеки CatBoost). При выполнении первой классификации по классу 2 формируются два вспомогательных файла - прогноз на трейне по сидам 15,24,98 (файл "Якутия train_summ_p_3.csv") и на тесте с теми же сидами (файл "Якутия старт проверка_summ_p_15_3.csv") , которые подгружаются в основную модель перед второй бинарной классификацией для сокращения теста и трейна, с исключением точно предсказанных значений.

2-й этап - корректировка итога результата основной модели. Выполняется за счет повторения шагов бинарной классификации, что и на первом этапе, с использованием классификатора xgboost. Основной модуль выполнения 2-го этапа в ноутбуке "xgboost_два_бинарных_разделения.ipynb". В модель подгружаются трейн, тест и промежуточные файлы те же, что и к первой модели. По итогам выполнения ноутбука формируется файл "Якутия ансамбль.csv", где классом "4" обозначены значения класса "1", а также значения записей, чьи прогнозы расходятся на моделях xgboost и CatBoost. Эти значения (всего 114 записей) предназначены для прогноза в окончательной модели на этапе 3 настоящего решения.

3-й этап. Окончательный прогноз на 114 значений по итогу 2-го этапа выполняется на кластеризаванных трейне и тесте. Кластеризация выполнена в модуле "Логином" (файл "Якутия разбор кластеризация и объединение данных.lgp"). Выходные файлы кластеризации приложены в файлах "train_cluster.csv" и "test_cluster.csv", также используется промежуточный файл 2-го этапа "Якутия ансамбль.csv", в котором записи, предназначенные для прогноза обозначены классом "4". При выполнении классификации моделью CatBoost (ноутбук "Якутия_кластеризация.ipynb") получен результат "Якутия финал_603837.csv". Однако, в связи с тем, что в полученном результате наблюдается явный перекос к классу "1", результат дополнительно обработан с помощью лучшей промежуточной сабмиссии (которая также подгружается в модель 3-го этапа) для общей балансировки итогового результата . Лучший результат корректировки сохранен в файле "Якутия окончательный прогноз.csv".
Модель 3-го этапа с формированием лучшей сабмиссии целиком воспроизводима.

Итоговый результат - 0.660199 на публичном лидерборде.
