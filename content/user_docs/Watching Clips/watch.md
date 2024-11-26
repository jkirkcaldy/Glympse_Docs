---
title: Watching clips
type: docs
prev: /user_docs/watch
weight: 2
---

![Watch Clips](/images/watch_clips.png)
This is where you will watch through the clips in Glympse. 

When the page loads, it will automatically load the first clip into the player. To start playing you can press the big play button in the centre. 



### The video Player

#### Timecode
Glympse uses the timecode from the media files. This is so that it matches the timecode that your editor sees within Avid or their NLE of choice. 

*It is possible that the timecode in Glympse may be a frame off, this is because the timecodes must be calculated rather than read from the file. It will never be more that one frame off the timecode in the NLE.*


The timecode of the clip is displayed in two places. The seek bar:
![Timecode 1](/images/timecode1.png)

And there is a timecode box in the top left:
![Timecode 2](/images/timecode2.png)

This timecode box can be moved anywhere in the player window by dragging it to where you'd like it to be. You can reset the position of the timecode or disable the timecode box using the [user settings menu.](/user_docs/user_settings)


#### Audio Selection
For clips that have more than one track of audio, you can select the track to play back by clicking on the headphone icon on the playbar and selecting the audio track. Glympse by default creates a full mix of all the audio channels and plays this by default. 
![Audio Tracks](/images/audio_select.png)

#### Theatre Mode
![Theatre Mode](/images/theatre_mode.png)
Clicking the theatre mode button will expand the player to take 100% the width of the browser window. Clicking the theatre mode button again will return the player back to its normal size. 

#### Full Screen
Click this button for full screen playback
![Full Screen](/images/full_screen.png)

#### Rotate
Some clips like GoPro files can be the wrong orientation. Click the rotate button to rotate the clip by 180°
![Rotate](/images/rotate.png)

Click the restore rotate to return the clip to its original orientation. 
![Restore Rotate](/images/restore_rotate.png)

#### Keyboard shortcuts

When on this page there are a number of keyboard shortcuts you can use. 
J, K, L are shuttle controls to speed up or change the direction of play. You may have to click in the player window to enable these controls. Holding the Shift Key will also increase the amount the play is sped up or slowed down. 

M will add a marker at the current time of the clip. Markers will appear for everyone who has access to the production. 

### Selecting Clips
You can select the clip you'd like to view from the table at the bottom of the screen. Click the title of the clip to load it into the player. 

You can also navigate through the clips by pressing the Next/Prev buttons at the bottom of the player. This will load the next or previous clip. 

The title of the currently playing clip is shown at the bottom of the player and the middle of the menu bar at the top. 

### Related Clips
When clips are loaded into Glympse, the system will scan for the start timecode and the end timecode of a clip. This allows it to show all the related clips of the clip you are playing. The related clips box onthe right of the player will update whenever the source of the video player changes. You can automatically hide or show the related clips dropdown using the [user settings menu.](/user_docs/user_settings)

In this example the clip being played belongs to the Main camera and there are two GoPro cameras that have clips with overlapping timecodes. 

![Related](/images/related.png)

Clicking on the title of these related clips will play it in the current page, allowing you to quickly view clips from the other cameras. When you're viewing a related clip, the title in the menu bar and below the player will change showing you that it's playing a related clip then it will list the card followed by the clip title. 
![Related](/images/related_clip.png)
As well as this, the prev/next buttons will disappear from below the player. You can select the next related clip by clicking on the clip title or you can return to the original clip by selecting it in the table at the bottom again. This will re-enable the next prev buttons and you can resume watching the camera clips as normal. 

You can also navigate to any related cards from the related menu in the manu bar. This will show you camera cards from the same camera operator shot on the same day. 

### Markers and Tags
You can add markers or tags to any clip within Glympse. 

#### Markers
Markers allow you to mark a specific place in a clip. They will be displayed in the marker window next to the player and clicking on them will take you to the correct position in the clip. 

Markers are shown to everyone who has access to the clip. 

#### Tags
Tags are to tag an entire clip. You can tag more than one clip at a time. There is also a setting in the user settings menu that will automatically add the tags to any related clips. 

Tags are available to everyone who has access to the clips. 


## Menu Bar
![Menu](/images/menu.png)
The menu bar may show different options depending on what permissions you have. For example, the Download button will only appear if you have been give download permission for your production. 

### File
From the file menu you can send a clip to be transcribed, edit the metadata of the card, submit the card for re-transcode or report issues with the card. 

#### Transcribe
Select the clip or clips you would like to transcribe by checking the box in the table and click file then transcribe. This will send the selected clip(s) to the transcription engine. You can learn more about how this works [here.](/user_docs/transcription)

#### Edit metadata
From this menu you can change some of the metadata for the card. Anything you change here will be applied to every clip in the card. 

You can change the date, the card name, or the shooter. You can also hide the card which will stop it from showing on the rushes page. Anyone with a link to the card would still be able to access the clips. This will *only* stop the card from appearing in the rushes page. Users with access to the production can also unhide any card from the metadata edit pages. 

#### Request Retranscode
This will send the entire card back through the transcode engine. It allows you to report any issues with the card and manually submit the clips to the transcoder. The transcode will only work if the original clips are still available. 

#### Report Issues
This allows you to report issues with the current card. But this option will not automatically retranscode the clips. They would need to be manually transcoded by the admin team. 

### Related
This will show any cards that were shot by the same camera operator on the same day as the current card. Clicking one of these will take you load that current card. 

### Download
If you have been given the download permissions for your production, you can download the original clip by selecting the clips from the table with the checkbox and clicking download. These files can be very large and may take a long time to download. 

### Help
This will open these help pages. 

### Settings
This will open the user settings menu. [Click here to learn more.](/user_docs/user_settings)


In the centre of the menu bar, the title of the currently loaded clip will be displayed. 