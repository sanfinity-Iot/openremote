OpenRemote can use multiple realms (or tenants), which are actually separated projects in the same instance of OpenRemote, each with its own set of users and roles. Only the system admin can view all realms and (if needed create e.g. global rules across realms). You can use realms to separate assets which are for example handled by different installers. In that case each installer can acces his own realm. Within a single realm end-users, e.g. home owners can access only those assets, assigned to him. This way an installer realm can include multiple homes. An alternative is to assign end-users directly to a new realm per end-user.

## Realms
Realms can be added and enabled/disabled on the settings page of the manager.

## Roles
On the settings page, roles can be defined by selecting 'read' and 'write' access rights for multiple parts of the system. It's done per realms. A few important things to keep in mind:
* If you want to give a role 'write' access, select both the 'read' and 'write' options.
* Selecting all options gives users full access to the system. If you want users not to be able to change, users, roles, realms, do NOT select both 'admin' options (read and write)
* Never delete the admin user, change its rights, or change the password for the 'admin' in the UI, as this password is used across the system and changes will easily be irreversible.

## Users
Also on the settings page, users can be created, edited, deleted or (temporarily) disabled. Users can be assigned roles. This is also done per realm.