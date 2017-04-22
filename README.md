# Scheduler

[Scheduler.js](https://github.com/cybri0nix/scheduler/blob/master/libs/scheduler.js) - Библиотека для работы с расписанием лекций. Рядом с ней вы найдете реализацию библиотеки работы со справочниками лекций, лекторов, школ и аудиторий. Реализация может быть любой, главное чтобы структура данных была "понятной" для Scheduler'а ([YaSchool](https://github.com/cybri0nix/scheduler/blob/master/libs/yaschool.js)).


## Быстрый старт
### Иннициализация библиотеки
```
Scheduler.init({
	data: scheduleData,
	directories: referenciesData
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
[  
  { _models.schedule_ },
  { _models.schedule_ }
  ...
  { _models.schedule_ }
]
```
**Структура объекта _models.schedule_**. Каждый такой обект содержит информацию о запланированной лекции:
```
{
  "schools"	: { _YaSchool.models.school_ },
  "rooms"		: { _YaSchool.models.room_ },
  "lessons"	: { _YaSchool.models.lesson_ },
  "lecturers"	: { _YaSchool.models.lecturer_ },
}
```

В исходном коде [YaSchool](https://github.com/cybri0nix/scheduler/blob/master/libs/yaschool.js) вы найдете описание моделей 

**_YaSchool.models.school_** . Это объект, который описывает школу:
```

```




## Возможности

#### Запланировать лекцию


## Пример использования

```

```
