# RTSP TO HLS


```
ffmpeg -fflags nobuffer \
 -loglevel debug \
 -rtsp_transport tcp \
 -i rtsp://admin:password@cameraip:port/cameraname \
 -vsync 0 \
 -copyts \
 -vcodec copy \
 -movflags frag_keyframe+empty_moov \
 -an \
 -hls_flags delete_segments+append_list \
 -f hls \
 -hls_time 1 \
 -hls_list_size 3 \
 -hls_segment_type mpegts \
 -hls_segment_filename '/tmp/hls/%d.ts' \
/tmp/hls/cm1.m3u8
```

html example 

```
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>HLS Stream</title>

<link href="https://vjs.zencdn.net/6.6.3/video-js.css" rel="stylesheet">
<script src="https://vjs.zencdn.net/6.6.3/video.js"></script>
<script src="https://cdn.streamroot.io/videojs-hlsjs-plugin/1/stable/videojs-hlsjs-plugin.js"></script>

  </head>
  <body>
        <video id="camera" width=600 height=300 class="video-js vjs-default-skin" controls>
            <source
                src="/live/cm1.m3u8"
                type="application/x-mpegURL">
        </video>
        <script>
            var options = {
                html5: {
                    hlsjsConfig: {
                        // Put your hls.js config here
                    }
                }
            };

            // setup beforeinitialize hook
            videojs.Html5Hlsjs.addHook('beforeinitialize', (videojsPlayer, hlsjsInstance) => {
                // here you can interact with hls.js instance and/or video.js playback is initialized
            });

            var player = videojs('camera', options);
        </script>
  </body>
</html>
```

configure your nginx server in /etc/nginx/sites-enabled/youdomain.conf

```

server {
    listen 80;
    listen [::]:80;
    server_name yourdomain.com;
    return 302 https://$server_name$request_uri;
}

server {

    listen 443 ssl http2;
    listen [::]:443 ssl http2;
    ssl_certificate         /etc/ssl/cert.pem;
    ssl_certificate_key     /etc/ssl/key.pem;


	server_name yourdomain.com;




	index index index.html;


    location /live { 
            alias /tmp/hls/; #create this dir
        } 

 
    types {
        application/vnd.apple.mpegurl m3u8;
        video/mp2t ts;
        text/html html;
    } 



	location / {
		# adjust to your app	
        proxy_set_header Host $host;
		proxy_set_header X-Real-IP $remote_addr;		
	    proxy_pass http://127.0.0.1:8000;
		#try_files $uri $uri/ =404;
		proxy_read_timeout 600;
   		proxy_connect_timeout 600;
   		proxy_send_timeout 600; 

	}

}

```



