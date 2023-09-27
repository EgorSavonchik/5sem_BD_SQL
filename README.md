# Савончик Егор, 153501
## Используемая технология
Postgre sql
## Функциональные требования
* Пользователь может зайти как студент или как супер пользователь.
* Супер пользователь может составлять расписание.
* Студент может просматривать свое расписание.
## Список таблиц
### Пользователи
* Описание
  + Хранит данные для входа студента или суперпоользователя
* Поля
  + user_id (Primary Key, INT, Auto-increment) - уникальный идентификатор пользователя
  + username (VARCHAR(50)) - логин
  + password (VARCHAR(50)) - пароль
  + is_superuser (BOOL) - является ли пользователь супер пользователем
* Связи
  + один к нулю или одному с таблицей Студенты
### Студенты
* Описание
  + Хранит данные о студентах учебного заведения
* Поля
  + student_id (Primary Key, INT, Auto-increment) - уникальный идентификатор студента
  + group_id (Foreign Key, INT) - группа в которой состоит студент
  + user_id (Foeign Key, INT) - логин и пароль студента
  + first_name (VARCHAR(50)) - имя студента
  + last_name (VARCHAR(50)) - фамилия студента
  + email (VARCHAR(50)) - электронная почта студента
  + phone_number (VARCHAR(50)) - номер телефона студента
* Связи
  + многие к одному связь с таблицей Группы(через поле group_id)
  + многие ко многим связь с Курсы(через таблицу course_registration)
  + многие ко многим связь с Экзамены(через связанную таблицу)
  + один или ноль к одному с таблицей Пользователи
### Курсы
* Описание
  + Хранит данные о курсах учебного заведения
* Поля
  + course_id (Primary Key, INT, Auto-increment) - уникальный идентификатор курса
  + teacher_id (Foreign Key, INT) - преподаватель, который ведет курс
  + course_name (VARCHAR(50)) - название курса
  + course_description (VARCHAR(1000)) - описание курса
  + number_of_hours (INT) - количество часов курса
* Связи
  + многие к одному связь с таблицей Преподаватели(через поле teacher_id)
  + многие ко многим связь с Студенты(через таблицу course_registration)
  + один ко многим связь с Экзамены
  + один ко многим связь с Расписание
  + один к одному связь с Материалы курса
### Экзамены
* Описание
  + Хранит результаты экзаменов
* Поля
  + exam_id (Primary Key, INT, Auto-increment) - уникальный идентификатор экзамена
  + student_id (Foreign Key, INT) - студент, сдавший экзамен
  + course_id (Foreign Key, INT) - курс, по которому был экзамен
  + exam_date (DATETIME) - дата экзамена
  + gradle (INT) - оценка за экзамен
* Связи
  + многие к многим связь с таблицей Студенты(через связанную таблицу)
  + многие к одному связь с таблицей Курсы(через поле course_id)
### Аудитории
* Описание
  Хранит информацию об аудиториях
* Поля
  + classroom_id (Primary Key, INT, Auto-increment) - уникальный идентификатор аудитории
  + room_number (VARCHAR(50)) - номер аудитории
  + building_name (VARCHAR(50)) - название корпуса
* Связи
  + один ко многим связь с Расписание
### Расписание
* Описание
  + Хранит информацию о расписании занятий
* Поля
  + schedule_id (Primary Key, INT, Auto-increment) - уникальный идентификатор расписания
  + course_id (Foreign Key, INT) - курс, занятие которого, происходит
  + classroom_id (Foreign Key, INT) - аудитория, в которой проходит занятие
  + day_of_week (week_day) - день недели
  + start_time (TIME) - начало занятия
  + end_time (TIME) - конец занятия
* Связи
  + многие к одному связь с Курсы(через поле course_id)
  + многие к одному связь с Аудитории(через поле classroom_id)
### Преподаватели
* Описание
  + Хранит информацию о преподавателях
* Поля
  + teacher_id (Primary Key, INT, Auto-increment) - уникальный идентификатор преподавателя
  + first_name (VARCHAR(50)) - имя преподавателя
  + last_name (VARCHAR(50)) - фамилия преподавателя
  + email (VARCHAR(50)) - электронная почта преподавателя
  + phone_number (VARCHAR(50)) - телефон преподавателя
* Связи
  + один ко многим связь с Курсы
### Группы студентов
* Описание
  + Хранит информацию о группах студентов
* Поля
  + group_id (Primary Key, INT, Auto-increment) - уникальный идентификатор группы
  + group_name (VARCHAR(50)) - номер группы
* Связи
  + один ко многим связь с Студенты
### Регистрация на курсы
* Описание
  + Хранит регистрации студентов на курсы
* Поля
  + registration_id (Primary Key, INT, Auto-increment) - уникальный идентификатор регистрации
  + student_id (Foreign Key, INT) - студент, зарегистрировавшийся на курс
  + course_id (Foreign Key, INT) - курс, на который была произведена регистрация
  + registration_date(DATETIME, DEFAULT CURRENT_TIMESTAMP) - дата регистрации на курс 
* Связи
  + многие к одному связь с Студенты(черещ поле student_id)
  + многие к одному связь с Курсы(через course_id)
### Материалы курса
* Описание
  + Хранит материалы курса
* Поля
  + material_id (Primary Key, INT, Auto-increment) - уникальный идентификатор материалов курса
  + course_id (Foreign Key, INT) - курс, для которого предназначены метериалы
  + title (VARCHAR(50)) - название материала
  + content (VARCHAR(5000)) - содержание материала
* Связи
  + один к одному связь с Курсы(через course_id)
