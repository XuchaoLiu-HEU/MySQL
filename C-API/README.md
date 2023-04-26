#  [MySQL C API](https://dev.mysql.com/doc/c-api/8.0/en/)

```cpp
#include<stdio.h>
#include<mysql.h>
#include<iostream>
#include<iomanip>
using namespace std;
//#pragma comment(lib,"libmysql.lib")

int main1()
{
	char host[] = "localhost";
	char dbName[] = "new_schema";
	const char username[] = "root";
	const char password[] = "1234";
	unsigned int port = 3306;
	MYSQL* conn;
	conn = mysql_init(NULL);
	if (!conn)
	{
		printf("初始化失败\n");
	}
	conn = mysql_real_connect(conn, host, username, password, dbName, port, NULL, 0);
	if (conn)
		printf("连接成功\n");
	else
		printf("连接失败\n");

	// 执行MySQL查询
	if (mysql_query(conn, "SELECT * FROM dept")) {
		cerr << "Error: " << mysql_error(conn) << endl;
		return -1;
	}

	// 获取查询结果
	MYSQL_RES* result = mysql_store_result(conn);
	if (!result) {
		cerr << "Error: " << mysql_error(conn) << endl;
		return -1;
	}

	// 处理查询结果
	MYSQL_ROW row;
	while ((row = mysql_fetch_row(result))) {
		cout << left << setw(3) << setfill(' ') << row[0];
		cout << left << setw(12) << setfill(' ') << row[1];
		cout << left << setw(11) << setfill(' ') << row[2] << endl;
	}

	// 释放查询结果对象
	mysql_free_result(result);

	mysql_close(conn);
	//getchar();
	return 0;
}


int main()
{
	char host[] = "localhost";
	char dbName[] = "new_schema";
	const char username[] = "root";
	const char password[] = "1234";
	unsigned int port = 3306;
	MYSQL* conn;
	conn = mysql_init(NULL);
	if (!conn)
	{
		cout << "Error: " << mysql_error(conn);
		exit(1);
	}
	conn = mysql_real_connect(conn, host, username, password, dbName, port, NULL, 0);
	if (conn)
	{
		cout << "Connected to MySQL database." << endl;

		// 创建学生表
		mysql_query(conn, "DROP TABLE students if exict;");
		mysql_query(conn, "CREATE TABLE students (id INT NOT NULL AUTO_INCREMENT, student_id VARCHAR(10) NOT NULL, name VARCHAR(30) NOT NULL, class VARCHAR(10) NOT NULL, PRIMARY KEY (id));");
		cout << "Table created." << endl;

		// 插入学生信息
		mysql_query(conn, "INSERT INTO students (student_id, name, class) VALUES ('101', 'Tom', 'Class 1');");
		cout << mysql_affected_rows(conn) << " rows inserted." << endl;
		mysql_query(conn, "INSERT INTO students (student_id, name, class) VALUES ('102', 'Jerry', 'Class 2');");
		cout << mysql_affected_rows(conn) << " rows inserted." << endl;

		// 修改学生信息
		mysql_query(conn, "UPDATE students SET name='Tim' WHERE student_id='101';");
		cout << mysql_affected_rows(conn) << " rows updated." << endl;

		// 删除学生信息
		//mysql_query(conn, "DELETE FROM students WHERE student_id='102';");
		//cout << mysql_affected_rows(conn) << " rows deleted." << endl;

		// 删除学生表
		//mysql_query(conn, "DROP TABLE students;");
		//cout << "Table dropped." << endl;
	}
	else
	{
		cout << "Error: " << mysql_error(conn);
		exit(1);
	}
	mysql_close(conn);
	return 0;
}

```

