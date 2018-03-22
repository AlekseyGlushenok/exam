# JSON Storage

## About

Приложение представляет собой api для работы с хранилищем данных.

* api работает по протоколу http
* Авторизация для доступа к приватным данным по протоколу Basic Auth
* Формат ответа JSON

## Installation guide
* Скачать код из репозитория
* Выполнить команду 
```sudo chmod +x ./run.sh && ./run.sh```
* Следовать инструкциям при выполнении скрипта( Для подробной информации выполнить команду `./run.sh -h`)

## Documentation

### Требования к запросам

Запрос имеет формат
http://server_name/{accessMod}/{operation}/{fileUrl}/{format}

#### AccessMod
* public для доступа к публичным данным
* private для доступа к приватным данным

#### Operation
* files  -получения списка всех файлов
* file   - получение содержимого одного файла, необходимо указать адрес файла
* upload - загрузить файл на сервер
* delete - удалить файл, необходимо указать адрес файла
* update - обновить файл, необходимо указать адрес файла
* download - скачать файл, необходимо указать адрес файла и формат(по умолчанию json)
#### fileUrl
* Адрес файла. По заданию устойчив к перебору(но это не точно).
#### format 
* Доступен только для операции скачивания файла. Можно указать xml, по умолчанию json

### Ответ сервера

Ответ сервера имеет формат JSON
```
{
	'status': int,
	'data':[]
}
```

При выполнении операции upload можно отправить несколько файлов тогда ответ сервера будет содержать статус для каждого файла.
```
{
	'status': 0,
	'data': [
		{   
			'status': int',
			'data': [],
			'name': 'uploadFileName'
		}, ...
	]
}
```

### Статусы
На каждый запрос, сервер высылает статус выполнения операции. (Исключение: при успешном выполнении операции download, статуса нет)

* 0 - Операция выполнена успешно
* 1 - Неверный запрос
 * 11 - Тело запроса пустое
* 2 - Ошибка обработки данных
 * 21 - Ошибка при конвертации файла
 * 22 - Данные не прошли валидацию
* 3 - Ошибка в работе с файлами
 * 31 - Запись в базе не найдена
 * 32 - Файл не существует
 * 33 - Ошибка при записи файла
* 4 - Нет прав на выполнение операции
