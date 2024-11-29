---
title: Glympse Admin
prev: /docs/setup/admin_frontend
---
You can manage most of the admin settings from the front end. When a user is assigned admin permissions or is joined to the "hub" group they will have access to the admin menu on the webUI.

## Dashboard
Access the dashboard page. This give you an overview of the system. It will show some figures like how many clips are waiting to be transcoded or how many clips have been transcribed. It also shows you all running tasks and will also show who is currently online.

You can also trigger the periodic tasks from the bottom of this page. 

## Logs
View the system logs. 

## Add Production
This brings up the add production modal where you can add a production. This is the same interface as pressing the add production button on the home page. 

## Edit Production
Here you can edit productions.
#### Production Admin
This is who is responsible for the production. It's likely that this should be the edit assistant assigned to the production. You can change this here, this person will receive the emails after the production has been scanned. 

#### Cover Image
Click the image to upload a new thumbnail image for the production. Or reset it. 

#### Active
An active production will be scanned for new rushes. A deactivated production will still be available to users.

#### Archive
This will remove the production from being able to be viewed. All files will remain, they will just be hidden. 

#### Export
This will export a json file containing the database entries for this production that can be archived to another system along with the media files. The media files will have to be manually backed up from your server. Once you have backed up the json file and the media, you can delete the production. 

#### Delete
This will delete the production and all the clips associated with it. 

## Manage Users

### Edit Users
![Manage Users](/images/Manage_Users.png)
Select a user from the dropdown menu and you can assign the user to a group. You can also deactivate the user which will keep the account but not allow the user to log in. Or you can delete the user which will delete their account completely. 

If the user was created by logging in via SSO, they will still be able to log in once they have been deleted. But they will have no permissions and won't be assigned to a group so will not have access to any productions. 

You can enable the ability for users to scan or start a production transcoder. They will only be able to scan or transcode the productions they have access to. 

Clicking on the number beside their name at the top will enter the impersonation for this user. This is useful if you need to check a user has the correct permissions. 

You can add a local user by clicking the blue + button at the top. 

### Edit Group Permissions
![Manage Groups](/images/Manage_groups.png)
When a base production is added, a new group of the same name will be created. 

From this page you can select a group and assign productions to it. Users assigned to this group will then be able to access these productions. 

You can add a new group by clicking the blue + button at the top right.

From this page you can add productions to a group. Adding a production to a group will give every user in that group access to all the productions in the group. 

## Settings
This will open the django admin setting page. 

## Send Email
Here you can send an email to registered users. 
Either select an existing email draft or click new to create a new one. 

This will take you to the editor page. Set the Subject at the top, then select a date and time to send the email. Not setting this will send immediately. 

Select the recipients of the email and draft the email below. 

The editor is a WYSIWYG editor so the email body will appear exactly as it does in this editor. You can add images and graphics to the email body should you wish. 

Click save and preview. This will show you a copy of what the email will look like when sent. You will be able to either schedule the send or edit the email again after this point. 

Click Delete to delete the email draft. 

## Clear Cache
This clears the cache of the system. Useful if there have been large database changes. By default the system caches data for around 10 minutes. 

Users may have to force refresh their browser to clear their local cache if the changes aren't appearing for them. 