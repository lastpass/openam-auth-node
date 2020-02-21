<img src="/images/lastpass_logo.png" width="250" />

# LastPass MFA Authentication Node

The LastPass MFA Authentication Node allows ForgeRock users to integrate their AM instance to the LastPass MFA authentication services.
This document assumes that you already have an AM 5.5+ instance running with an users base configured.

## Installation

Follow this steps in order to install the node:

1. Download the jar file from [here](https://github.com/lastpass/openam-auth-node/blob/master/target/lastpass-openam-auth-node-1.0.zip).
2. Copy the **lastpass-openam-auth-node-1.0.jar** file on your server: `/path/to/tomcat/webapps/openam/WEB-INF/lib`
3. Restart AM.
4. Login into LastPass MFA admin portal and open the `Keys` menu on the left side. Copy the **LastPass MFA Login** value by clicking in the green button and save it for later.

![image alt text](/images/lastpass_keys.png)

5. Login into AM console as an administrator and go to `Realms > Top Level Real > Authentication > Trees`.
6. Click on **Add Tree** button. Name the tree LastPass and click **Create**.

![image](/images/add_tree.png)

7. Add 4 tree nodes: Start, Username Collector, LastPass Service Initiator and Failure.
8. Connect them as shown in the image below.

![image](/images/tree_1.png)

9. Select the **LastPass Service Initiator** node and set the LastPass MFA Key. Paste the key value from step 4 on **LastPass MFA Key**. Set the following URL `https://identity-api.lastpass.com/auth/loginAsync` in **Authentication Endpoint**.
10. Select the **Authentication Method** you'd like to use. If you leave it as **Default** then the **Authentication Method** will be the one selected by users on their phone.
11. Add 3 nodes: Polling Wait Node, LastPass Service Decision and Success and connect them as shown in the image below.

![image](/images/tree_2.png)

12. Select the **LastPass Service Decision node** and set the following URL `https://identity-api.lastpass.com/auth/checkLoginToken` in **Login Token Endpoint**.
13. Select the Polling Wait Node and set **Seconds To Wait** to 4.
14. Add a Retry Decision Limit node and connect it as shown in the image below.

![image](/images/tree_3.png)

15. Select the Retry Decision Limit and set the **Retry Limit** to 15.
16. Save changes.
17. You can test the LastPass MFA authentication tree by accessing this URL in your browser `https://YOUR_AM_SERVER HERE/openam/XUI/?realm=/#login/&service=LastPass`.</br>
18. Enter your username and hit enter. **LastPass MFA tree will search for user email (mail or email attribute) in the data store if email is empty an email address will be generated from user DN**. An authentication request will be send to LastPass through the LastPass MFA tree. LastPass will verify you username and key. If everything is correct you should get an authentication request on your phone.

![image](/images/demo_auth.png)
