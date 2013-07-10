# Goal

The goal is to re-add an export of an OpenLDAP DIT into its own DIT, without
using **slapcat** and **slapadd**. You can select only a part of your DIT you
want to reimport by using regular OpenLDAP filters.

# Why ?

Because slapadd doesn't support the addition of structural attributes like
**memberOf**, and the **overlay memberof** is only used when OpenLDAP is
started, so I build this tools as a workaround to add **memberOf** attributes
for all users.

It will be the same for any other overlays that support dynamically added
structural attributes.

# Usage

This is an example of the default usage :

```bash
ldapreadd -x -b "dc=example,dc=com" -D "cn=admin,dc=example,dc=com" -H ldap://localhost -w password -LLL '(&(objectclass=*)(uid=user1))'
```

As we don't want to alter the default parameter of the ldap utilities, we set
options as environment variables.

* **FORCE=1** :  to by pass the confirmation mode, useful for scripting
* **VERBOSE=1** to display more informations, like list of DNs deleted and restored

```bash
FORCE=1 VERBOSE=1 ldapreadd -x -b "dc=example,dc=com" -D "cn=admin,dc=example,dc=com" -H ldap://localhost -w password -LLL '(&(objectclass=*)(uid=user1))'
```

Obviously please don't add spaces around = !

# Notes

* The program will store the entries that match the filter with ldapsearch,
  delete them with ldapdelete, and re-import with the command ldapadd.

* This parameters will be interpreted by **ldapsearch**, and the options *-b*
  and *-L* will be automatically remove for the command **ldapdelete** and
  **ldapadd**.
