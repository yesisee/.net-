### 一、Docker中运行sqlserver2022
  1. 先拉取SqlServer镜像文件
  ```sh
  docker pull mcr.microsoft.com/mssql/server:2022-latest
  ```
  2. 运行容器
  ```sh
  docker run -d -p 1433:1433 --restart always --name sql_server_2022 --hostname sql_server_2022 -e "MSSQL_SA_PASSWORD=Sa@123456" -v D:\sqlserver\data:/var/opt/mssql/data -v D:\sqlserver\log:/var/opt/mssql/log -v D:\sqlserver\secrets:/var/opt/mssql/secrets -e "ACCEPT_EULA=Y" mcr.microsoft.com/mssql/server:2022-latest
  ```
  `注意：密码的长度不少于8位，并且包含大小写字母数字特殊符号。`


### 二、Docker中运行mysql
  1. 先拉取mysql镜像文件
  ```sh
  docker pull mysql
  ```
  2. 运行容器
  ```sh
  docker run --restart=always --privileged=true -p 3306:3306 --name mysql -e MYSQL_ROOT_PASSWORD=123456 -e MYSQL_ROOT_HOST=% -d mysql
  ```
