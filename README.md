# LdapFilters
My notes on good LDAP filters for enumeration

Return Domain Admins group
```
(&(objectclass=group)(cn=Domain Admins))"
```

Get properties of a user account
```
(&(objectCategory=person)(objectClass=user)(SamAccountName=sa1))
```

Return members of the Administrators group
```
(memberOf:1.2.840.113556.1.4.1941:=CN=Administrators,CN=Builtin,DC=lab,DC=local)
```

Return nested group membership of a user
```
(member:1.2.840.113556.1.4.1941:=CN=John Smith,DC=lab,DC=local)
```

Find all enabled user accounts:
```
(&(objectCategory=person)(objectClass=user)(!(userAccountControl:1.2.840.113556.1.4.803:=2)))
```

Find all kerberoastable accounts:
```
(&(servicePrincipalName=*)(UserAccountControl:1.2.840.113556.1.4.803:=512)(!(UserAccountControl:1.2.840.113556.1.4.803:=2))(!(objectCategory=computer)))"
```
