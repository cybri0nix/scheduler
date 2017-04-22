# Scheduler

[Scheduler.js](https://github.com/cybri0nix/scheduler/blob/master/libs/scheduler.js) - Библиотека для работы с расписанием лекций. Рядом с ней вы найдете реализацию библиотеки работы со справочниками лекций, лекторов, школ и аудиторий. Реализация может быть любой, главное чтобы структура данных была "понятной" для Scheduler'а ([YaSchool](https://github.com/cybri0nix/scheduler/blob/master/libs/yaschool.js)).

## Структура данных:
![Смотреть](https://github.com/cybri0nix/scheduler/blob/master/data-tructure.png)



## Быстрый старт
### Иннициализация библиотеки
```javascript
Scheduler.init({
	data: scheduleData,
	directories: directoriesData
})
.on("afterScheduleAdded", function(data) {
	// Будет выполнено после добавления пункта в расписании
	// Структура объекта data:
	// {
	// 	success: true,
	//	data: newSchedule (см. _models.schedule_)
	// }
})
.on("afterScheduleDeleted", function() {
	// ПОсле удаления пункта в расписании
})
.on("afterScheduleUpdated", function() {
	// После обновления данных
});
```

**Сруктура объекта scheduleData** (одномерный массив объектов):
```javascript
// Это массив объектов
var scheduleData = [  
  { _models.schedule_ },
  { _models.schedule_ }
  ...
  { _models.schedule_ }
]
```

**Структура объекта _models.schedule_**. Каждый такой обект содержит информацию о запланированной лекции:

```javascript
{
	"_id": null, // string unique hash
	"plannedDateTime": null, // int дата и время начала лекции в секундах
	"plannedDateTimeEnd": null, // int дата и время окончания лекции в секундах (TODO: переделать на duration)
	"lessonId": null, // string ключ в справочнике лекций
	"lecturerId": null, // string ключ в справочнике лекторов
	"roomId": null, // string ключ в справочнике аудиторий
	"schools": [], // array ["schoolId", "schoolId", ...] ключи в справочнике аудиторий
	"isDraft": false, // boolean true - не отображать запланированную лукцию в расписании
	"hasCookies": false // boolean false - нет печенек, true - есть печеньки
}
```
Структуру можно дополнять своими мета-данными, при добавлении расписания, новые поля будут присущи только добавляемому объекту.




**Структура объекта directoriesData**. Объект содержит:
```javascript
{
  "schools"	: { _YaSchool.models.school_ }, // Справочник школ
  "rooms"	: { _YaSchool.models.room_ }, // Справочник аудиторий
  "lessons"	: { _YaSchool.models.lesson_ }, // Справочник лекций
  "lecturers"	: { _YaSchool.models.lecturer_ }, // Справочник лекторов
}
```

В исходном коде [YaSchool](https://github.com/cybri0nix/scheduler/blob/master/libs/yaschool.js) вы найдете описание моделей 

**_YaSchool.models.school_** . Это объект, который описывает школу:
```javascript
{
	"_id"		: null, // string unique hash
	"title"		: "", // string - Полное название школы
	"studentsCount"	: 0, // int - количество студентов
	"shortTitle" 	: "" // string - сокращенное название школы
}
```

**_YaSchool.models.room_** . Это объект, который описывает аудиторию:
```javascript
{
	"_id"		: null, // string unique hash
	"title"		: "", // string - название аудитории
	"capacity"	: 0,  // int - количество мест в аудитории
	"location"	: "" // string - как пройти в аудиторию
}
```

**_YaSchool.models.lesson_** . Это объект, который описывает лекцию:
```javascript
{
	"_id"	: null, // string unique hash
	"title"	: "" // Название лекции
}
```

**_YaSchool.models.lecturer_** . Это объект, который описывает лектора:
```javascript
{
	"_id"	: null, // string unique hash
	"name"	: "", // 
	"ava"	: "",
	"bio"	: "",

}
```



## Возможности
#### Библиотека оперирует секундами (например в аргументах, при планировании лекуий), поэтому удобно использовать метод перевода человекопонятной даты в секунды:
```javascript
Scheduler.toSeconds('22-04-2017-19:00'); // ДД-ММ-ГГГГ-ЧЧ:ММ - вернут секунды (unixtime)
```
### Узнать название ошибки по коду:
```javascript
Scheduler.ERRORS.getErrorName(result.errorCode); 
```
**Названия ошибок:**
_Служебные ошибки_
* DATA_REQUIRED - 900 - объект data не подключен, см. Scheduler.init({ ... })
* YASCHOOL_DIRECTORIES_NOT_BINDED - 901 - объект со справочниками (directories) не подключен см. Scheduler.init({ ... })

_Ошибки целостности данных, при доабвлении или изменении пункта в расписании_
* LESSON_ALREADY_PLANNED_IN_THIS_ROOM_AT_THIS_TIME - 1000 - Лекция уже запланирована в этой Аудитории на эту ДатуВремя
* LESSON_ALREADY_PLANNED_FOR_THIS_SCHOOL_AT_THIS_TIME - 1001 - Лекция уже запланирована для этой школы в эту ДатуВремя
* TOO_MANY_STUDENTS_FOR_ROOM - 1002 - Недостаточно мест в аудитории для проведения лекции
* LECTURER_CANNOT_BE_IN_SEVERAL_ROOMS_AT_THE_SAME_TIME - 1003 - Лектор не может быть одновременно в нескольких аудиториях одновременно (в ук. ДатаВремя)
* UNKNOWN_SCHOOL - 1004 - школа не найдена (должна быть в справочнике)
* UNKNOWN_LESSON - 1005 - лекция не найдена
* UNKNOWN_ROOM - 1006 - аудитория не найдена
* UNKNOWN_LECTURER - 1007 - лектор не найден
* ITEM_NOT_FOUND - 1008 - элемент не найден (например, при обновлении)

Коды/Названия ошибок можно получить отсюда: Scheduler.ERRORS ({errname:code}


### Запланировать лекцию
```javascript
addSchedule(secondsLessonBegin, secondsLessonDuration, scheduleData[, callback])
```
* int **secondsLessonBegin**    	- Дата и время начала лекци в секундах
* int **secondsLessonDuration** 	- Продолжительность лекции в секундах
* object **scheduleData**         - Параметры нового пункта в расписании (см. models.schedule)
* function **callback**           - в аргумент передается object: {success:true, data:newSchedule}, либо {success:false, errorCode:999}

**Метод возвращает**: 
boolean: true - успешно создано, false не создано

##### Пример:

```javascript
Scheduler.addSchedule(
	Scheduler.toSeconds('22-04-2017-19:00'), // 22 апреля 2017 в 19:00
	3600, // Продолжительность лекции - 1 час
	{ 
		lessonId:"lesson2", // лекция 
		lecturerId:"lecturer1", // лектор
		roomId:"room1", // аудитория
		schools:[ "school1", "school3" ] // школы 
	}, 
	function(result) { 
		if (false === result.success) {
			console.log(Scheduler.ERRORS.getErrorName(result.errorCode) ); 
		} 
		else { 
			console.log(result.data._id);
		}
	}
);
```


### Удалить лекцию
```javascript
removeScheduleById((id [, callback])
```

* string **id**       	- ключ пункта в расписании
* function **callback** 	- будет выполнен в случае успешного удаления

**return** 
boolean: true - удалено, false - не удалено

##### Пример:

```javascript
Scheduler.removeScheduleById("5f7fa37e-e578-f2bd-edaf-8abeece7d604", myCallbackFunction);
```


### Обновление пункта в расписании
```javascript
updateSchedule: function(id, scheduleData [, callback])
```
* string **id** - id пункта в расписании
* objecet **scheduleData** - см. _models.schedule_

_Вернет тоже что и addSchedule_

##### Пример:
```javascript
Scheduler.updateSchedule(
	"5f7fa37e-e578-f2bd-edaf-8abeece7d604", // id пункта в расписании
	{
		// см. _models.schedule_
		// Здесь может быть любое поле из модели, а также новое поле (кроме _id, оно ни на что не повлияет). 
		"lecturerId":"lecturer3" // новый лектор
        }, 
	function(result) { 
		if (!result.success) {
			console.log(Scheduler.ERRORS.getErrorName(result.errorCode) ); 
		}
		else { 
			console.log(result.data._id);
		}
	}
);
```



## Приложения написанные с Sheduler + YaSchool:
#### Расписание занятий Яндекс школ (сохранение в localStorage, рендеринг на Vuejs). 
* Демо: https://cybri0nix.github.io/ya-mobilization/ 
* Исходный код: https://github.com/cybri0nix/ya-mobilization
