# MySQL isolation levels

## Table of contents

- [MySQL isolation levels](#mysql-isolation-levels)
  - [Table of contents](#table-of-contents)
  - [Running](#running)

## Running

`docker-compose.yaml` file:

```yaml

services:
  mysql:
    image: mysql:8.4
    container_name: isolation-levels-mysql
    # NOTE: use of "mysql_native_password" is not recommended: https://dev.mysql.com/doc/refman/8.0/en/upgrading-from-previous-series.html#upgrade-caching-sha2-password
    # (this is just an example, not intended to be a production configuration)
    command: --default-authentication-plugin=mysql_native_password
    restart: always
    ports:
      - 3306:3306
    environment:
      MYSQL_ROOT_PASSWORD: root # Use "root" for both user and password
```
