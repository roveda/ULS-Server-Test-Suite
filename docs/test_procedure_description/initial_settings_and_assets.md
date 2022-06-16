# Initial Settings and Assets

TEST_MODULE: Initial Settings and Assets (INIT)

The test scripts use and expect specific settings and defined assets. 
They are defined in this chapter. This is the test of the administrative user interface.

## Firefox Profile
Set up a specific profile for testing ULS, use `English` as default language to get the same presentation as found in this guide.

Close all firefox sessions and open the dos box. Create a profile.
```
DOS:> "C:\Program Files (x86)\Mozilla Firefox ESR\firefox.exe" -CreateProfile "ULSTest D:\Temp\ULS\ulstest-moz-profile"
```

Start firefox using this profile

```
DOS:> "C:\Program Files (x86)\Mozilla Firefox ESR\firefox.exe" -P "ULSTest"
```
and change the language settings to English and close the session.

Then start firefox using this profile, in a private window and directly supply the URL:
```
DOS:> "C:\Program Files (x86)\Mozilla Firefox ESR\firefox.exe" -P "ULSTest" -private-window  "https://<ip-of-stage-server>:11976"
```

Use a desktop icon and edit its properties accordingly.
You must use the correct ip address. 

## Admin User

Log in to ULS as user "admin", switch to the _[admin menu] - user - create user_ and enter the following data:

| field               |  value  |
|:--------------------|:--------|
| username            | the_spyder |
| given name, surname | Spyder, Rafael |
| e-mail              | the_spyder@email.com <sup>(1)</sup> |
| random password     | [ ]             |
| password            | Abcd1234;:      |
| retype password     | Abcd1234;:      |
| rights              | [x] admin       |

<sup>(1)</sup> use an appropriate e-mail address, generally, your e-mail address

TEST: INIT 0100: create first user

## Domains

As `the_spyder` define the domains and sources. Verify that you must change your password on first login.

TEST: INIT 0110: change password on first login

Go to _[admin menu] - domain → edit domains_ and append the new domains.

Test specific domains must be found or defined.

domain   |    description
:--------|------------------------------
Woodlark |    A domain only for tests
Knowhere |    A domain only for tests

TEST: INIT 0120: creation of domains

## Sources (Servers)

Test specific sources of values (servers) must be defined. Go to _[server] → create server_ for each server in list below.

Add the sources, all sources must be defined with the ip-address of the ULS stage server, all sources are mocked. 
Use the ip-address of your server from where the tests are run!

serverid   | servername | domain    | serverclass | ip-address | secondary ip-address | accept ip-addresses
-----------|------------|-----------|-------------|------------|----------------------|-----------
ulstest1   | testsrv11  |  Knowhere | prod        | 10.1.2.3   |                      |
ulstest2   | testsrv12  | Knowhere  | stage       |            |    10.1.2.3          |
ulstest3   | testsrv21  | Woodlark  | prod        |            |                      | 10.1.2.3
ulstest4   | testsrv22  | Woodlark  | test        |            |                      | 10.1.2.3/32 <sup>1</sup>

<sup>1</sup> means: only exactly this ip-address, this is the default

