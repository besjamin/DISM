
Create a Folder in C: drive named, "24H2" with the following subfolders as your staging environment. 

![image](https://github.com/user-attachments/assets/9f7b1a47-9a87-44cf-b119-07644d0f7851)

Download the language/feature on demand packs that you will be using to inject into the windows image.

	• https://learn.microsoft.com/en-us/azure/virtual-desktop/windows-11-language-packs#create-a-content-repository-for-language-packages-and-features-on-demand

![image](https://github.com/user-attachments/assets/1073865f-46f9-46ae-af42-8255e88564c9)


	• After obtaining the ISO, mount it and transfer the necessary packages from the ISO to your designated staging folder at "C:\24H2\langfod".
	
![image](https://github.com/user-attachments/assets/edae8c5c-7303-447a-bf2d-c1aed1c6af6e)

	
	• Mount the Windows 11 ISO, navigate to "sources" folder and copy the "install.wim" wim file to the root of the C:\24H2 folder.
		
		○ Now that we have our staging folder ready, we can go ahead and inject the language and feature on-demand packages. 

	• Open CMD in Administrator mode, navigate to "C:\24H2\" and run the following commands.
	
		○ You must determine the appropriate WIM index for injecting these packages, we need to inject them into the Windows 11 Education WIM index. To determine this, run the following command.
		
		dism /get-wiminfo /wimfile:c:\install.wim
		
![image](https://github.com/user-attachments/assets/92048737-e842-4e94-9c42-48b5433e4620)
		
		As you can see, it's Index:1 Windows 11 Education.
		
		○ Now we have to mount the wim into our staging directory, run the following command:
	
		dism /mount-wim /wimfile:"C:\24H2\image.wim" /index:1 /mountdir:C:\24H2\mount
		
![image](https://github.com/user-attachments/assets/f5392657-7017-459b-9dca-1a43bb3689b3)
	
		
		○ No we need to inject the language packs and fod demand packs into the wim.
		
		dism /image:"C:\24H2\mount" /Add-Package /PackagePath="C:\24H2\langfod"
		
![image](https://github.com/user-attachments/assets/07a204b0-043c-4d6e-9a80-fcb751829af2)
		
		
		○ As we've injected the packages successfully. Our image is ready to be added into SCCM/MDT for Building and Capturing. We can now go ahead and commit the changes, to do so run this command.
		
		dism.exe /Unmount-Image /MountDir:C:\24H2\mount /commit
		
![image](https://github.com/user-attachments/assets/f215ce99-8b35-4281-a57d-84ee26f7fb73)
		
		
	Language pack will not apply unless you apply the latest Cumulative Update/s.
