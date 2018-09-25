# **Welcome Apes**
We are happy to create this git repo to collaborate in this project:
**Judy, the smart butler**
## Summary
1. Global architecture
2. Server side
3. Client side
4. Conclusion  

## Global architecture
Our application is devided into two parts: 
Server side : built with nodeJS and expressJS created with express generator with some modifications.
Client side: built with VueJS2 using Vue-cli and webpack   


## Server side  
### Directory tree  
./Judy-server  
├── bin  
├── config  
├── public  
│   ├── images  
│   ├── javascripts  
│   └── stylesheets  
├── src  
│   ├── controllers  
│   ├── middlewares  
│   ├── models  
│   ├── routes  
│   │   ├── guest  
│   │   └── staff  
│   ├── services  
│   └── utils  
└── views  
### Walking through  
Starting from the top -> down  
* ***Bin***   
Contains "www" which is the initial script to start the node server  
* ***Config***  
Contains JSON files holding the application's paramaeters such as :  
Database URI, Roles, etc.  
**** Public***  
Contains all the images, css and js files  
* ***Src***  
This folder is the main folder, and it is the core of our application, it contains those   directories:   
**Controllers:** a controller holds the buisness logic of the application.  
Please the controller name has to be :   

> controller_name.controller.js

controller example:  
   

    let controller = {
	    addPlate(req, res) {},
	    getPlate(req, res) {
    		res.send({ message: 'Many plates!!' });
    	}
    };
    module.exports  = controller;
and this how to use this controller  

    const foodController =  require('../../controllers/restaurant.controller');
    ... <-- (mch tab3in el code brabbi nahhohom ba3d :D)
    router.get('/plate', passport.authenticate('jwt', { session: false }), foodController.getPlate);

**Middlewares:** before getting to the final destination (the matched route), a request may pass by some 'patrols': authentication check, access verification etc.  
authentication and access controle are implemented!  
to add a route to a middlware, just add  
> passport.authenticate('jwt', { session: false })  

like this :   

    router.get('/some/route', passport.authenticate('jwt', { session: false }), controller.someMethodInYourController)
And for access controle middleware, every route contains "staff" or "admin" can only be accessed by Person with admin or staff role.  
**Models:** Contains the schemas of the documents in the database, using mongoose as ODM  
PS: All models are implemented.  
**Routes:** Contains the routes of the application, this folder has two subdirectories: Staff and Guest  
a routes file defined in one of this folder is going to have:  

> /staff or guest/routes_file_name/other_routes_defined_in_that_file

for example: we have **restaurant.js** inside **guest** folder, and we have **/plate** as a route, so the access to this route is via  

> localhost:3000/guest/restaurant/plate

PS: All routes files are autoloaded, so you don't have to add them by yourself!  
**Services:** this folder represents a repository, this is where you can add your own methods to fetch database, controle before insert etc.  
Every service must be injected to the model, later.  
**Utils:** Contains some utlility code, which is not related to the main purpose of the app.  
* ***views***  
We don't need it in this project, in our case we are implementing a restful api consumed by a client side application built by vuejs  

## Client side  

### Directory tree  
./judy-client  
├── build  
├── config  
├── src  
│   ├── assets  
│   ├── components  
│   │   ├── restaurant  
│   │   └── users  
│   ├── router  
│   │   ├── guest  
│   │   └── staff  
│   ├── services  
│   └── store  
└── static  
### Overview  
* Build  
Contains utility code to build the application, and run the application in different modes (dev and prod). it contains also webpack configurations  
* Config  
Contains the configuration files of the application.  
* Src  
This directory is the core of the application, all the logic is implemented within this folder.  
**assets:** Contains images, css and js files  
**components:** Contains the different components. Every module has it own directory, for example restaurant module has a directory restaurant which contains it components.  
Naming rule (pascal case): `NameOfTheComponent.vue`  

  

      <template>
		  <h1 @click='doAction()'>{{ obj }}</h1>    
      </template>
      <script>
        	export default {
	        	name: 'NameOfTheComponent',
	        	data() {
        		return  {
	        		obj: 'somthing'
        		},
        		computed: {
        		},
        		methods: {
	        		doAction() {
		        		console.log('hello world')
	        		},
        		},
        		mounted() {
	        		//some logic to do whene the component is accessed
        		}
        	}
        }
      </script>
      <style scoped>
      </style>

**router:** Contains the routes for client side application; like the server side, this folder has two subdirectories: guest and staff.  
The routes files inside each folder has the followning naming rule  

> filename.(staff or guest).routes.js

and the routes inside the file are going to be accessed as below:  

> /staff or guest/filename/route_inside_the_file

Examlple :  
**restaurant.guest.routes.js**  

    import PlateForm from  '@/components/restaurant/PlateForm'
    export  default  new Array (
    {
	    path: '/plate',
	    name: 'plate',
	    component: PlateForm
    })

*PS: The routes are auto-imported, you have to focus only on creating your routes*  
**services:** Contains all the backend-call implementation using axios api.  
Api.js contains the baseURI and Authentication header, so you don't need to add the token by yourself!   
the file naming rule: `name_of_module.js`  
Example:   
**User.js**  

    import Api from  "@/services/Api"
    
    export  default {
	    register(credentials) {
		    return  Api().post('users/register', credentials)
	    },
	    login(credentials) {
		    return  Api().post('users/login', credentials)
	    }
    }

**store:** Contains store.js which holds token, user object and some other information  
* static  
files in `static/` are not processed by Webpack at all: they are directly copied to their final destination as-is, with the same filename. You must reference these files using absolute paths, which is determined by joining `build.assetsPublicPath` and `build.assetsSubDirectory` in `config.js`  

## Conclusion  
Vue is very beautiful and so is node js and express.  

  

