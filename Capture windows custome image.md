## Customizing windows and prepare to take image
1. Install Windows with iso
2. Create a local admin account 
3. Do sysprep audit mode restart
4. Login to audit mode with system administrator account
5. Remove all local account
6. Customize windows with apps and function
7. Create a disk partition or Prepare USB for take .wim image file
7. Sysprep with OOBE Generilze, Shutdown
8. Boot computer with external WinPXE 
9. Press Shift + F10 to get into cmd prompt
10. Command prompt is on x:\sources> folder
11. Verify with your USB or disk partition for capture image
12. Use DISM command to capture image. For example, D is external USB or drive partition
	x:\Windows\System32> DISM /Capture-Image /ImageFile:"D:\image.wim" /CaptureDir:C:\ /Name:"ImageName"
13. Wait for completion
	
	