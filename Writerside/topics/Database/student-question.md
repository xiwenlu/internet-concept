# 学生问题

数据库使用mysql

```SQL
create table student_info
(
    id              int auto_increment primary key,
    student_name    varchar(32) null comment '姓名',
    idcard          varchar(32) null comment '身份证号',
    gender          tinyint     null comment '性别(女0男1)',
    birthday        date        null comment '生日',
    enrollment_time date        null comment '入学时间',
    create_time     datetime    null comment '创建时间',
    update_time     datetime    null comment '修改时间'
) comment '学生表';

create table course_info
(
    id          int auto_increment primary key,
    course_name varchar(32) null comment '课程名称',
    teacher_id  int         null comment '教师id',
    create_time datetime    null comment '创建时间',
    update_time datetime    null comment '修改时间'
) comment '课程表';

create table score_info
(
    id          int auto_increment primary key,
    student_id  int      null comment '学生id',
    course_id   int      null comment '课程id',
    score       tinyint  null comment '成绩',
    create_time datetime null comment '创建时间',
    update_time datetime null comment '修改时间'
) comment '成绩表';

create table teacher_info
(
    id           int auto_increment primary key,
    teacher_name varchar(32) null comment '姓名',
    idcard       varchar(32) null comment '身份证号',
    gender       tinyint     null comment '性别(女0男1)',
    birthday     date        null comment '生日',
    entry_time   date        null comment '入职时间',
    create_time  datetime    null comment '创建时间',
    update_time  datetime    null comment '修改时间'
) comment '教师表';

INSERT INTO student_info (id, student_name, idcard, gender, birthday, enrollment_time, create_time, update_time) VALUES (1, '张三', '110101199003078888', 1, '1990-03-07', '2010-09-01', now(), now());
INSERT INTO student_info (id, student_name, idcard, gender, birthday, enrollment_time, create_time, update_time) VALUES (2, '李四', '110102199107158888', 1, '1991-07-15', '2011-09-01', now(), now());
INSERT INTO student_info (id, student_name, idcard, gender, birthday, enrollment_time, create_time, update_time) VALUES (3, '王五', '110103199204238888', 0, '1992-04-23', '2012-09-01', now(), now());
INSERT INTO student_info (id, student_name, idcard, gender, birthday, enrollment_time, create_time, update_time) VALUES (4, '赵六', '110104199305318888', 0, '1993-05-31', '2013-09-01', now(), now());
INSERT INTO student_info (id, student_name, idcard, gender, birthday, enrollment_time, create_time, update_time) VALUES (5, '赵六', '110104199305318888', 0, '1993-05-31', '2013-09-01', now(), now());

INSERT INTO course_info (id, course_name, teacher_id, create_time, update_time) VALUES (1, '语文', 1, now(), now());
INSERT INTO course_info (id, course_name, teacher_id, create_time, update_time) VALUES (2, '数学', 2, now(), now());
INSERT INTO course_info (id, course_name, teacher_id, create_time, update_time) VALUES (3, '英语', 3, now(), now());


INSERT INTO score_info (id, student_id, course_id, score, create_time, update_time) VALUES (1, 1, 1, 90, now(),now());
INSERT INTO score_info (id, student_id, course_id, score, create_time, update_time) VALUES (2, 1, 2, 85, now(),now());
INSERT INTO score_info (id, student_id, course_id, score, create_time, update_time) VALUES (3, 1, 3, 95, now(),now());
INSERT INTO score_info (id, student_id, course_id, score, create_time, update_time) VALUES (4, 2, 1, 78, now(),now());
INSERT INTO score_info (id, student_id, course_id, score, create_time, update_time) VALUES (5, 2, 2, 89, now(),now());
INSERT INTO score_info (id, student_id, course_id, score, create_time, update_time) VALUES (6, 2, 3, 75, now(),now());
INSERT INTO score_info (id, student_id, course_id, score, create_time, update_time) VALUES (7, 3, 1, 82, now(),now());
INSERT INTO score_info (id, student_id, course_id, score, create_time, update_time) VALUES (8, 3, 2, 68, now(),now());
INSERT INTO score_info (id, student_id, course_id, score, create_time, update_time) VALUES (9, 3, 3, 92, now(),now());
INSERT INTO score_info (id, student_id, course_id, score, create_time, update_time) VALUES (10, 4, 1, 86, now(),now());
INSERT INTO score_info (id, student_id, course_id, score, create_time, update_time) VALUES (11, 4, 2, 79, now(),now());
INSERT INTO score_info (id, student_id, course_id, score, create_time, update_time) VALUES (12, 4, 3, 94, now(),now());

INSERT INTO teacher_info (id, teacher_name, idcard, gender, birthday, entry_time, create_time, update_time) VALUES (1, '张三', '420101198001010011', 1, '1980-01-01', '2010-05-01', now(), now());
INSERT INTO teacher_info (id, teacher_name, idcard, gender, birthday, entry_time, create_time, update_time) VALUES (2, '李四', '420101198502020022', 0, '1985-02-02', '2015-12-15', now(), now());
INSERT INTO teacher_info (id, teacher_name, idcard, gender, birthday, entry_time, create_time, update_time) VALUES (3, '王五', '420101199012120033', 1, '1990-12-12', '2020-07-25', now(), now());
INSERT INTO teacher_info (id, teacher_name, idcard, gender, birthday, entry_time, create_time, update_time) VALUES (4, '赵六', '420101197511110044', 1, '1975-11-11', '2017-11-25', now(), now());

```

