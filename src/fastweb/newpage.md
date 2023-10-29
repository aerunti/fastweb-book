# Tutorial - How create a new page (query/model/controller/template)



## Understand fastweb structure

Fastweb use mvc principles, so their code is organized in folders

### queries

Folder queries contains files for cornucopia use. We creata a file for each table and organize related queries inside. 

after inclusive/modify a query in a file, you should run

    bash generate_cornucopia.sh

some of first projects this file is still named 'gerar.sh'

When you setup a new project, you should update the postgresql connection inside this file to correct one



### src/models

Models should be preferably be coded inside files under src/models 

### src/controllers

controllers are organized inside this folders, and can have nested folders too, if the project is big enough

### templates

the main template is under templates/top, and templates can have nested folders too. 


## How to create a new page

If your new page is related to a current funcionality area, you can put it inside a existent file under src/controllers/* folders. 

If you want to create a new funcionality area, you should create a new rust module (folder or file), inside controllers. 

A controller mod configuration has 3 steps:

1) Create the file and reference it in controllers mod. 
2) a head like this, where you declare the mod public, make imports and setup configuration 

    pub mod registers;

    use crate::models::operator::AuthOperator;
    use crate::models::{redirect_dashboard, render_minijinja, AppState};
    use crate::prelude::*;
    use actix_web::http::header;
    use actix_web::{get, HttpRequest};
    use serde::Serialize;

    pub fn configure(config: &mut ServiceConfig) {
        config.service(camera_last_photo)
        .service(admin_dashboard)
        .service(timelapses)
        .service(contact);
    }

3) the implementation of controllers. Each controllers have his own requirements. You will need learn apart, or later

this is a very simple controller implementation. you can use it to start you implementation, or copy someone other that suits better your requirements. 



    #[get("/admin")]
    pub async fn admin_dashboard(session: Session,
        data: web::Data<AppState>,
        req: HttpRequest) -> impl Responder {
        // let operator = AuthOperator::get_operator(&session, &data, &req).await;
        redirect_dashboard(&session, &data, &req).await
    }


## set up configuration in main.rs or bin/fastwebr.rs

for the case above, in the main.rs or bin/fastweb.rs have a line like it


    .configure(controllers::admin::configure)


if you add a new mod, you should include a line to use the configuration for that module. 

