基本概念：

1. 自习室：用于学生自习的场所，包括普通教室、图书馆的专用自习室以及其他区域的自习场地。场地的可用性由管理员统一管理。

2. 自习座位：自习室内登记在册的座位。有唯一编号，可按需预定。座位的可用性由管理员统一管理。

原始需求：

1. 为了方便学生自习，学校开放了一批自习室。自习室中的座位利用率不高，经常出现一座难求，同时又有书包占座人却不在的情况。为了提升座位的利用率和周转率，学校一方面考虑动态调整自习室的数量，另一方面也希望采用信息技术手段提升座位的利用率。

人员功能：

1. 管理员可以设定可用的自习室以及开放时间。
2. 管理员通过查看自习室和座位的预约情况来调整管理策略，提升资源利用率。
3. 学生则可以查看可用自习室，并预约自习时间。为了提高座位利用率并考虑减少学生连续学习造成的疲劳，每次预约均以整点小时为单位，最多一次性预约4小时（系统参数可调）

座位管理：

1. 每个座位目前都是普通座位，为了便于预约，会给予唯一编号并在桌面进行标记（比如贴一个号码，但目前还没有这个号码，需要自己数第几排哪个座）。未来计划将这个标记做成号码和二维码一体化标签，从而便于预约的学生签到。
2. 为了便于学生在线资料获取和学习，靠近固定插座或者移动导轨插座的座位有会有特殊标记，以便学生携带电子设备使用。

签到与抢座：

1. 学生按预约到达座位后，最好有签到功能。签到应当需要考虑位置信息，比如用手机小程序或类似机制。引入签到功能后，在预约时间之前15分钟会有推送（邮件推送或微信服务号推送等）提醒；在预约时间之后10分钟如果还未签到，则会再次提醒；预约时间15分钟后如果还未签到，则自动取消预约、释放座位，并提醒学生，同时后台记录一次违约。
2. 在任意一个时间点，如果当前有空闲的座位，学生发现自己就在附近并且希望使用的话，可以马上抢位并根据整点时间段选择使用时间。在有签到功能的情况下，必须在5分钟内签到，从而减少座位资源的浪费。

其他功能：

1. 为了便于使用，学生可以对所需的座位进行搜索，也可以查看历史预定并再次预定该座位。
2. 为方便留学生使用，希望有多语言的版本，可以便捷切换界面语言。





1. 管理员可以设定可用的自习室

2. 管理员可以设定自习室开放时间，一般是早7点到晚10点开放，不排除将来有个别教室会通宵开放

3. 学生则可以查看可用自习室，并预约自习时间

4. 每次预约均以整点小时为单位，最多一次性预约4小时（系统参数可调）

5. 签到功能，签到应当需要考虑位置信息

6. 在预约时间之前15分钟会有推送提醒

7. 在预约时间之后10分钟如果还未签到，则会再次提醒

8. 预约时间15分钟后如果还未签到，则自动取消预约、释放座位

9. 预约时间15分钟后如果还未签到，提醒学生，同时后台记录一次违约

10. 学生可以马上抢位并根据整点时间段选择使用时间，必须在5分钟内签到

11. 学生可以对所需的座位进行搜索

12. 可以查看历史预定并再次预定该座位







1. 自习室 (StudyRoom)

- id (主键, INT): 自习室的唯一标识符
- name (VARCHAR): 自习室的名称
- capacity (INT): 自习室的座位数量
- open_time (TIME): 自习室的开放时间
- close_time (TIME): 自习室的关闭时间
- is_available (BOOLEAN): 自习室是否可用，可由管理员设置

1. 座位 (Seat)

- id (主键, INT): 座位的唯一标识符
- study_room_id (外键, INT): 关联自习室表，表示座位所在的自习室
- seat_number (INT): 座位的编号
- is_near_power (BOOLEAN): 座位是否靠近电源插座

1. 用户 (User)

- id (主键, INT): 用户的唯一标识符
- username (VARCHAR): 用户名（学号）
- email (VARCHAR): 用户的邮箱
- user_type (ENUM): 用户类型（管理员、学生）
- notification_method (ENUM): 推送通知方式（邮件推送、微信服务号推送等）
- violation_count (INT): 用户的违约次数

1. 预约 (Reservation)

- id (主键, INT): 预约的唯一标识符
- user_id (外键, INT): 关联用户表，表示预约的用户
- seat_id (外键, INT): 关联座位表，表示预约的座位
- start_time (DATETIME): 预约开始时间
- end_time (DATETIME): 预约结束时间
- check_in_time (DATETIME): 签到时间
- check_in_location (VARCHAR): 签到位置信息，例如经纬度坐标
- status (ENUM): 预约状态（已预约、已签到、已取消、违约）

1. 系统参数 (SystemParameter)

- id (主键, INT): 系统参数的唯一标识符
- parameter_name (VARCHAR): 系统参数名称
- parameter_value (VARCHAR): 系统参数值
- description (VARCHAR): 系统参数描述



```sql
CREATE TABLE study_room (
  sr_id INT AUTO_INCREMENT PRIMARY KEY,
  sr_name VARCHAR(255) NOT NULL,
  sr_capacity INT NOT NULL,
  sr_open_time TIME NOT NULL,
  sr_close_time TIME NOT NULL,
  sr_is_available BOOLEAN NOT NULL
);

CREATE TABLE seat (
  seat_id INT AUTO_INCREMENT PRIMARY KEY,
  seat_study_room_id INT NOT NULL,
  seat_number INT NOT NULL,
  seat_is_near_power BOOLEAN NOT NULL,
  UNIQUE (seat_study_room_id, seat_number)
);

CREATE TABLE usr (
  usr_id INT AUTO_INCREMENT PRIMARY KEY,
  usr_username VARCHAR(255) NOT NULL,
  usr_email VARCHAR(255) NOT NULL,
  usr_password VARCHAR(255) NOT NULL,
  usr_type ENUM('student', 'admin') NOT NULL,
  usr_notification_method ENUM('email', 'wechat') NOT NULL,
  usr_violation_count INT NOT NULL
);

CREATE TABLE reservation (
  rsv_id INT AUTO_INCREMENT PRIMARY KEY,
  rsv_user_id INT NOT NULL,
  rsv_seat_id INT NOT NULL,
  rsv_start_time DATETIME NOT NULL,
  rsv_end_time DATETIME NOT NULL,
  rsv_check_in_time DATETIME,
  rsv_status ENUM('reserved', 'checked_in', 'canceled', 'no_show') NOT NULL,
  rsv_check_in_location VARCHAR(255)
);

CREATE TABLE system_parameter (
  sp_id INT AUTO_INCREMENT PRIMARY KEY,
  sp_parameter_name VARCHAR(255) NOT NULL,
  sp_parameter_value VARCHAR(255) NOT NULL,
  UNIQUE (sp_parameter_name)
);

INSERT INTO system_parameter (sp_parameter_name, sp_parameter_value) VALUES ('max_reservation_duration', '4');

CREATE INDEX reservation_seat_time ON reservation(rsv_seat_id, rsv_start_time, rsv_end_time);

```

