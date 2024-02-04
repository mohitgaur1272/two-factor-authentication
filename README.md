# two-factor-authentication
## implementing Two-Factor Authentication (2FA) for SSH on Ubuntu adds an extra layer of security to your server. Here's a step-by-step guide for setting up 2FA for SSH on Ubuntu:
## step 1: launch the instance on AWS cloud platform 
 and connect your instance on your local system by this command 
```
ssh -i "hello1.pem" ubuntu@ec2-13-232-245-103.ap-south-1.compute.amazonaws.com
```
### replace with your_private_key and your_public_key  
you will see your instance will be ssh easily

## step 2: update your system by this command
```
sudo apt update
```
then 
## Step 3: Install Two-Factor Authentication PAM Module
Open a terminal on your Ubuntu server

Install the libpam-google-authenticator package, which provides the necessary PAM module for 2FA:
```
sudo apt install libpam-google-authenticator -y
```
## step 4: Configuring SSH to Use Two-Factor Authentication

Open the SSH daemon configuration file using a text editor. In this example, I'll use vim, but you can use any text editor you're comfortable with:
```
sudo vim /etc/ssh/sshd_config
```
### Find the line that says "ChallengeResponseAuthentication no" ,"PasswordAuthentication no" and change its value 'no to yes'. If the line doesn't exist, you can add it.

PasswordAuthentication no
ChallengeResponseAuthentication yes
#### Save the changes and exit the text editor.
To make SSH use the Google Authenticator PAM module, add the following line to the file:
```
sudo vim /etc/pam.d/sshd
```
and add this line 
( auth required pam_google_authenticator.so ) 
Save the changes and exit the text editor.
## step 5: Restart the SSH service to apply the changes:
```
sudo systemctl restart ssh
```
## Step 6: Configure User Accounts
#### Switch to the user account for which you want to enable 2FA
```
su - your_username
```
#### Run the following command to generate a secret key and display a URL with a QR code
```
google-authenticator
```
this command will guide you through the setup process and provide a QR code that you can scan using a 2FA app on your mobile device.
#### Follow the on-screen instructions.

```
Do you want authentication tokens to be time-based (y/n) y
```
#### you see QR code so download the google authenticator application on your mobile
and scan this code you see code of 6 number.
Save the emergency scratch codes and take screen shot of QR code that are provided. These codes can be used to log in if you lose access to your 2FA device.
```
Do you want me to update your "/home/ubuntu/.google_authenticator" file? (y/n) y
```
```
Do you want to disallow multiple uses of the same authentication
token? This restricts you to one login about every 30s, but it increases
your chances to notice or even prevent man-in-the-middle attacks (y/n) y
```
```
By default, a new token is generated every 30 seconds by the mobile app.
In order to compensate for possible time-skew between the client and the server,
we allow an extra token before and after the current time. This allows for a
time skew of up to 30 seconds between authentication server and client. If you
experience problems with poor time synchronization, you can increase the window
from its default size of 3 permitted codes (one previous code, the current
code, the next code) to 17 permitted codes (the 8 previous codes, the current
code, and the 8 next codes). This will permit for a time skew of up to 4 minutes
between client and server.
Do you want to do so? (y/n) y
```

```
If the computer that you are logging into isn't hardened against brute-force
login attempts, you can enable rate-limiting for the authentication module.
By default, this limits attackers to no more than 3 login attempts every 30s.
Do you want to enable rate-limiting? (y/n) y
```
## step 7: give password to your system by this command 
```
sudo passwd <username>
```
### now restart your ssh service again for our security 
```
sudo system restart ssh
```
## Step 8: Test Two-Factor Authentication
### logout your system and again ssh on your local system by this command
```
sudo ssh <username>@<public_ip>
```
##### for example: sudo ssh ubuntu@13.232.120.180
this will show you 
```
Password: 
Verification code:
```
Attempt to SSH into your server. You should now be prompted for your verification code after entering your password.
Open your authenticator app, scan the QR code, and enter the code when prompted during the SSH login process.

so type your password and varification code that you have in your mobile in google authenticator app.
and you will be ssh on your system.


Ensure that you have a reliable method to access your server in case you lose your 2FA device. The emergency scratch codes generated during setup can be used for this purpose.

Regularly backup your SSH configuration before making changes, especially security-related changes.

By following these steps, you've added an extra layer of security to your SSH access using Two-Factor Authentication on Ubuntu. This significantly enhances the security of your server by requiring both something you know (your password) and something you have (your mobile device with the 2FA app)
