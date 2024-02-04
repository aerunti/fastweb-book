
# Module 2: Actix-web basics and nginx configuration

### Guiding Questions:
- How start a project using actix-web and configure it for development/production?
- How do I configure using .env file?
- How do I create a subdomain in nginx for my project and secure it with certbot?
Learning Outcomes
- Students will perform create a new project using actix-web
- Students will edit files in Visual Studio Code with Remote-ssh plugin
- Students will write Markdown and preview it

### Actix-web is the base framework we choose to work.
We start with a very simple project and configure it using a .env file and providing a subdomain
to work with.
At end, we finish it with a git commit to learn to work with code versioning
Students will:
1. Create a new project
2. Add crates to Cargo.toml
3. Use crates in main.rs
4. Configure .env
5. Configure subdomain in nginx
6. Generate a ssl certificate (optional)

### Lesson 2:
- Configure a basic environment to work: VsCode, Git, WSL or VPS, install rust
- Example
- Resume

Before we start to develop, we need some toolkit to work. 

### Linux or WSL
I recommend you use linux for development, so if you use Windows, you will need to setup and use WSL (you can use Vmware or VirtualBox instead)

I will not reinvent the wheel, use the official documentation [WSL](https://learn.microsoft.com/en-us/windows/wsl/install)

If you already is using a linux distro, congratulations. I recomend use Ubuntu. 


### Git 

As programmer, you need to use a version control tool. Git is really the best one. No args, please. 

Search in google and learn how install and use in your platform. 



### Vscode with remote-ssh or wsl

I recommend Vscode with remote-ssh. Its allow you code in a remote server or wsl with easy. 

I will not work lazy students. Check the [Vscode Website](https://code.visualstudio.com/) and learn how to install and configure in your machine.

Add 'Remote-ssh" extension. 

If you work in a remote server, you will need to use a ssh key. Generate one with (terminal or git bash):

    ssh-keygen

Next step is add your remote server in .ssh/config

    Host <yourserveralias>
    Hostname <yourserverip>
    User <youruser>

Values inside tags <> mean you need to replace with your data. 

After that, you will need to copy your key to server

    ssh-copy-id <yourserveralias>

it will ask to confirm fingeprint and then ask for the server password. 

After that, you will connect to your server without asking for password every time. 

If you are a person that need a video for everything, check [this](https://www.youtube.com/watch?v=SF9IgA9Z8Tg)


### Install Rust

Follow instructions in [https://rustup.rs/](https://rustup.rs/) to install in your linux enviroment 


PS: You won't become a good programmer if you don't learn to look for answers









### Lesson 3:
- Create and configure a new project with actix-web
- Code Example
- Summary

First of all: Always look the official documentation of any tool/framework/crate you will use. 

So check [Actix Web Website](https://actix.rs/docs/getting-started)

But if you are a lazy student, I will provide a resume

    cargo new hello-world
    cd hello-world
    cargo add actix-web

Replace the content of src/main.rs with the code below and save:

    use actix_web::{get, post, web, App, HttpResponse, HttpServer, Responder};

    #[get("/")]
    async fn hello() -> impl Responder {
        HttpResponse::Ok().body("Hello world!")
    }

    #[post("/echo")]
    async fn echo(req_body: String) -> impl Responder {
        HttpResponse::Ok().body(req_body)
    }

    async fn manual_hello() -> impl Responder {
        HttpResponse::Ok().body("Hey there!")
    }

    #[actix_web::main]
    async fn main() -> std::io::Result<()> {
        HttpServer::new(|| {
            App::new()
                .service(hello)
                .service(echo)
                .route("/hey", web::get().to(manual_hello))
        })
        .bind(("127.0.0.1", 8031))?
        .run()
        .await
    }

I always suggest use a unusual port. As developer you probably get some of common ports (5000, 8000, 8080) in use. 

In the folder hello-world, run the project with

    cargo run

or better, to auto-reload when you change the code:

    cargo watch -x run 

If needed install cargo watch with

    cargo install cargo-watch




### Lesson 4:
- Install nginx and configure a subdomain optional
- Code Example
- Summary

If you are working a remote server or vps, you will certainly need configure it to view the result of what you are doing, and later, show it for your client or testers. 

Nginx is the recommended webserver: its very simple to configure as a proxy for your project


1) Install it (ubuntu/debian):

    sudo apt install nginx 

2) create a configuration file in /etc/nginx/sites-available and later link it in /etc/nginx/sites-enabled/, or create it directly in the later. 

content of /etc/nginx/sites-enabled/<yoursubdomain>.conf

    server {
        listen 80;
        listen [::]:80;

        server_name <yoursubdomain.com>;

        location / {
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_pass http://localhost:8031;
        }
    }


you need change the "8031" to the port you project use. 

test the config with 

    nginx -t

and if its ok, restart nginx 

    service nginx restart


PS: you subdomain should already be configured to point to your server. its not covered here. 





### Lesson 5:
- Generate a free ssl certificate with certbot
- List of commands
- Summary


1) install certbot:

    sudo apt install certbot python3-certbot-nginx

2) execute it

    certbot

At first time, it will require to answer few questions and ask for your email. 

After, It will show a list of subdomains configured in nginx to you choose which ones you wanna generate a certificate. 
Choose that one you configured.

At end, it will change the config file you created and restart nginx. 

All you need to do is access your subdomain in your browser to check is now redirecting to the https


