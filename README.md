# OpenLDAP

我們使用 [osixia/docker-openldap](https://github.com/osixia/docker-openldap):

```sh
docker pull osixia/openldap:1.2.4
```

- Search

```sh
ldapsearch -h localhost -p 10389 -x -D cn=admin,dc=example,dc=com -w secret -b dc=example,dc=com
```

## Migration from ApacheDS to OpenLDAP

首先匯出原本 ApacheDS 的資料成 ldif 檔

```sh
ldapsearch -v -h <apacheds-ip> -p 10389 -x -D uid=admin,ou=system -w secret -b dc=example,dc=com > my.ldif
```

將 ldif 檔匯入 OpenLDAP

```sh
ldapadd -v -h <openldap-ip> -p 10389 -x -D cn=admin,dc=example,dc=com -w secret -f my.ldif -c > /dev/null 2>&1
```

> 這個 import 指令會需要執行好幾次, 因為在建立時 parent 不存在會跳過不會自動建立

驗證方式為, 查詢 ApacheDS 總 entries 數量:

```sh
ldapsearch -h <apacheds-ip> -p 10389 -x -D uid=admin,ou=system -w secret -b dc=example,dc=com
# 找到最下面, 如
# numEntries: 12007
```

要跟 OpenLDAP 總 entries 一致

```sh
ldapsearch -h <openldap-ip> -p 10389 -x -D cn=admin,dc=example,dc=com -w secret -b dc=example,dc=com
```
