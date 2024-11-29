---
title: Troubleshooting
weight: 10
---

If you should run into any problems here are some steps you can take to resolve them.

## Clips Don't play
If clips don't play, all users can submit the roll to be retranscoded. This should be a self serve resolution. 

navigate to the roll that has issues, click file then request re-transcode. This will allow users to add a message as to why they are resubmitting the clips. 

Once submitted the roll will automatically be transcoded. This should fix most issues. 

## Glympse hasn't discovered new files
Anyone in the admin groups can navigate to a production and trigger a scan of the production files. 

Navigate to the production and click `scan files` 
![Scan files](/images/scan_files.png)

From the [Edit users](/docs/user_management/manage_users) page, it is possible to give users the ability to trigger a rescan or to trigger the transcoder for their projects. 

### Files are not found even after triggering a manual scan
If you have scanned the production but new files are not appearing, it may be that there was a sync issue with the file scanner and the database. 

Glympse keeps track of what files it has already discovered in a json format for each production. This allows it to skp over files it has previously scanned and only process newly discovered files. It is possible that if a file scan takes place whilst files are being copied that this may get out of sync. 

From the production page, if you hold the `alt` key you can Force Scan the files. This will delete the tracked state of the production and scan the entire production again. This will pick up any files that are not already in the database. 

Existing files inthe database will remain through this process so it is considered a safe opperation. However it will send all clips to be processed again. Exisiting clips should be skipped over as part of the processing. 

## Automatic tasks are not running
If the transcode or processing tasks are not running automatically, it is important to check that the celery beat worker is running. This is what keeps the scheduled tasks on schedule. IT is also important to check that there is only one instance of this running across your nodes. 

To check if it is running, on the container with the `ENABLE_CELERY_BEAT: yes` variable set, find the name of the container and enter the following command: `docker exec <GlympseContainerName> supervisorctl status`

If everything is running correclty you should see the line: `celery_beat    RUNNING   pid 29, uptime 8 days, 20:41:58`

If this is correct and the tasks are still not running, check the logs for any indication that the tasks are starting.  You can also check the django admin page to check the last run time. 

![periodic tasks](/images/periodic_tasks.png)

