# Goal

The goal is to re-add an export of an OpenLDAP DIT into its own DIT, without
using slapcat and slapadd. You can select only a part of your DIT you want to
reimport by using regular OpenLDAP filters.

# Why ?

Because slapadd doesn't support adding structural attributes like **memberOf**,
and the **overlay memberof** is only used when OpenLDAP is started, I build
this tools as a workaround of adding **memberOf** for all users.

It will the same for any other overlay that support structural attributes
dynamically added.

# Usage

This is an example of the default usage :

```bash
ldapreadd -x -b "dc=example,dc=com" -D "cn=admin,dc=example,dc=com" -H ldap://localhost -w password -LLL '(&(objectclass=*)(uid=user1))'
```

# Notes

* The program will store the entries that match the filter with ldapsearch,
delete them with ldapdelete, and re-import with the command ldapadd.

* This parameters will be interpreted by **ldapsearch**, and the options *-b* and
*-L* will be automatically remove for the command **ldapdelete** and
**ldapadd**.
