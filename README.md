# Counter-Strike 1.6 cs_mansion Docker Server with bots
Docker Image for a dedicated Counter-Strike 1.6 server with metamod, amxmodx and fast download.



## Usage

The fastest way to set this up is to pull the image and start it via `docker run`.

``` bash
docker pull archont94/counter-strike1.6
```

``` bash
docker run --name cs16-server -p 27015:27015/udp -p 27015:27015 -v ./cstrike:/hlds/cstrike --entrypoint /bin/sh archont94/counter-strike1.6:latest -c "service nginx start && ./hlds_run -game cstrike -strictportbind -ip 0.0.0.0 -port 27015 +sv_lan 0 +map cs_mansion -maxplayers 10 +localinfo mm_pluginsfile addons/metamod/plugins.ini +pb fillserver"
```

Port 27015 is required by Counter-Strike 1.6 game server.

Port 80 is used to serve assets (maps, gfxs etc.) via http for fast download feature. 

## Custom config files

You can add you own `server.cfg`, `banned.cfg`, `listip.cfg` and `mapcycle.txt` (or any other file) by linking them as volumes into the image.

``` bash
-v /path/to/your/server.cfg:/hlds/cstrike/server.cfg
```

The complete command looks like this:

``` bash
docker run --name cs16-server -p 27015:27015/udp -p 27015:27015 -v /path/to/your/server.cfg:/hlds/cstrike/server.cfg archont94/counter-strike1.6:latest
```

Keep in mind the server.cfg file can override the settings from your environment variables:  
`MAP`, `MAXPLAYERS` and `SV_LAN`

## Additional mods

In order to install additional amxmodx mods, follow the instrucitons. Usually it is required to copy files to proper folder (`/hlds/cstrike/addons/amxmodx/plugins`) and modify some config files.

In order to copy files to docker you can use `docker cp` command:

``` bash
docker cp EXTRACTED_MOD_DIRECTORY CONTAINER_ID:/hlds/cstrike/addons/amxmodx
```

Be careful to not overwrite other files. Before any changes, you can always use `docker commit` command to create image with your changes, which you can restore easily later.
