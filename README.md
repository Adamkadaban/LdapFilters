# LdapFilters
My notes on good LDAP filters for enumeration

Good guide to writing queries: [An Introduction to Manual Active Directory Querying with Dsquery and Ldapsearch](https://posts.specterops.io/an-introduction-to-manual-active-directory-querying-with-dsquery-and-ldapsearch-84943c13d7eb)


### users

Get properties of a user account
```
(&(objectCategory=person)(objectClass=user)(SamAccountName=sa1))
```

Return nested group membership of a user
```
(member:1.2.840.113556.1.4.1941:=CN=John Smith,DC=lab,DC=local)
```

Find all enabled user accounts
```
(&(objectCategory=person)(objectClass=user)(!(userAccountControl:1.2.840.113556.1.4.803:=2)))
```
Find all users that need to change their password on the next logon
```
(&(objectCategory=user)(pwdLastSet=0))
```

Find all users that have never logged in
```
(&(objectCategory=user)(lastlogon=0))
```

Find all users that are almost locked out
```
(&(objectCategory=user)(badPwdCount>=4))
```

Find all kerberoastable accounts
```
(&(objectClass=user)(servicePrincipalName=*)(!(cn=krbtgt))(!(userAccountControl:1.2.840.113556.1.4.803:=2)))
```

Find all asrep-roastable users
```
(&(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=4194304))
```

### groups

Return Domain Admins group
```
(&(objectclass=group)(cn=Domain Admins))
```

Return members of the Administrators group
```
(memberOf:1.2.840.113556.1.4.1941:=CN=Administrators,CN=Builtin,DC=lab,DC=local)
```
