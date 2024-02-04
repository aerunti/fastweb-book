# Module 3 :Using minijinja as template engine

### Guiding Questions:
- How do I use minijinja and create a structure for templating?
- How do I create controllers to use with templates?

### Learning Outcomes:
- Students will configure Cargo.toml to use minijinja
- Students will create the structure to work with templates
- Students will create controllers and routes to work with templates


### Lesson 6:
- Add and configure minijinja in your project
- Code Example
- Summary


Using as base the hello-world project we made before, lets add minijinja and actix_files


    cargo add minijinja -F loader 
    cargo add minijinja-contrib -F datetime,timezone
    cargo add once_cell
    cargo add actix_files
    cargo add serde -F serialize,deserialize


create 2 folders named templates and static 

    mkdir templates static

You will need a base template to use in you project. You can use one free, buy or develop yourself, and put it the static folder. 
in our project, we downloaded the free version of [coreui](https://coreui.io/product/free-bootstrap-admin-template/#pricing-tables)

unzip it and copy the dist folder to the static folder 

cp -r <path>/dist ../static/dashboard

For frontend, we download https://github.com/startbootstrap/startbootstrap-shop-homepage . Unzip it and move to static

mv <path> static/shop

For blog, we download https://startbootstrap.com/theme/clean-blog 

mv <path> static/blog


this are examples. you can use as many templates you wish, if you follow the process. 



add to main.rs 

    use once_cell::sync::Lazy;
    use minijinja::{context,Value};
    use minijinja_contrib;
    use actix_files;


    static TEMPLATE: Lazy<minijinja::Environment<'static>> = Lazy::new(|| {
        let mut env = minijinja::Environment::new();
        minijinja_contrib::add_to_environment(&mut env);
        let tmpl_path = concat!(env!("CARGO_MANIFEST_DIR"), "/templates");
        env.set_loader(minijinja::path_loader(tmpl_path));
        env
    });


    pub fn render_minijinja(template: &str, ctx: Value) -> Result<HttpResponse, actix_web::Error> {

        let tmpl = match TEMPLATE.get_template(template) {
            Ok(tmpl) => tmpl,
            Err(error) => {
            //println!("Minijinja error: {:#?}", error);
                return Ok(HttpResponse::Ok()
                    .content_type("text/html")
                    .body(error.to_string()));
            }
        };

        let rendered = tmpl.render(ctx);

        match rendered {
            Ok(result) => Ok(HttpResponse::Ok().content_type("text/html").body(result)),
            // Err(error) => Ok(HttpResponse::Ok().content_type("text/html").body(error.to_string())),
            Err(error) => {
                let cause = error;
                ////println!("Minijinja error: {:#?}", cause);

                Ok(HttpResponse::Ok()
                    .content_type("text/html")
                    .body(cause.to_string()))
            }
        }
    }


and setup the http_server 

        .service(
                actix_files::Files::new("/static", "static")
                    .use_last_modified(true),

        )


Check the updated content of src/main.rs


    use actix_web::{get, post, web, App, HttpResponse, HttpServer, Responder};
    use once_cell::sync::Lazy;
    use minijinja::{context,Value};
    use minijinja_contrib;
    use actix_files;
    use serde::{Serialize,Deserialize};

    static TEMPLATE: Lazy<minijinja::Environment<'static>> = Lazy::new(|| {
        let mut env = minijinja::Environment::new();
        minijinja_contrib::add_to_environment(&mut env);
        let tmpl_path = concat!(env!("CARGO_MANIFEST_DIR"), "/templates");
        env.set_loader(minijinja::path_loader(tmpl_path));
        env
    });


    pub fn render_minijinja(template: &str, ctx: Value) -> Result<HttpResponse, actix_web::Error> {

        let tmpl = match TEMPLATE.get_template(template) {
            Ok(tmpl) => tmpl,
            Err(error) => {
            //println!("Minijinja error: {:#?}", error);
                return Ok(HttpResponse::Ok()
                    .content_type("text/html")
                    .body(error.to_string()));
            }
        };

        let rendered = tmpl.render(ctx);

        match rendered {
            Ok(result) => Ok(HttpResponse::Ok().content_type("text/html").body(result)),
            // Err(error) => Ok(HttpResponse::Ok().content_type("text/html").body(error.to_string())),
            Err(error) => {
                let cause = error;
                ////println!("Minijinja error: {:#?}", cause);

                Ok(HttpResponse::Ok()
                    .content_type("text/html")
                    .body(cause.to_string()))
            }
        }
    }


    #[get("/")]
    async fn index() -> impl Responder {



        let flash = "";
        let msg_error = "";
        
        #[derive(Serialize,Deserialize)]
        struct Person{
            name:String,
            email:String
        }

        let user = Person{
            name:"John".to_string(),
            email:"john@mail.com".to_string()
        };


        let ctx = context!(flash, msg_error, user);
        render_minijinja("blog/index.html", ctx)
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
                .service(index)
                .service(echo)
                .service(
                    actix_files::Files::new("/static", "static")
                        .use_last_modified(true),

            )
                .route("/hey", web::get().to(manual_hello))
        })
        .bind(("127.0.0.1", 8031))?
        .run()
        .await
    }


