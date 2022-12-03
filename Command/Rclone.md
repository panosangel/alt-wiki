# Rclone

## Install

Run the following script:
```shell
curl https://rclone.org/install.sh | sudo bash
```
or download it from:
`https://rclone.org/downloads/`

## Help list

```shell
rclone help
```

## (optional) Create a Google Drive API Key

Even though it's not necessary, using your own API key could be beneficial in avoiding speed limitation enforced by RClone's API key.

1. Visit [Google Cloud Platform - APIs & Services](https://console.cloud.google.com/apis?supportedpurview=project)
2. Select a project. If you don't already have one created, create one. 
3. Click "Enable APIs and Services" and then select Google Drive.
4. You will then need to click on the "Credentials" tab on the left and then "Create Credentials" at the top of the screen. 
5. Once here, you'll want to create an OAuth Client ID.
6. If you haven't already, the site will prompt you to create a consent screen.
7. You will be able to choose between "Internal" and "External". For our purposes, you'll want to select "External".
8. Once on the consent screen configuration page, give your application a name and domain to link it to. Much of the other fields here are optional but I encourage you to fill out as much as we can here. 
9. Once done, simply hit "Save".
10. Next, you'll want to select an Application Type. Here, you'll want to select the type of device that most directly pertains to your use case. 
11. Finally, you'll want to grab your Client ID and Secret and keep them handy for later on in our Rclone configuration.

## Getting Started

```shell
rclone config
```
and follow any guide from the `Sources` to add a remote.

## Mount Google Drive locally

Create a local folder.
```shell
mkdir /path/to/folder
chmod 755 /path/to/folder
```
Then run the following command:
```shell
rclone mount <remote-name>: <local-folder> --allow-other --fast-list --vfs-cache-mode full
```
## Unmount

If it doesn't happen automatically when the `CTRL+C` is sent to the terminal then you can run:
```shell
fusermount -u <path/to/mount>
```

## Automation

TODO

## Access Rclone Web GUI

The recent Rclone version ships with a simple Web based UI for Rclone. You can access Rclone Web GUI by running the following command from the Terminal:
```shell
rclone rcd --rc-web-gui
```
It will open the Rclone dashboard at `http://localhost:5572/` URL in your default browser.

## Appendix A - Sources
- 
- [Rclone](https://rclone.org/)
- [How To Install Rclone In Linux And Unix](https://ostechnix.com/install-rclone-in-linux/)
- [How To Mount Google Drive Locally Using Rclone In Linux](https://ostechnix.com/mount-google-drive-using-rclone-in-linux/)
- [Setting up Rclone with Google Drive](https://tcude.net/setting-up-rclone-with-google-drive/)
- [How to Use rclone to Back Up to Google Drive on Linux](https://www.howtogeek.com/451262/how-to-use-rclone-to-back-up-to-google-drive-on-linux/)
- [How to mount an encrypted Google Drive folder with rclone](https://blog.thelazyfox.xyz/how-to-mount-an-encrypted-google-drive-folder-with-rclone/)
- [How to mount your GDrive on Ubuntu using rclone](https://blog.galt.me/how-to-mount-gdrive-in-nextcloud/)
- [Archive data via rclone limiting the bandwidth](https://blog.thelazyfox.xyz/script-factory/archive-data-via-rsync-limiting-the-bandwidth/)
