# openldap

我們使用 [osixia/docker-openldap](https://github.com/osixia/docker-openldap):

```sh
docker pull osixia/openldap:1.2.4
```

- Search

```sh
ldapsearch -h localhost -p 10389 -x -D cn=admin,dc=example,dc=com -w secret -b dc=example,dc=com
```

## Migration from ApacheDS to OpenLdap

- export ldif

```sh
ldapsearch -v -h <apacheds-ip> -p 10389 -x -D uid=admin,ou=system -w secret -b dc=example,dc=com > my.ldif
```

- import

```sh
ldapadd -v -h <openldap-ip> -p 10389 -x -D cn=admin,dc=example,dc=com -w secret -f my.ldif -c > /dev/null 2>&1
```

> import 會需要執行好幾次, 因為 parent 要先建立才能往下建

- validation

```sh
ldapsearch -h <apacheds-ip> -p 10389 -x -D uid=admin,ou=system -w secret -b dc=example,dc=com

# 找到最下面
# numResponses: 12008
# numEntries: 12007

ldapsearch -h <openldap-ip> -p 10389 -x -D cn=admin,dc=example,dc=com -w secret -b dc=example,dc=com

# 一樣找到 Entries 比數要跟原本的一樣
```
