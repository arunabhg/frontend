# Practice project - Frontend for CI/CD using GitHub Actions

### [Link to the Backend](https://github.com/arunabhg/backend)

### Steps

- Create your frontend project using CRA/Vite.
- Launch a common Ubuntu ec2 instance for your full-stack project in aws.
- Inside Actions, search for Node. Click on `Configure`. This brings up the nodejs yaml file for deployment.
- Make some small changes in the yml file - **_remove_** the _pull request_ part and the _npm test_ part. Change runs-on to `self-hosted`. Remove the node versions which you aren't supporting. Commit the file.
- If on Windows, use Putty or use Terminal to SSH into the instance.
- Install node, npm, nginx, and pm2 globally.
- **Note:** Ensure that sudo can run node, npm and pm2 commands.

```sh
sudo ln -s "$(which node)" /sbin/node
```

```sh
sudo ln -s "$(which npm)" /sbin/npm
```

```sh
sudo ln -s "$(which pm2)" /sbin/pm2
```

- Copy the instance DNS and paste in a browser. You will be able to see the welcome message of nginx.
- Go to the runners part in Settings -> Actions in your GitHub project. Click on `New and Self-hosted runners`.
- Based on the OS (linux) you have chosen, select the runner.
- Copy and paste first line in terminal/putty, _changing_ the directory name with your frontend directory name.
- Next, keep copying and pasting each line as it is.
- Lastly, press Enter three times and we're done configuring the runner.
- Inside Putty/terminal, cd to your frontend directory and give the following command,

```sh
sudo ./svc.sh install
```

- Next,

```sh
sudo ./svc.sh start
```

- Check the runners part inside Settings. There should be an IP which should show Active.
-

