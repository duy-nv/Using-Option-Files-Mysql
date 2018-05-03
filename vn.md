
# MySQL :: MySQL 5.7 Reference Manual :: 4.2.6 Using Option Files

### 4.2.6 Sử dụng các tệp tin cấu hình

Hầu hết các chương trình MySQL đều có thể đọc các tùy chọn khởi động từ các file tùy chọn (thường được gọi là các file cấu hình). Các file tuỳ chọn cung cấp 1 cách thuận tiện nhất để sử dụng các tùy chọn thường được chỉ định để chúng không cần phải nhập vào mỗi dòng lệnh khi bạn chạy một chương trình nào đó.

Để xác định xem một chương trình có đọc các tệp tùy chọn hay không, hãy gọi nó bằng tùy chọn `--help`. (Đối với mysqld, sử dụng `--verbose` và `--help`.) Nếu chương trình đọc các tập tin tùy chọn, thông báo trợ giúp cho biết các tập tin sẽ tìm và các nhóm tùy chọn nào sẽ nhận ra.

Lưu ý

Một chương trình MySQL thường được bắt đầu với tùy chọn `\--no-defaults`, không đọc bất kỳ file tùy chọn nào khác ngoài file `.mylogin.cnf`.

Nhiều tệp tùy chọn là các tệp văn bản thuần túy, được tạo bằng bất kỳ trình soạn thảo văn bản nào. Ngoại lệ là tệp `.mylogin.cnf` chứa các tùy chọn đường dẫn đăng nhập. Đây là một tệp được mã hóa được tạo bởi tiện ích [**mysql_config_editor**][4]. Xem Phần [Section 4.6.6, "**mysql_config_editor** — MySQL Configuration Utility"][4]. Một "Đường dẫn đăng nhập" là một nhóm tùy chọn chỉ cho phép một số tùy chọn nhất định: `host`, `user`, `password`, `port` và `socket`. Chương trình client chỉ định đường dẫn đăng nhập nào sẽ đọc từ `.mylogin.cnf` bằng cách sử dụng tùy chọn `--login-path`.

Để chỉ định tên tệp đường dẫn đăng nhập thay thế khác, hãy thiết lập biến môi trường `MYSQL_TEST_LOGIN_FILE`. Biến này được sử dụng bởi cung cụ kiểm tra **mysql-test-run.pl**, nhưng cũng được công nhận bởi [**mysql_config_editor**][4] và bởi các MySQL client như [**mysql**][6], [**mysqladmin**][7], và vv.

MySQL tìm kiếm các tệp tùy chọn theo thứ tự được mô tả trong phần thảo luận sau và đọc bất kỳ tệp nào tồn tại. Nếu một tệp tùy chọn mà bạn muốn sử dụng không tồn tại, hãy tạo nó bằng cách sử dụng phương thức thích hợp, như được vừa thảo luận.

Trên Windows, các chương trình MySQL đọc các tùy chọn khởi động từ các tệp được hiển thị trong bảng sau, theo thứ tự được chỉ định (các tệp được liệt kê đầu tiên được đọc trước tiên, rồi đến các file sau theo thứ tự ưu tiên).

**Bảng 4.1 Các file tùy chọn đọc trên các hệ điều hành Unix và giống Unix**

