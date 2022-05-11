# Интеграция с Netsparker

Stingray обеспечивает возможность передачи данных в различные инструменты анализа защищенности приложений, включая популярный инструмент динамического и интерактивного анализа — [Netsparker](https://www.netsparker.com/). Такая интеграция позволяет расширить список ссылок, анализируемых Netsparker, и существенно повысить качество результатов последующего сканирования. 

Передача данных осуществляется в формате har. Чтобы передать данные в Netsparker, необходимо предварительно в Stingray получить результаты работы модуля Сетевая активность. Данный процесс подробно описан в разделе «[Интеграция c Burp Suite](https://help.stingray-mobile.ru/mergedProjects/aag/integraciya_c_burp_suite.htm) / [Получение результатов работы модуля Сетевая активность](https://help.stingray-mobile.ru/mergedProjects/aag/integraciya_c_burp_suite.htm#%D0%9F%D0%BE%D0%BB%D1%83%D1%87%D0%B5%D0%BD%D0%B8%D0%B5_%D1%80%D0%B5%D0%B7%D1%83%D0%BB%D1%8C%D1%82%D0%B0%D1%82%D0%BE%D0%B2_%D1%80%D0%B0%D0%B1%D0%BE%D1%82%D1%8B_%D0%BC%D0%BE%D0%B4%D1%83%D0%BB%D1%8F_%D0%A1%D0%B5%D1%82%D0%B5%D0%B2%D0%B0%D1%8F_%D0%B0%D0%BA%D1%82%D0%B8%D0%B2%D0%BD%D0%BE%D1%81%D1%82%D1%8C)».

Скачанный архив содержит три файла в различных форматах, включая har — его мы и будем импортировать в Netsparker.

## Импорт данных в Netsparker

В главном меню Netsparker выберите **Scans** > **New Scan**.

На странице **New Scan** выберите **Imported Links**.

<figure markdown>
![](../ag/img/22.png)
</figure>

Укажите импортируемый файл в поле **Import Links**.