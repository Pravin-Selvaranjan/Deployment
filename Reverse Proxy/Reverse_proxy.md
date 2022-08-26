# Nginx as a reverse proxy

- `sudo nano /etc/nginx/sites-available/default` to enter config file for nginx
- under `server_name` `location / {` (see below) enter proxy_pass `http://localhost:3000;`


<img width="360" alt="maiks" src="https://user-images.githubusercontent.com/110179866/184926265-e1a3875a-6190-4010-bf78-6bb3a6681321.png">



- This should now allow you to access the app without using port 3000