# DISM
This repo will provide instructions on how to inject Cumulative and Patches, en-AU language and Feature on Demand packages


Create a Folder in C: drive named, "23H2" with the following subfolders as your staging environment. 

![image](https://github.com/user-attachments/assets/7b60f230-2eea-430b-b8e5-e133119553c6)



Download the language/feature on demand packs that you will be using to inject into the windows image.

	• https://learn.microsoft.com/en-us/azure/virtual-desktop/windows-11-language-packs#create-a-content-repository-for-language-packages-and-features-on-demand

![image](https://github.com/user-attachments/assets/01fe16ee-a5d2-4bd4-b527-df53679a1010)



	• After obtaining the ISO, mount it and transfer the necessary packages from the ISO to your designated staging folder at "C:\23H2\langfod".
	
	![image](https://github.com/user-attachments/assets/95a769ee-cfd4-4b4e-aff6-3b208fa190ab)

	
	• Mount the Windows 11 ISO, navigate to "sources" folder and copy the "install.wim" wim file to the root of the C:\23H2 folder.
		
		○ Now that we have our staging folder ready, we can go ahead and inject the language and feature on-demand packages. 

	• Open CMD in Administrator mode, navigate to "C:\23H2\" and run the following commands.
	
		○ You must determine the appropriate WIM index for injecting these packages, we need to inject them into the Windows 11 Education WIM index. To determine this, run the following command.
		
		dism /get-wiminfo /wimfile:c:\install.wim
		
![image](https://github.com/user-attachments/assets/e72d7ee9-d248-4e9b-9a7c-48c1110cfd37)


		
		As you can see, it's Index:1 Windows 11 Education.
		
		○ Now we have to mount the wim into our staging directory, run the following command:
	
		dism /mount-wim /wimfile:"C:\23H2\image.wim" /index:1 /mountdir:C:\23H2\mount
		
		
		
		○ No we need to inject the language packs and fod demand packs into the wim.
		
		dism /image:"C:\23H2\mount" /Add-Package /PackagePath="C:\23H2\langfod"
		
		
		
		○ As we've injected the packages successfully. Our image is ready to be added into SCCM/MDT for Building and Capturing. We can now go ahead and commit the changes, to do so run this command.
		
		dism.exe /Unmount-Image /MountDir:C:\23H2\mount /commit
		
		
		
		
Language pack will not apply unless you apply the latest Cumulative Update/s.![image](https://github.com/user-attachments/assets/d11c2996-d3a2-430a-b4a9-f4f0fc832571)
