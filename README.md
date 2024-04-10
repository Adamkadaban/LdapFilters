# LdapFilters
My notes on good LDAP filters for enumeration

Good guide to writing queries: [An Introduction to Manual Active Directory Querying with Dsquery and Ldapsearch](https://posts.specterops.io/an-introduction-to-manual-active-directory-querying-with-dsquery-and-ldapsearch-84943c13d7eb)

## Table of Contents

- [LdapFilters](#ldapfilters)
   * [Pre-build Queries](#pre-build-queries)
      + [users](#users)
      + [groups](#groups)
   * [Building a Query](#building-a-query)

## Pre-built Queries

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


## Building a Query

To compound 2 filters, you can use the `&`, as such:
```
(&(attribute1=value1)(attribute2=value2))
```

To negate a filter, you can use the `!` as such:
```
(&(attribute1=value1)(!(attribute2=value2)))
```

When filtering on an attribute, it is possible to use wildcards:
```
(&(objectClass=user)(name=*adm*))
```

See more at the [official documentation](https://learn.microsoft.com/en-us/archive/technet-wiki/5392.active-directory-ldap-syntax-filters)

LDAP has identifiers (called [matching rules](https://learn.microsoft.com/en-us/openspecs/windows_protocols/ms-adts/4e638665-f466-4597-93c4-12f2ebfabab5)) that allow you to do logical operations when making queries:


| Capability name                    | OID                     | AD Support | Explanation                                                                   |
| ---------------------------------- | ----------------------- | ---------- | ----------------------------------------------------------------------------- |
| LDAP_MATCHING_RULE_BIT_AND         | 1.2.840.113556.1.4.803  | >= 2000    | Bitewise AND                                                                  |
| LDAP_MATCHING_RULE_BIT_OR          | 1.2.840.113556.1.4.804  | >= 2000    | Bitwise OR                                                                    |
| LDAP_MATCHING_RULE_TRANSITIVE_EVAL | 1.2.840.113556.1.4.1941 | >= 2008    | Recursively search attributes (rather than only direct attributes)            |
| LDAP_MATCHING_RULE_DN_WITH_DATA    | 1.2.840.113556.1.4.2253 | >= 2012    | Match on portions of values of syntax Object(DN-String) and Object(DN-Binary) |
