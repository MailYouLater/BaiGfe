# BaiGfe
**Remove Mandatory Login of Geforce Experience**  
[Support: Visit [The main repository's GitHub issues](https://github.com/Moyster/BaiGfe/issues) to offer or request help.]

**Note:** This fork of [https://github.com/Moyster/BaiGfe](https://github.com/Moyster/BaiGfe) is intended to update the instructions since newer versions of GFE have changed some things, and also to improve the layout to hopefully make it a bit easier to follow.  
### **IMPORTANT:** In its current state, this mod does not restore full functionality of Geforce Experience without logging in. If you're able to help improve this situation, your help would be appreciated.

&#x200B;
# How to Remove Mandatory Login:
### **Note:** Make a backup of every file you edit!

## Easy copy/paste fix:
1. Download pre-modded app.js: [https://github.com/MailYouLater/BaiGfe/raw/master/app.js](https://github.com/MailYouLater/BaiGfe/raw/master/app.js)
2. Copy & paste it into:
   ```
   C:\Program Files\NVIDIA Corporation\NVIDIA GeForce Experience\www\
   ```

&#x200B;

## Auto-install easy fix:

**Right-click `Install-Fix.ps1` and choose Run as Administrator**

&#x200B;

You may need to allow the script to run on your system. To do this:

1. Run powershell as administrator, then run this powershell call:
   ```
   Set-ExecutionPolicy RemoteSigned
   ```
2. Type "A" and press enter
3. Run `Install-Fix.ps1` again

&#x200B;

## Manual way:
**NOTE:** Some of the code below (i.e. the letters used as function names) might change between versions, just compare with one of the previously uploaded app.js ;)

1. Open `C:\Program Files\NVIDIA Corporation\NVIDIA GeForce Experience\www\app.js`

 - (Optional) Use [http://jsbeautifier.org/](http://jsbeautifier.org/) to make it easier to read and edit.

2. Nullify login requirement (in app.js):
   - find this:
     ```
                         if (e.domains.list.indexOf(n) > -1) return !0
     ```
   - and replace it with this:
     ```
                         if (e.domains.list.indexOf(n) > -1) return b.handleLoggedIn(e), !0
     ```

3. Add fake user info for the not-logged-in user:
   - find this:
     ```
                 }, b.isLeftPaneVisible = function() {
                     return !("choose" === y.nvActiveAuthView)
                 }
     ```
   - and replace it with this:
     ```
                 }, b.isLeftPaneVisible = function() {
                     return !("choose" === b.nvActiveAuthView)
                 }, b.handleLoggedIn({
                     sessionToken: "dummySessionToken",
                     userToken: "dummyUserToken",
                     user: {
                         core: {
                             displayName: "Anonymous",
                             primaryEmailVerified: true
                         }
                     }
                 });
     ```

4. Force-enable ShadowPlay and Share buttons:  
   <sub>(To make the ShadowPlay & Share buttons show on the main GFE screen.)</sub>
   - find this:
     ```
     Q.isShareSupported = !1, Q.isShareButtonClicked = !1
     ```
   - and replace with this:
     ```
     Q.isShareSupported = !0, Q.isShareButtonClicked = !0
     ```

- (Optional) Remove forced email verification:  
  <sub>(should help in some cases where a user has previously been logged in)</sub>
    - find:
      ```
      y.info("automatically resent verification email"), u.endActionAsync(r, "EMAIL_NOT_VERIFIED"), b.showEmailVerification(e)  
      ```
    - and remove:
      ```
      , b.showEmailVerification(e)
      ```

## How to Block Data Collection / Telemetry (block all or keep Games Optimisations)

1. Open the hosts file in a text editor (notepad++):
   ```
   C:\Windows\System32\drivers\etc\hosts
   ```
   (copy-paste the file on your desktop to edit, copy back to "etc" folder if you have permission errors)

2. Copy and paste one of these lists to the end of the file (CHOOSE ONE OR THE OTHER LIST, NOT BOTH):
   > Full blocklist: blocks telemetry / driver & Gfe updates (Keeps Game Optimisations and all "offline" features working)  
   > Lite blocklist: only blocks telemetry (might break Game Optimisations in some cases, fix/workaround pending)

- FULL BLOCKLIST: (as of 04/17/2020 - GFE 3.20.3.63)  
  ```
  0.0.0.0 telemetry.gfe.nvidia.com
  0.0.0.0 gfe.nvidia.com
  0.0.0.0 gfwsl.geforce.com
  0.0.0.0 services.gfe.nvidia.com
  0.0.0.0 accounts.nvgs.nvidia.com
  0.0.0.0 accounts.nvgs.nvidia.cn
  0.0.0.0 events.gfe.nvidia.com
  0.0.0.0 img.nvidiagrid.net
  0.0.0.0 images.nvidiagrid.net
  0.0.0.0 images.nvidia.com
  0.0.0.0 ls.dtrace.nvidia.com
  0.0.0.0 ota.nvidia.com
  0.0.0.0 ota-downloads.nvidia.com
  0.0.0.0 rds-assets.nvidia.com
  0.0.0.0 assets.nvidiagrid.net
  0.0.0.0 nvidia.tt.omtrdc.net
  0.0.0.0 api.commune.ly
  0.0.0.0 login.nvgs.nvidia.com
  0.0.0.0 login.nvgs.nvidia.cn
  ```

- LITE BLOCKLIST (keep GFE / Drivers Update working): (as of 04/17/2020 - GFE 3.20.3.63)  

  might break Game List for now, use the full blocklist if it does
  ```
  0.0.0.0 ls.dtrace.nvidia.com
  0.0.0.0 telemetry.gfe.nvidia.com
  0.0.0.0 accounts.nvgs.nvidia.com
  0.0.0.0 accounts.nvgs.nvidia.cn
  0.0.0.0 nvidia.tt.omtrdc.net
  0.0.0.0 api.commune.ly
  0.0.0.0 login.nvgs.nvidia.com
  0.0.0.0 login.nvgs.nvidia.cn
  ```

3. Save and forget :)

&#x200B;

**Note:** "[0.0.0.0](https://0.0.0.0)" is preferred to "[127.0.0.1](https://127.0.0.1)" but either works  
&#x200B;  
**Note:** If you previously blocked all nvidia domains, you will need to flush your DNS cache to restore the "Game Optimizations" functionality:  
open "CMD" > `ipconfig /flushdns` )

&#x200B;

&#x200B;

# PS: The "beautified" version of app.js will work just fine, you can minify it back, but it's not mandatory (just like login :) ).

*Post and fix based off the previous mod author(s), mainly:* [*https://www.reddit.com/r/nvidia/comments/8b5nej/updated\_remove\_mandatory\_login\_of\_geforce/*](https://www.reddit.com/r/nvidia/comments/8b5nej/updated_remove_mandatory_login_of_geforce/)  
This fork of [https://github.com/Moyster/BaiGfe](https://github.com/Moyster/BaiGfe) is intended to update the instructions since newer versions of GFE have changed some things, and also to improve the layout to hopefully make it a bit easier to follow.