## 查询出最近1个月入学学生信息

```SQL
select * from student_info where enrollment_time - interval 1 MONTH <= 30;
```
## 查询年龄大于14岁的前10名学生信息

```SQL
SELECT *  FROM student_info WHERE DATEDIFF(CURDATE(), birthday) / 365 > 14  ORDER BY DATEDIFF(CURDATE(), birthday) DESC  LIMIT 10;
```

## 每个学生有多少门课程？
```SQL
select student_id, count( distinct course_id) from score_info group by student_id ORDER BY COUNT(course_id);
```

## 计算每个学生的所有课程的平均成绩（补考的按最后成绩算）
score_info成绩表由于有补考，每门课程有多次成绩。
```SQL
select student_id, avg(score) from score_info where id in (select max(id) from score_info group by course_id,student_id) group by student_id order by avg(score)

```

## 查询重复的学生数据

```SQL
select * from student_info where id not in ( select max(id) from student_info group by idcard );

select * from student_info a where a.id != (select max(b.id) from student_info b where a.idcard = b.idcard);
```

## 统计出各个月份入学学生总数

```SQL
select date_format(enrollment_time,'%m'),count(1) from student_info group by date_format(enrollment_time,'%m');
```

## 查出学生id等于“1”的学生所有课程得分
```SQL
SELECT s.student_name, c.course_name, sc.score FROM score_info sc
JOIN student_info s ON sc.student_id = s.id
JOIN course_info c ON sc.course_id = c.id
WHERE s.id = 1;
```

## 查询平均分大于80的学生 {id="80_1"}

```SQL
select * from student_info where id in (
    select student_id from ( select student_id,count(1) as count ,sum(score) as num from score_info group by student_id ) as t where t.num > 80 * count
);

SELECT s.* FROM student_info s  
JOIN score_info sc ON s.id = sc.student_id  
GROUP BY s.id, s.student_name HAVING AVG(sc.score) > 80;
```

## 所有课程都大于80分的学生 {id="80_2"}
```SQL

select * from student_info where id not in (
    select distinct student_id from score_info where score <=80
);

select * from student_info where id in (
    select student_id from score_info group by student_id having min(score) > 80
);

```

## 查出所有课程得分都大于80分的女生姓名
```SQL
select * from student_info where id in (
    select student_id from score_info group by student_id having min(score) > 80
) and gender = 0;

SELECT s.student_name FROM student_info s
JOIN score_info sc ON s.id = sc.student_id
WHERE sc.score > 80
GROUP BY s.id, s.student_name, s.gender
HAVING COUNT(DISTINCT sc.course_id) = (SELECT COUNT(*) FROM score_info) AND s.gender = 0;

```


