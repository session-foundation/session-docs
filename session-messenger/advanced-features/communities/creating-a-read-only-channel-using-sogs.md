# Creating a read-only channel using SOGS

Some SOGS operators may want to set up a room on their SOGS which features one way style communication; these rooms allow moderators and administrators to update the room with new messages, but disallows unprivileged users to send new messages. These sorts of rooms can be useful for providing updates or information.

#### 1. Create a new room

```
sogs --add-room TOKEN --name "NAME" --description "DESCRIPTION"
```

{% hint style="success" %}
Skip to step 3 if you already have a room and you are an administrator in that room
{% endhint %}

Replace `TOKEN` with the address to use in the room URL (which must consist of **ONLY** lowercase letters, numbers, underscores, or dashes), replace `NAME` with the room name to display in Session and optionally replace `DESCRIPTION` with a short description of the topic of the room.

#### 2. Ensure you are an administrator of the room <a href="#docs-internal-guid-fe64838c-7fff-6888-e9ea-b2b202b8895d" id="docs-internal-guid-fe64838c-7fff-6888-e9ea-b2b202b8895d"></a>

```
sogs --room TOKEN --admin --add-moderator ACCOUNTID
```

Replacing `TOKEN` with the room token you created in step 1, and replacing `ACCOUNTID` with your own Account ID.

#### 3. Alter room permissions <a href="#docs-internal-guid-e93e33b1-7fff-ffd4-731c-731b867c5d01" id="docs-internal-guid-e93e33b1-7fff-ffd4-731c-731b867c5d01"></a>

There are 4 types of permission currently available in rooms

* r = Read (ability to read messages)
* w = Write (ability to write new messages)
* u = Upload (ability to upload attachments, including files, voice notes, images and videos)
* a = Access (ability to join the room)

When modifying these permissions changes will only apply to non admins/moderators, the default permissions in a room will be "rwua" for all users

To create a read only group we want to remove regular users ability to write and upload attachments, we can do this using the following command:

```
sogs --add-perms ra --remove-perms wu --room TOKEN
```

This command will ensure read and access permissions are provided and remove write and upload permissions for regular users.

As the permission changes do not affect administrators and moderators, they will still be able to read, write and upload to the group. New Account ID's can be given write/upload access by making those Account ID's moderator or admin, this can be done directly in the Session Desktop client or via the[ CLI](https://github.com/oxen-io/session-pysogs/blob/dev/administration.md).
