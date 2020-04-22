# Technology Overview
- PostgreSQL
- Django
- Zappa


## To test the app locally, you need to do the followings:

1. Go to virtual environment, "source app-env/bin/activate"
2. Install packages, "pip install -r requirements.txt" 
3. Install redis-server for websocket, "sudo apt install redis-server"
4. Run the redis server in a terminal, "redis-server"
5. Run the Django in other terminal, "python manage.py runserver"

** Make sure to copy your AWS credentials into ~/.aws/credentials
-------------------------------------------------------------------------------
Open the Chrome console to inspect errors.

**You need to run redis-server first,** if encountered by this error

*  This error persists when deployed with Zappa

```
(index):24 WebSocket connection to '<domain>' failed: Error during WebSocket handshake: Unexpected response code: 200
(anonymous) @ (index):24
(index):45 Chat socket error occured
(index):41 Chat socket closed
```



-------------------------------------------------------------------------------
# Quick summary of changes
* app>setting.py
    * CHANNEL_LAYERS =[... "hosts": [("localhost", 6379)] ]   
    * WSGI_APPLICATION = 'app.wsgi.application", ASGI_APPLICATION = 'app.routing.application'
    * INSTALLED_APPS = [...'channels'...]
* chat>views.py
    * ASGI_APP.... changed chat.routing to app.routing
* chat>consumer.py
* chat>template>chat>room.html
    * changed -> var chatSocket = new WebSocket 'ws://' + window.location.host + '/ws/chat/' + roomName + '/')
* chat>templates>chat>index.html
* chat>routing.py 
* app>asgi.py
* chat>urls.py
* zappa_settings.json -> timeout set to 300 seconds (becayse, when deployed, registering a user takes a long timm and causes timeout error)