The content of Cargo.toml

    [package]
    name = "minijinja-example"
    version = "0.1.0"
    edition = "2021"

    # See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

    [dependencies]
    actix-files = "0.6.5"
    actix-web = "4.5.1"

    once_cell = "1.19.0"
    minijinja = { version = "1.0.12", features = ["loader"] }
    minijinja-contrib = { version = "1.0.12", features = ["datetime", "timezone"] }
    serde = { version = "1.0.196", features = ["derive"] }



At this point, if you run the project, you can check the static templates at 

[http://localhost:8031/static/dashboard/index.html](http://localhost:8031/static/dashboard/index.html)

[http://localhost:8031/static/shop/index.html](http://localhost:8031/static/shop/index.html)

[http://localhost:8031/static/blog/index.html](http://localhost:8031/static/blog/index.html)


Well, these are static files, we need to transform this examples in templates. 


#### from static to template

1) To use a html file as template, we need first copy it to folder template

    mkdir templates/blog
    cp static/blog/index.html templates/blog/index.html

2) we need to create a controller in code to use it


    #[get("/")]
    async fn index() -> impl Responder {


        let flash = "";
        let msg_error = "";
        
        let ctx = context!(flash, msg_error);
        render_minijinja("blog/index.hml", ctx)
    }

Now you will can access [http://localhost:8031/](http://localhost:8031/) and ...
find everythingh unformated

3) Fix the internal links in templates/*/<yourpage>.html

Every href and src attribute should be fixed. Ex:

    href="assets/favicon.ico" should became href="static/blog/assets/favicon.ico"

Links to internal pages, should be adjusted to correct routes like 

    href="index.html" should became href="/"

and 
    href="post.html" should became href="/post"

Obviously, you shoul create the correspondent controllers and templates later

After the correct fixes, you shoul see you route responds like the static one.

4) Spliting the layout from content

Every template has a 'layout' part, common to all page, and the content of the page

To do this, copy the index.html as layout.html

    cd templates/blog
    cp index.html layout.html

Find the part of code that is especific for /index, remove it and replace with


    {% block content %}
    {% endblock %}


in index.html, remove the common part of code, and place at start of file:

    {% extends "frontend/layout.html" %}
    {% block content %}

At end of file, also remove the common part of code, and close the block

    {% endblock content %}

5) Passing values from controller to template

As you can note on mainr.rs code, you can pass values from your code to template adding variables to context.

If you pass structs, they should be Serializable 

        ...
        
        
    #[derive(Serialize,Deserialize)]
    struct Person{
        name:String,
        email:String
    }

    let user = Person{
        name:"John".to_string(),
        email:"john@mail.com".to_string()
    };


    let ctx = context!(flash, msg_error, user);
    render_minijinja("blog/index.html", ctx)
        
        ...


So you can  use the variables in template using 

    {{ user.name}} 

or 

    {{flash}}


Check the minijinja docs to check more info in how to use it. 



### Lesson 7:
- How organize your code to work as MVC (Model-View-Controller) pattern
- Code Example
- Summary