# Undutmaning25 - CTF challenge "Löken"
WriteUp

We (CTF-Team Shellskapsresan) worked on this during the CTF Undutmaning 2025 but we didnt finish it during the CTF, but it got me so frustrated I had to revisit it afterwards to solve it, so here is a small quick and dirty writeup.

# Let's go
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

We dig around som more in the Pictures folder, and find the windows password inside one of the images (OIP (5) - kopia.jfif)
.
![image](https://github.com/user-attachments/assets/d4cb52a5-9630-4005-8807-d360770abd67)

The password from the image is correct and we are able to open the passwords.zip.

![image](https://github.com/user-attachments/assets/37450028-0b1a-4bd1-909f-4410376caba4)

This guy really seem into space and satellite stuff, lets see what the Firefox profile can give us.
We use firepwd (http[s]://github.com/lclevy/firepwd) and we provide the password we used to open the passwords.zip

![image](https://github.com/user-attachments/assets/865ea073-e307-47f7-87e9-10d58d50e572)
![image](https://github.com/user-attachments/assets/aad0ab6e-2c3a-4aa7-956b-f358d3eecc5e)

So we get a few more passwords, most of them seem to be [satellitename][Year] and with or without a finishing [!] .

Also in the firefox profile we find some search history and downloads using DumpZilla (http[s]://github.com/Busindre/dumpzilla)

Search history indicating wanting to use Veracrypt for hiding things, along with thoughts on inner and outer volumes.

![image](https://github.com/user-attachments/assets/8359e6b3-96b4-4de1-9bc7-cb0277808fa4)

We can also see the specific version of Veracrypt beeing downloaded.

![image](https://github.com/user-attachments/assets/c547c960-7504-4e5f-860a-e6db635cf3e7)

# Recap
So now let us look at what we have so far.
1. The USB.zip gives a file named Evilplans wich most likely is a Veracrypt 
version 1.26.15 volume consisting of a inner and outer volume.
2. We have a bunch of passwords that indicates a preferred password format beeing [satellitename][year] and an optional [!]

Let's continue by digging deeper into the Windows catalog.

![image](https://github.com/user-attachments/assets/dfc4a621-4e76-4beb-9990-057fbff12486)

SAM and SYSTEM files, let's see what we can find with samdump2 - hashes nice!

![image](https://github.com/user-attachments/assets/a602ab67-59b2-4b62-a356-6a735a13ec28)

Some work with john and we get to verify that the windows password (space1337) actually was the windows password.

# Looking at breaking into the Veracrypt volume(s).

Since opening a Veracrypt volume is a slow operation throwing 1000's of attempts at it would take too much time we have to try it small first.

Building a wordlist out of the passwords we have seen so far and adding years from 2019 to 2025 and adding ! will take long enough without going the "all the names in the spreadsheet" - route.

Part of the generated passwordfile:

![image](https://github.com/user-attachments/assets/977a1535-59fb-4629-800a-5f3ff9d78059)

Then throwing that at the encrypted volume and getting a snack..... 

![image](https://github.com/user-attachments/assets/d4dbc0b8-f17c-4042-ab8e-5c0dbb123075)

Then opening the volume with password: Viking2023!

![image](https://github.com/user-attachments/assets/507deca6-8859-4637-840d-793dc300a3c1)

We have more stuff!

![image](https://github.com/user-attachments/assets/fa9684a3-c72d-45ff-b6b4-24a30c5fabfe)

Content of the textfile.

![image](https://github.com/user-attachments/assets/f5f116b1-c0d4-49c4-82e1-e247a1841200)

When visiting the URL: https://www.youtube.com/watch?v=dQw4w9WgXcQ we get Rickrolled, and perhaps so did you.

After some time and frustration spent on trying to analyze the URL, the Youtube video and searching for clues in the comment section (perhaps Harriet or Kenneth posted there) we end up with nothing.

Nothing.

Nothing..

But frustration.

So time to take a step back, and read the content of the file againt.

![image](https://github.com/user-attachments/assets/f5f116b1-c0d4-49c4-82e1-e247a1841200)

Hiding something deeper indicates a inner volume, an inner volume is opened just by using a different password, the textfile actually says "Pw above"

Testing the Rickroll URL AS the password for the volume....

![image](https://github.com/user-attachments/assets/7d5edee1-197c-4730-8a48-7430ab47bc11)

# BOOM

![image](https://github.com/user-attachments/assets/2c72f1f3-b4c7-4277-932c-e92e0f8001f8)

The flag

![image](https://github.com/user-attachments/assets/383cf9bc-a78e-4a6d-b89d-aed63a026c9f)


# Thoughts

This was a frustrating challenge, and it only got 3 solves during the competition.

There should be a different route to opening the outer volume by using hashcat and trying to crack the first 512 bytes, cant tell if its faster but try if you want to.

Big thanks to the creators of the CTF, it was just as frustrating as we expect from this yearly event :)

Thanks to my mates in Shellskapsresan who make CTF's fun to participate in!

// BobbyTables1337
