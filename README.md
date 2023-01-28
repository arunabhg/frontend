# Practice project - Frontend for CI/CD using GitHub Actions

**About** - This deployment method _doesn't_ use AWS CodeDeploy or CodePipeline. It uses only `GitHub Actions` to CI/CD a full-stack project to AWS EC2.

### [Link to the Backend](https://github.com/arunabhg/backend)

### Prerequisites

Knowledge of the JavaScript tech stack like Node, Express, React or any other full-stack ecosystem. Knowledge of Git, GitHub, Putty and basic knowledge of AWS.

### Steps

- Create your frontend project using CRA/Vite.
- Launch a common Ubuntu ec2 instance for your full-stack project in aws.
- Inside Actions, search for Node. Click on `Configure`. This brings up the nodejs yaml file for deployment.
- Make some small changes in the yml file - **_remove_** the _pull request_ part and the _npm test_ part. Change runs-on to `self-hosted`. Remove the node versions which you aren't supporting. Commit the file.
- If on Windows, use Putty or use Terminal to SSH into the instance.

```sh
sudo apt update
```

```sh
sudo apt upgrade
```

- Install `node`, and `npm`.

```sh
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash
```

Copy each line one by one

```sh
export NVM_DIR="$HOME/.nvm"
```

```sh
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
```

```sh
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

- Check that node and npm have been installed

```sh
npm -v
```

```sh
node -v
```

- Install nginx and pm2 globally.

```sh
npm i nginx
```

```sh
npm i -g pm2
```

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

- Check the Runners part inside Settings -> Actions. There should be an IP which should show Active.
- If everything is fine in frontend, go to the \_work directory in the frontend folder and twice inside the same name (frontend, in our case) directory
  ```
  cd /frontend/_work/frontend/frontend
  ```
- There is a **build** folder inside that path. Go inside the folder and copy the full path using `pwd`. Save it in a notepad.
- cd and go to the home folder, then cd to `/etc/nginx`. Go to `sites-available`. Edit default file.

  ```
  sudo nano default
  ```

- After root, there is a path specified. Paste the path of the build folder you have copied in the notepad.
  ```
  /home/ubuntu/frontend/_work/frontend/frontend/build
  ```
- The backend code we are getting on port 3000, so copy the following location inside the default file to route the requests on backend from localhost to the current location (We can name it _api_) -

  ```
  location /api/ {
        proxy_pass  http://localhost:3000/;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
  ```

  Remember to set the indents correctly inside the file.

  Finally, Ctrl + X. Enter. Save the file.

- Restart nginx.
  ```
  sudo systemctl restart nginx
  ```
- Refresh the browser with the server DNS. It should now show the output of the react frontend.
- Change the URL by adding /api at the end. This will show the output of the backend code.
- Now change something in the App.js file and commit it.
- The GitHub Actions CI/CD job will run and the latest changes can be viewed in the browser after a short time.