Check that all domains and sources have been created correctly. Check the server from the command line on the stage server:
```
$ flush_test_values -t -h testsrv11
OK
$ flush_test_values -t -h testsrv12
OK
$ flush_test_values -t -h testsrv21
OK
$ flush_test_values -t -h testsrv22
OK
```
(in specific situations you may have to use:
```
$ flush_test_values -t -h testsrv11 -u <ip_of_local_server>:11976)
```

TEST: INIT 0130: creation of servers

Go to _[server] - edit server_ and search for 

| field               |  value  |
|:--------------------|:--------|
| servername | testsrv21   |

Should show the result:

serverid | hostname  | domain   |  state  | description | ip-address | first  seen | last seen
---------|-----------|----------|---------|-------------|------------|-------------|-----------
ulstest3 | testsrv21 | Woodlark | active  |             | 0.0.0.0    |             | 

Click on ulstest3 to edit the server details.

| field               |  value  |
|:--------------------|:--------|
| description         | Test server for ULS
| contact             | Sarli Kosmarikos
| location            | Clayson's star

TEST: INIT 0140: sources, edit of servers

## Access Code Descriptions

Go to _[admin menu] -> edit access_ change description for access-code "all" to "domain" and the other entries to all lowercase characters.

name       |  retention time | description       
-----------|-----------------|---------------------
all        |  14             |  domain             
adm        |  90             |  admin              
sec        |  366            |  security           
ulsadmlog  |  90             |  uls admin actions  

TEST: INIT 0150: change description for access-code

## Groups

Define test specific groups. Go to _groups -> create group_

Be sure to show _[all domains]_, including those freshly created.
(domain|domain read-only)

| group    | rights on domain "Knowhere" | rights on domain "Woodlark" |
| ------------ | ------------------------------------ | ------------------------------------ |
| AllRounder   | domain                               | domain                               |
| Databasics   | domain                               | domain read-only                     |
| Hotliners    | none                                 | domain read-only                     |
| Systemaniacs | domain read-only                     | domain                               |

TEST: INIT 0160: define user groups

## Users

As `the_spyder`, define test specific users. Do not assign any domains or domain privileges. _[user] - create user_

### Standard Users

| field                 | value   |
| --------------------- | ------------------------------------------- |
| username              | AmieAction                                  |
| e-mail                | amieaction@email.com  <sup>1</sup>          |
| random password       | \[\_\]                                      |
| password              | Amie\_Action5                               |
| rights                | \[\_\] admin \[\_\] operator \[\_\] service |
| password settings     | \[x\] password never expires                |
| groups                | Databasics                                  |

| field                 | value                                       |
| --------------------- | ------------------------------------------- |
| username              | HeyJoe                                      |
| e-mail                | heyjoe@email.com <sup>1</sup>               |
| random password       | \[\_\]                                      |
| password              | Joe's-Hello                                 |
| rights                | \[\_\] admin \[\_\] operator \[\_\] service |
| password settings     | \[x\] password never expires                |
| groups                | Hotliners                                   |

| field                 | value                                       |
| --------------------- | ------------------------------------------- |
| username              | SystematicGuy                               |
| e-mail                | systematicguy@email.com <sup>1</sup>        |
| random password       | \[\_\]                                      |
| password              | N0thing2l0se                                |
| rights                | \[\_\] admin \[\_\] operator \[\_\] service |
| password settings     | \[x\] password never expires                |
| groups                | Systemaniacs                                |

<sup>1</sup> use an appropriate e-mail address (e.g. your own or a local e-mail address)

_edit user_ the_spyder and complete the groups to:

| field       | value                                          |
| ----------- | ---------------------------------------------- |
| username    | the_spyder                                     |
| groups      | AllRounder, Databasics, Hotliner, Systemaniacs |

Use _user overview_ to show the list of all users and verify all users for correct group membership (needs a bit longer). 
Verify especially that `the_spyder` has full rights on both domains and can change to the admin menu. 

| field       | value                                          |
| ----------- | ---------------------------------------------- |
| set filter        | [x] member of groups                       |
| filter username   | AmieAction,HeyJoe,SystematicGuy,the_spyder |


| user      | last access | rights | groups                                      | domain | access |
| ------------- | --------------- | ---------- | ----------------------------------------------- | ---------- | ---------- |
| AmieAction    | 0000-00-00      |            | Databasics                                      |            |            |
| HeyJoe        | 0000-00-00      |            | Hotliners                                       |            |            |
| SystematicGuy | 0000-00-00      |            | Systemaniacs                                    |            |            |
| the\_spyder   | 2021-05-21      | admin      | AllRounder, Databasics, Hotliners, Systemaniacs |            |            |

TEST: INIT 0170: define users and group membership

Extend the filter by

| field       | value                                      |
| ----------- | ------------------------------------------ |
| set filter  | [x] show grouprights                       |

and click _[OK]_

| user          | last access     | rights           | groups                                          | domain     | from group     | access           |
| ------------- | --------------- | ---------------- | ----------------------------------------------- | ---------- | -------------- | ---------------- |
| AmieAction    | 0000-00-00      |                  | Databasics                                      | Knowhere   | G              | domain           |
|               |                 |                  |                                                 | Woodlark   | G              | domain read-only |
| HeyJoe        | 0000-00-00      |                  | Hotliners                                       | Knowhere   | G              | domain read-only |
|               |                 |                  |                                                 | Woodlark   | G              | domain read-only |
| SystematicGuy | 0000-00-00      |                  | Systemaniacs                                    | Knowhere   | G              | domain           |
|               |                 |                  |                                                 | Woodlark   | G              | domain           |
| the\_spyder   | 2019-12-27      | admin            | AllRounder, Databasics, Hotliners, Systemaniacs | Knowhere   | G              | domain           |
|               |                 |                  |                                                 | Woodlark   | G              | domain           |

TEST: INIT 0172: group and domain membership of users

Every time you login as a different user, go to _[administration] - options_ and set options for the user:

| field          | value         |
| -------------- | ------------- |
| show username  | [x]    |

That will display the current user name on the right side of the upper frame.

### Security User

Add the security user, not bound to a group _[user] - create user_

| field                 | value                                       |
| --------------------- | ------------------------------------------- |
| username              | SecurityAdmin                               |
| e-mail                | securityadmin@email.com <sup>1</sup>        |
| random password       | \[x\]                                       |
| password              |                                             |
| rights                | \[\_\] admin \[\_\] operator \[\_\] service |
| password settings     | \[x\] password never expires                |
| groups                | Databasics                                  |

rights on domain "Knowhere" 

domain read-only | admin          | security read-only | uls admin actions
---------------- | ---------------| ------------------ | -------------------
\[\_\]  \[x\]    |  \[x\]  \[\_\] | \[\_\]  \[x\]      |  \[x\]  \[\_\] 


rights on domain "Woodlark" 

domain read-only | admin          | security read-only | uls admin actions
---------------- | ---------------| ------------------ | -------------------
\[\_\]  \[x\]    |  \[\_\]  \[x\] |  \[x\]  \[\_\]     | \[\_\]  \[x\] 

TEST: INIT 0180: define security user and privileges

_[change password]_ and set the password to `VerySecre+`.

TEST: INIT 0183: define security user and privileges

Use _user overview_ for 

| field           | value                                     |
| --------------- | ------------------------------------------|
| set filter      | \[x\] security,  \[x\] security read-only |
| filter username | SecurityAdmin                             |


| user          | last access     | rights  | domain     | access           |
| ------------- | --------------- | ------- | ---------- | ---------------- |
| SecurityAdmin | 0000-00-00      |         | Knowhere   | domain read-only, admin, security read-only, uls admin actions           |
|               |                 |         | Woodlark   | domain read-only, admin read-only, security, uls admin actions read-only |

TEST: INIT 0185: group and domain membership of security user

## Notification Destinations

CONT HERE

