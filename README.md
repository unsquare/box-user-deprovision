# Box User Deprovision
A simple script that uses the [Box Python SDK][1] to transfer file ownership and delete users.

# Requirements
You'll need to install Python 3.6+ and the [Box Python SDK][1] with JWT for this to work.

`pip install "boxsdk[jwt]"`

# Basic Setup
When you create an app on the [Box Developer site][2], make sure to choose the option to authenticate with JWT and grant your application **App + Enterprise** access. This is necessary to deprovision users that your app does not own.

You'll also need to turn on the ability to make API calls with the as-user header to make changes as a specific user. This user's name and email address will show up in a comment on the transferred folder.

After you've created the app, Box will give you the option to generate a public/private key pair. This script uses the JSON file generated to create a config file when you run it the first time, but you can also create a config.json file and key pair manually:

```json
{
  "clientID": "YOUR_CLIENT_ID",
  "clientSecret": "YOUR_CLIENT_SECRET",
  "publicKeyID": "YOUR_PUBLIC_KEY",
  "passphrase": "YOUR_API_PASSPHRASE",
  "enterpriseID": "YOUR_ENTERPRISE_ID",
  "rsa_private_key_path": "PATH_TO_PRIVATE_KEY_PEM_FILE",
  "admin_user": "USER_TO_IMPERSONATE",
  "destination_user": "USER_TO_RECEIVE_FILES"
}
```

`admin_user` and `destination_user` should both be email addresses.

This script will not delete users if they own any files. If you want to change that behavior, set force to True:

```python
client.user(user['ID']).delete(notify=False, force=False)
```

[1]: https://github.com/box/box-python-sdk
[2]: https://developer.box.com
