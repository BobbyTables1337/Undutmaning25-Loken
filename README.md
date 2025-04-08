# Undutmaning25-Loken
WriteUp for CTF-Challenge "Löken" for Undutmaning25 CTF

# Layout
We are given two files to download along with a brief description.
![image](https://github.com/user-attachments/assets/7b4ffbe7-e032-4475-8a35-cc8588716833)

The first file (USB.zip) seems to be an encrypted file of some kind.

![image](https://github.com/user-attachments/assets/bdd058b7-3982-47ab-8003-c46a944cd169)

The other file (WIN10HDD.zip) is what appears to be a saved user profile from a Windows box along with some files from the OS itself.

![image](https://github.com/user-attachments/assets/1756f3c3-6dd3-4ffd-ab63-9b8137b765db)

We dig deeper into the profiles folders and we find Documents, images and a Firefox profile.

On the Desktop, we also find a file (NotesForMyself.odt) and a password.zip file.
The notes file contains some clues in the metadata.
![image](https://github.com/user-attachments/assets/20aafb98-96ba-4399-8071-69b476f3d685)

OK, password to password.zip is the Windows user password, and also we are hinted that we should consider building rules och wordlists based on certain criterias.

We dig around som more in the Pictures folder, and find the windows password inside one of the images.

![image](https://github.com/user-attachments/assets/f4715c5e-6677-49e4-aba0-17241a68691d)
![image](https://github.com/user-attachments/assets/d4cb52a5-9630-4005-8807-d360770abd67)

The password from the image is correct and we are able to open the passwords.zip.

![image](https://github.com/user-attachments/assets/37450028-0b1a-4bd1-909f-4410376caba4)

This guy really seem into space and satellite stuff, lets see what the Firefox profile can give us.
We use firepwd (http[s]://github.com/lclevy/firepwd) and we provide the password we used to open the passwords.zip
![image](https://github.com/user-attachments/assets/865ea073-e307-47f7-87e9-10d58d50e572)
![image](https://github.com/user-attachments/assets/aad0ab6e-2c3a-4aa7-956b-f358d3eecc5e)

So we get a few more passwords, most of them seem to be [satellitename][Year] and with or without a finishing [!] .

Also in the firefox profile we find some search history and downloads.





Let's continue by digging deeper into the Windows catalog.

![image](https://github.com/user-attachments/assets/dfc4a621-4e76-4beb-9990-057fbff12486)

SAM and SYSTEM files, let's see what we can find with samdump2 - hashes nice!

![image](https://github.com/user-attachments/assets/a602ab67-59b2-4b62-a356-6a735a13ec28)


