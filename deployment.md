# NodeJS 

# URL : https://flying-paws.node.demo4work.com/
# Port : 3006

# Add known host to flying paws
1) Go to Bitbucket >> flying paws repo
2) Go to Repo settings >> SSH  >> Enable Pipelines (check it)
3) Go to Repo settings >> SSH >> Add known host
   - Add host "159.89.165.78" & fetch
   - Click "Add"
  
# Create webapp
1) Go to web applications
2) Click "Deploy New Web App"
3) Go to Git Repository
4) Select Bitbucket
5) Enter "Web Application Name" >> "flying-paws-3006"
6) Add subdomain >> flying-paws.node.demo4work.com
7) Enable AutoSSL
8) Now, go to git section
   1) ENter Repository >> "incipientinfo-node/flying-paws-backend"
   2) Enter branch >> "master"
   3) Add access key to bitbucket. (Go to bitbucket repo >> Repo settings >> Access keys)
9) Select "web application stack" >> "Native NGINX + Custom config"
10) Click "Deploy" button at last

# Add Nginx Config

- Now go to web settings >> NGINX Config
- click "Add new nginx config"
- Click On Create Config and choose location.root:
- And insert a filename, you can insert whatever you want: I did "flying-paws-web-app-config"
- Insert the following in Config Content, and hit the “Update config” Button:
```
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header Host $http_host;
proxy_pass http://127.0.0.1:3006; 
```
- Save config
  
# Add webhook
- Go to webapp >> git >> copy webhook URL
- Now, go to bitbucket repo >> repo settings >> webhook >>  Add runcloud webhook 
  https://manage.runcloud.io/webhooks/git/lBZLWV0lzwHCviNXuvBqXeda1703573823/zFsJeGBdQu2u8rMHMmkVvQXRl884Pspp

# Add script to run project (NOTE: this will differ based on technology specific commands)
- Go to git >> Deployment script
- And paste below code.
  ```
  export NVM_DIR=$HOME/.nvm;
  source $NVM_DIR/nvm.sh;
  git fetch --all
  git reset --hard origin/master
  git pull
  nvm use 18
  npm -v
  node -v
  npm install --force
  (pm2 delete -s flying-paws-3006 || exit 0) && npm run prod


- Check "Enable" & click "deploy"
  
# Add ENV
- Connect to terminal using ssh `$ ssh runcloud@159.89.165.78`
- Go to directory `$ cd /webapps/flying-paws-3006`
- Run `$ nano .env`
Paste your env

```
# server's port number
PORT=3006

# database uri string   
DB_URI =XXXX


```
- You need to redoploy. Go to runcloud >> git >> deployment script >> click deploy button at top.
- `https://flying-paws.node.demo4work.com/` it should now work.

## HOW to add SSH key
for ubuntu
1) run below steps
```
ssh-keygen -t rsa -b 2048 -C "your.name@e.incipientinfo.com"
ssh-add ~/.ssh/id_rsa
cat ~/.ssh/id_rsa.pub
```
2) then copy your ssh key 
  i.e: ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDXXXXXXXXXXXXXXXXX
3) Go to runcloud  >> SSH >> Add New SSH key
  1) unselect `Use vaulted SSH Key`
  2) enter label 
    i.e. your.name-laptop/system-#N (xyz.pqr-laptop-123 OR asd.rdf-system-999)
  3) check the `Store in SSH Key vault for later use`
  4) click `add ssh key`
