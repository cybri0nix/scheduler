# Scheduler

[Scheduler.js](https://github.com/cybri0nix/scheduler/blob/master/libs/scheduler.js) - Библиотека для работы с расписанием лекций. Рядом с ней вы найдете реализацию библиотеки работы со справочниками лекций, лекторов, школ и аудиторий. Реализация может быть любой, главное чтобы структура данных была "понятной" для Scheduler'а ([YaSchool](https://github.com/cybri0nix/scheduler/blob/master/libs/yaschool.js)).


## Быстрый старт
### Иннициализация библиотеки
```
Scheduler.init({
	data: scheduleData,
	directories: directoriesData
})
.on("afterScheduleAdded", function() {
	
})
.on("afterScheduleDeleted", function() {
	
})
.on("afterScheduleUpdated", function() {
	
});
```

**Сруктура объекта scheduleData** (одномерный массив объектов):
```
// Это массив объектов
var scheduleData = [  
  { _models.schedule_ },
  { _models.schedule_ }
  ...
  { _models.schedule_ }
]
```

**Структура объекта _models.schedule_**. Каждый такой обект содержит информацию о запланированной лекции:

```
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
```
{
  "schools"	: { _YaSchool.models.school_ }, // Справочник школ
  "rooms"	: { _YaSchool.models.room_ }, // Справочник аудиторий
  "lessons"	: { _YaSchool.models.lesson_ }, // Справочник лекций
  "lecturers"	: { _YaSchool.models.lecturer_ }, // Справочник лекторов
}
```

В исходном коде [YaSchool](https://github.com/cybri0nix/scheduler/blob/master/libs/yaschool.js) вы найдете описание моделей 

**_YaSchool.models.school_** . Это объект, который описывает школу:
```
{
	"_id"		: null, // string unique hash
	"title"		: "", // string - Полное название школы
	"studentsCount"	: 0, // int - количество студентов
	"shortTitle" 	: "" // string - сокращенное название школы
}
```

**_YaSchool.models.room_** . Это объект, который описывает аудиторию:
```
{
	"_id"		: null, // string unique hash
	"title"		: "", // string - название аудитории
	"capacity"	: 0,  // int - количество мест в аудитории
	"location"	: "" // string - как пройти в аудиторию
}
```

**_YaSchool.models.lesson_** . Это объект, который описывает лекцию:
```
{
	"_id"	: null, // string unique hash
	"title"	: "" // Название лекции
}
```

**_YaSchool.models.lecturer_** . Это объект, который описывает лектора:
```
{
	"_id"	: null, // string unique hash
	"name"	: "", // 
	"ava"	: "",
	"bio"	: "",

}
```



## Возможности

#### Запланировать лекцию


## Пример использования

```

```
