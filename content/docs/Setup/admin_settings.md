---
title: Django Admin Settings
next: /docs/setup/admin_frontend
---
The admin settings page is where you can change almost everything. If it's in the database, it's likely you can make changes here. 

## System Settings
# Whisper Model
[See here for more information](https://github.com/openai/whisper?tab=readme-ov-file#available-models-and-languages)\
Default: small

| Size 	    | Parameters 	| English-only model 	|Multilingual model 	|Required VRAM 	|Relative speed     |
| --------- | ------------- | --------------------- | --------------------- | ------------- | --------------    |  
| tiny 	    | 39 M 	        | tiny.en 	            |tiny 	                |   ~1 GB 	    |    ~10x           |
| base 	    | 74 M 	        | base.en 	            |base 	                |   ~1 GB 	    |    ~7x            |
| small 	| 244 M 	    | small.en 	            |small 	                |   ~2 GB 	    |    ~4x            |
| medium 	| 769 M 	    | medium.en 	        |medium 	            |   ~5 GB 	    |    ~2x            |
| large 	| 1550 M 	    | N/A 	                |large 	                |   ~10 GB 	    |    1x             |
| turbo 	| 809 M 	    | N/A 	                |turbo 	                |   ~6 GB 	    |    ~8x            |

## Authentication and Authorization
Here you can manage the advanced settings of the users and Groups. Most of these settings can be changed from the front end without coming into the Django Admin page. 

## Impersonate 
Here you can view the logs for the impersonation.

## Periodic Tasks. 
These are created when installed. Should you wish to change the time the tasks run, you would do so in the Periodic Tasks section. 

### Clocked
These run once at a specific date and time. 

### Cron Tabs
This is where you can set advanced schedules using cron. [See here for more information about Cron](https://crontab.guru/)

### Intervals
These will allow you to let a task run at specific intervals, e.g. run once every hour. 

### Periodic Tasks
This is where you register or change the tasks, To change the schedule, you will need to create the schedule using one of the above options and then select it in this page. 

You can also enable and disable tasks here as well as override the priority of the task. 

## Review
### Review Files
This is where you can see all the files that have been uploaded for review. You can retranscode or delete the files from here. 

### Share Links
Add, edit or delete the share links for files here. 

## Rushes Management
You can make changes to the metadata of the clips or add or remove the productions here. But most of these tasks can and should be done in the front end.

### Base Productions
Add or edit the base productions. You can also change the group that the Base Production is assigned to. 

### Production
Here you can make changes to the production such as the name, friendly name, filepath etc. You can also trigger some tasks from this page such as the file scan or the transcoder. 

### clips
Here you can see and edit all the metadata for the clips in the Glympse database. 


## Transcribe
### Clips for Transcription
Here you can see all of the clips that have been sent to the transcription engine. You can trigger tasks such as re-transcribe or delete the files from here. 



