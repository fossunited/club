Frappe / Frappe Framework Session Playbook 

Prerequisites: 

- Git  
- Python  
- MariaDB / Postgres ( Basic of DBMS )   
- Frappe bench / Frappe-Manager ( Docker based )   
- HTML / CSS   
- JavaScript / jQuery   
- Jinija Templating   
- Vue ( Recommended ) / Any JS Frameworks   
- ( Optional ) Doppio \- for building static pages

Beginner Session Outline : 

- Working with MariaDB / Frappe Console ( to change passwords or check logs )   
- What is Frappe? (MVC \+ Meta-driven dev)  
- What is Doctypes and its features   
- Bench ( Understanding & Creating )   
- Create Site / Multiple site in one bench  
- Folder structure (apps, sites, config)  
- Bench get / Create app / install app to the site  
- Doctype fields, types, validation, naming series  
- Linked Doctypes, child tables  
- Hide/show fields, dependencies, default values  
- Auto-generated List / Form / Report views  
- Setting up website pages, route and published Doctypes  
- Frappe Builder ( Building frontend or static site from doctype )   
- Repeated block / Client script 

Intermediate Session Outline : 

- Client-side scripting  
- Hooking into Frappe Doctype events (before\_save, on\_submit)  
- Jinja Templates ( Dynamic Data , Python Contexts to pass to templates )  
- frappe.whitelist, RESTful endpoints, tokens, OAuth  
- Fetch data using /api/resource/Doctype  
- User Roles and Permissions ( Shared / Private doctypes )  
- frappe.enqueue, background processing  
- Schedules jobs with [hooks.py](http://hooks.py)  
- Using Vue or JS Frameworks with Frappe  
- Fetching Frappe data with Axios  
- Web Form Vs Desk Forms   
- Email Alert, Notification doctype  
- Templates and triggers

Expert Session Outline : 

- Using frappe.db.sql vs frappe.get\_all ( Complex queries and joins )  
- WebSockets with Frappe Pub/Sub  
- Notifications & Dashboard  
- Custom Page Types and Dashboards  
- Custom APIs & Frappe as a Headless CMS  
- Testing & CI/CD  
- Caching , Indexing , Lazy Loading   
- Deployment  
- Multitenancy

Bonus Session Outline : 

- Doppio   
- Frappe-JS SDK  
- Frappe-React SDK

Reference : 

- Frappe Framework Documentation