| File Name                                                                                  | Purpose                                                       |  
| ------------------------------------------------------------------------------------------ | ------------------------------------------------------------- |  
| ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.ini`, ``%PROGRAMDATA%`MySQLMySQL Server 5.7my.cnf` | Global options                                                |  
| ``%WINDIR%`my.ini`, ``%WINDIR%`my.cnf`                                                     | Global options                                                |  
| `C:my.ini`, `C:my.cnf`                                                                     | Global options                                                |  
| `_`BASEDIR`_my.ini`, `_`BASEDIR`_my.cnf`                                                   | Global options                                                |  
| `defaults-extra-file`                                                                      | The file specified with [`\--defaults-extra-file`][8], if any |  
| ``%APPDATA%`MySQL.mylogin.cnf`                                                             | Login path options (clients only)                             |  

Trong bảng trên,`% PROGRAMDATA%` thể hiện thư mục hệ thống tệp chứa dữ liệu ứng dụng cho tất cả người dùng trên máy chủ. Đường dẫn này mặc định là `C: \ ProgramData` trên Microsoft Windows Vista trở về sau và `C: \ Documents and Settings \ All Users \ Application Data` trên các phiên bản cũ hơn của Microsoft Windows.

`%WINDIR%` đại diện cho vị trí thư mục Windows của bạn.Nó thường là `C:WINDOWS`. Sử dụng lệnh sau để xác định vị trí chính xác của nó từ giá trị của biến môi trường `WINDIR`:

    
    C:> echo %WINDIR%

`%APPDATA%` đại diện cho giá trị của thư mục chứa dữ liệu của các ứng dụng trên môi trường Windows. Sử dụng câu lệnh sau để xác định vị trí chính xác của nó từ giá trị của biến môi trường `APPDATA`:

    
    C:> echo %APPDATA%

`_BASEDIR_` đại diện cho thư mục gốc cài đặt MySQL. Khi MySQL 5.7 được cài đặt bằng trình cài đặt MySQL,nó thường là `C:\PROGRAMDIR\MySQLMySQL 5.7 Server`, trong đó `PROGRAMDIR` đại diện thư mục của chương trình (thường là `Program Files` trên các phiên bản tiếng Anh của Windows), Xem thêm tại [Section 2.3.3, "MySQL Installer for Windows"][9]. 

Trên các hệ thống Unix và dựa trên Unix, các chương trình MySQL đọc các tùy chọn khởi động từ các tệp được hiển thị trong bảng sau, theo thứ tự được chỉ định (các tệp được liệt kê đầu tiên được đọc trước tiên, rồi đến các file sau theo thứ tự ưu tiên).

Lưu ý: 
Trên các nền tảng Unix, MySQL bỏ qua các tệp cấu hình có thể ghi toàn cục. Đây là mục đích an ninh.

**Table 4.2 Option Files Read on Unix and Unix-Like Systems**

| File Name               | Purpose                                                       |  
| ----------------------- | ------------------------------------------------------------- |  
| `/etc/my.cnf`           | Global options                                                |  
| `/etc/mysql/my.cnf`     | Global options                                                |  
| `_`SYSCONFDIR`_/my.cnf` | Global options                                                |  
| `$MYSQL_HOME/my.cnf`    | Server-specific options (server only)                         |  
| `defaults-extra-file`   | The file specified with [`\--defaults-extra-file`][8], if any |  
| `~/.my.cnf`             | User-specific options                                         |  
| `~/.mylogin.cnf`        | User-specific login path options (clients only)               |  

Trong bảng trên,`~` đại diện cho thư mục home của user hiện tại ( giá trị của biến `$HOME`).

`SYSCONFDIR` đại diện cho thư mục được chỉ định với tùy chọn [`SYSCONFDIR`][10] cho **CMake** khi MySQL được xây dựng. Mặc định, đây là thư mục `etc` đặt thư mục cài đặt đã được biên dịch.

`MYSQL_HOME` là một biến môi trường chứa đường dẫn đến thư mục chứa tệp `my.cnf` chỉ định của máy chủ.  Nếu `MYSQL_HOME` không được thiết đặt và bạn khởi động server sử dụng chương trình [**mysqld_safe**][11],[**mysqld_safe**][11] thiết lập nó thành _`BASEDIR`_, thư mục gốc cài đặt của MySQL.

_`DATADIR`_ thường là `/usr/local/mysql/data`, mặc dù điều này có thể thay đổi theo từng nền tảng và phương thức cài đặt. Giá trị của nó là vị trí của thư mục đi kèm khi MySQL được biên dịch, nó không phải vị trí được chỉ định bằng tùy chọn  [`\--datadir`][12] khi[**mysqld**][1] khởi động. Sử dụng [`\--datadir`][12] tại thời điểm chạy không ảnh hưởng gì đến việc tìm kiếm những file tùy chọn của máy chủ mà nó đọc trước khi sử lý các tùy chọn.

Nếu tìm thấy nhiều thể hiện cho một tùy chọn đã có, thế hiện cuối cùng sẽ được ưu tiên, với một ngoại lệ là: đối với **[mysqld**][1], thể hiện đầu tiên của tùy chọn `[\--user`][13] được sử dụng như một cách để đảm bảo an toàn bảo mật , để ngăn chặn một người dùng chỉ định ghi đè lên bằng dòng lệnh.

Mô tả sau đây về cú pháp áp dụng trong file tùy chọn mà bạn có thể sửa bằng tay. Nó không bao gồm file `.mylogin.cnf`, được tạo ra bằng cách sử dụng **[mysql_config_editor**][4] và nó được mã hóa.

Bất kỳ tùy chọn dài nào có thể được lấy bằng dòng lệnh khi chạy một chương trình MySQL thì cũng có thể được lấy từ một file tùy chọn. Để lấy danh sách các tùy chọn sẵn có, chạy nó với tùy chọn `\--help`. (Đối với **[mysqld**][1], sử dụng `[\--verbose`][2] and `[\--help`][3].)

Cú pháp để chỉ định các tùy chọn trong một tệp tùy chọn cũng tương tự như trong cú pháp dòng lệnh(xem tại [Section 4.2.4, "Using Options on the Command Line"][14])). Tuy nhiên, trong một file tùy chọn, bạn loại bỏ hai dấu gạch ngang đầu tiên từ phần tên của tùy chọn và bạn phải chỉ rõ mỗi tùy chọn trên 1 dòng. Ví dụ, `\--quick` và `\--host=localhost` trong dòng lệnh nên được chỉ định `quick` và `host=localhost` trên các dòng riêng biệt trông một file tùy chọn. Để chỉ định một tùy chọn của biểu mẫu `\--loose-_opt_name_` trong một file tùy chọn, viết nó như sau `loose-_`opt_name`_`.

Các dòng trống trong các tệp tùy chọn thì được bỏ qua. Các dòng không trống thì phải thuộc các định dạng sau:


* `#_`comment`_`, `;_`comment`_`

Dòng chú thích được bắt đầu bằng `#` hoặc `;`. một chú thích `#` cũng có thể được bắt đầu ở giữa dòng.

* `[_`group`_]`

`_group_` là tên của một chưong trình hay một nhóm mà bạn muốn thiết lập tùy chọn. Sau một dòng group, mọi dòng thiết lập tùy chọn đều được áp dụng cho nhóm đã được đặt tên cho tới khi kết thúc file tùy chọn hoặc có một dòng group khác được tìm thấy. Tên của nhóm tùy chọn không có phân biệt chữ hoa chữ thường.

* `_`opt_name`_`

Điều này tương đương với  `\--_`opt_name`_` trên chế độ dòng lệnh.

* `_`opt_name`_=_`value`_`

Điều này tương đương với `\--_`opt_name`_=_`value`_` trên dòng lệnh. Trong một file tùy chọn, bạn có thể có khoảng trống xung quanh ký tự `=`, nhưng điều này thường không đúng trên dòng lệnh. Giá trị của tùy chọn có thể được đặt trong dấu nháy đơn hoặc dấu nháy kép, nó sẽ hữu ích hơn nếu giá trị của nó chứa kí tự chú thích `#`.

Các khoảng trống ở đầu và cuối được tự động bị xóa đi trong những tên và giá trị tùy chọn.

Bạn có thể sử dụng các ký tự đóng như `b`, `t`, `n`, `r`, `\`, and `s` trong các giá trị tùy chọn để đại diện cho backspace, tab, newline, carriage return, backslash, and space characters. Trong các file tùy chọn, các quy tắc cho ký tự đặc biệt này như sau:

* Một ký tự backslash theo sau một ký tự chuỗi đánh dấu đặc biệt được chuyển đổi thành một ký tự đại diện được trình bày theo trình tự. Ví dụ như, `\s` được chuyển đổi thành dấu khoảng trắng.
* Một ký tự backslash không theo sau nó là một ký tự chuỗi thoát hợp lệ thì giá trị của nó sẽ không bị thay đổi. Ví dụ, `S`vẫn được giữ nguyên.

Các quy tắc trước đó có nghĩa là với một ký tự backslash có thể được coi như là `\`, hoặc là `` nếu nó không theo sau bởi một ký tự trình tự thoát hợp lệ.

Các quy tắc cho những chuỗi thoát trong các file tùy chọn khác so với những nguyên tắc cho các chuỗi thoát trong chuỗi ký tự của câu lệnh SQL. Trong trường hơp sau, nếu _`x`_ không phải là một ký tự chuỗi thoát hợp lệ,  `_`x`_` trở thành"_`x`_"thay cho`_`x`_`. Xem [Section 9.1.1, "String Literals"][15]. 

Các quy tắc thoát cho các giá trị file tùy chọn đặc biệt thích hợp đối với các tên đường dẫn trên môi trường Windows, sử dụng `` là dấu tách trên đường dẫn. Một dấu phân tách trên đường dẫn trong môi trường Windows là `\` nếu nó có một ký tự chuỗi thoát theo sau. Nó cũng có thể được viết là `\` hoặc `` .... Ngoài ra , dấu `/` có thể được sử dụng trong tên đường dẫn của Window và sẽ được coi như là ``. Giả sử bạn muốn chỉ định thư mục gốc `C:\Program Files\MySQL\MySQL Server 5.7` trong một file tùy chọn. Nó có thể được thực hiện theo nhiều cách khác nhau. Một vài ví dụ:

    
    basedir="C:Program FilesMySQLMySQL Server 5.7"
    basedir="C:\Program Files\MySQL\MySQL Server 5.7"
    basedir="C:/Program Files/MySQL/MySQL Server 5.7"
    basedir=C:\ProgramsFiles\MySQL\MySQLsServers5.7

Nếu tên của nhóm tùy chọn trùng với tên của chương trình, các tùy chọn trong group sẽ áp dụng riêng cho chương trình đó. Ví dụ các nhóm `[mysqld]` và nhóm `[mysql]` áp dụng lần lượt cho máy chủ **[mysqld**][1] và chương trình client**[mysql**][6].

Nhóm tùy chọn `[client]` được đọc bởi các chương trình client cung cấp trong các bản phân phối của MySQL (nhưng không phải bởi **[mysqld**][1]). Để hiểu cách mà các chương trình client bên thứ ba sử dụng C API lại có thể sử dụng các file tùy chọn, xem tài liệi C API tại [Section 27.8.7.50, "mysql_options()"][16].

Các nhóm `[client]` cho phép bạn chỉ định những tùy chọn nào được áp dụng cho tất cả các client. Ví dụ, `[client]` là một group thích hợp để chỉ định việc sử dụng mật khẩu cho kết nối tới server. (Nhưng hãy đảm bảo là các file tùy chọn chỉ cho phép bạn truy cập, vì thế những người khác không thể tìm ra mật khẩu của bạn). Hãy đảm bảo là không đặt một tùy chọn trong nhóm `[client]` trừ khi nó được công nhận đối với tất cả các chương trình client mà bạn sử dụng. Các chương trình không hiểu các tùy chọn sẽ thoát sau khi hiển thị thông báo lỗi nếu bạn cố gắng chạy chúng.

Liệt kê thêm những nhóm tùy chọn chung và các nhóm riêng cụ thể. Ví dụ, một nhóm `[client]` phổ biến hơn bởi vì nó được đọc bởi tất cả các chương trình client, trong khi một nhóm `[mysqldump]` lại chỉ được đọc bởi  **[mysqldump**][17]. Các tùy chọn cụ thể sau sẽ ghi đè các tùy chọn cụ thể được chỉ định trước đó, vì vậy hãy đặt các nhóm tùy chọn theo thứ tự `[client]`, `[mysqldump]` cho phép **[mysqldump**][17]- các tùy chọn cụ thể được ghi đè lên các tùy chọn của `[client]`.

Đây là một tệp tùy chọn toàn cục điển hình:

    
    [client]
    port=3306
    socket=/tmp/mysql.sock
    
    [mysqld]
    port=3306
    socket=/tmp/mysql.sock
    key_buffer_size=16M
    max_allowed_packet=8M
    
    [mysqldump]
    quick

Đây là một file tùy chọn người dùng điển hình:
    
    
    [client]
    # The following password will be sent to all standard MySQL clients
    password="my password"
    
    [mysql]
    no-auto-rehash
    connect_timeout=2

Để tạo ra các nhóm tùy chọn chỉ được đọc bởi các máy chủ **[mysqld**][1] từ hàng loạt các bản phát hành MySQL cụ thể, sử dụng các group với các tên như  `[mysqld-5.6]`, `[mysqld-5.7]`, và tương tự thế. Các nhóm sau đây chỉ ra rằng việc cài đặt `[sql_mode`][18] chỉ nên được sử dụng với các máy chủ MySQL có số hiệu phiên bản 5.7.x:

    
    [mysqld-5.7]
    sql_mode=TRADITIONAL

Có thể sử dụng chỉ thị`!include` trong các file tùy chọn để include các file tùy chọn khác và chỉ thị `!includedir` để tìm kiếm các thư mục cụ thể cho các file tùy chọn. Ví dụ, để include file `/home/mydir/myopt.cnf`, sử dụng chỉ thị sau:

    
    !include /home/mydir/myopt.cnf

Để tìm kiểm `/home/mydir` thư mục và đọc các file tùy chọn được tìm thấy ở đây, sử dụng chỉ thị sau: 
    
    
    !includedir /home/mydir
    
MySQL không đảm bảo về thứ tự các tệp tùy chọn trong thư mục sẽ được đọc

Lưu ý

Bất cứ file nào được tìm thấy và include vào bằng cách sử dụng chỉ thị `!includedir` trong các hệ điều hành Unix _must_có tên file kết thúc bằng `.cnf`. Trên Windows, chỉ thị này sẽ kiểm tra các file với phần mở rộng là `.ini` hoặc `.cnf`.

Viết nội dung của một file tùy chọn được include như các file tùy chọn khác. Nó nên chứa các nhóm tùy chọn, mỗi nhóm được đứng trước bởi 1 dòng `[_`group`_]` chỉ rõ chương trình mà các tùy chọn này áp dụng.

Trong khi 1 file được thêm đang được xử lý, chỉ những tùy chọn trong các nhóm mà chương trình hiện tại đang tìm kiếm để sử dụng. Thì các nhóm khác sẽ bị loại bỏ. Giả sử rằng file `my.cnf` chứa dòng:

    
    
    !include /home/mydir/myopt.cnf

Và giả sử là `/home/mydir/myopt.cnf` như sau:
    
    
    [mysqladmin]
    force
    
    [mysqld]
    key_buffer_size=16M

Nếu `my.cnf` được xử lý bởi **[mysqld**][1], chỉ nhóm `[mysqld]` trong `/home/mydir/myopt.cnf` được sử dụng. Nếu file được xử lý bởi **[mysqladmin**][7], chỉ có nhóm `[mysqladmin]` được sử dụng. Nếu file được xử lý bởi bất kỳ chương trình nào khác, không có tùy chọn nào trong `/home/mydir/myopt.cnf` được sử dụng

Chỉ thị `!includedir` được xử lý tương tự ngoại trừ tất cả các file tùy chọn trong thư mục được gọi tên được đọc.

Nếu 1 file tùy chọn bao gồm các chỉ thị `!include` hoặc `!includedir` các file được gọi bởi các chỉ thị đó được xử lý mỗi khi file tùy chọn được xử lý, không quan tâm đến việc chúng xuất hiện ở đâu trong file


[1]: https://dev.mysql.com/mysqld.html "4.3.1 mysqld — The MySQL Server"
[2]: https://dev.mysql.com/server-options.html#option_mysqld_verbose
[3]: https://dev.mysql.com/server-options.html#option_mysqld_help
[4]: https://dev.mysql.com/mysql-config-editor.html "4.6.6 mysql_config_editor — MySQL Configuration Utility"
[5]: https://dev.mysql.com/option-file-options.html#option_general_login-path
[6]: https://dev.mysql.com/mysql.html "4.5.1 mysql — The MySQL Command-Line Tool"
[7]: https://dev.mysql.com/mysqladmin.html "4.5.2 mysqladmin — Client for Administering a MySQL Server"
[8]: https://dev.mysql.com/option-file-options.html#option_general_defaults-extra-file
[9]: https://dev.mysql.com/mysql-installer.html "2.3.3 MySQL Installer for Windows"
[10]: https://dev.mysql.com/source-configuration-options.html#option_cmake_sysconfdir
[11]: https://dev.mysql.com/mysqld-safe.html "4.3.2 mysqld_safe — MySQL Server Startup Script"
[12]: https://dev.mysql.com/server-options.html#option_mysqld_datadir
[13]: https://dev.mysql.com/server-options.html#option_mysqld_user
[14]: https://dev.mysql.com/command-line-options.html "4.2.4 Using Options on the Command Line"
[15]: https://dev.mysql.com/string-literals.html "9.1.1 String Literals"
[16]: https://dev.mysql.com/mysql-options.html "27.8.7.50 mysql_options()"
[17]: https://dev.mysql.com/mysqldump.html "4.5.4 mysqldump — A Database Backup Program"
[18]: https://dev.mysql.com/server-system-variables.html#sysvar_sql_mode

  
