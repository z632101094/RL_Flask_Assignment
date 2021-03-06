<h1>Introduction</h1>
It is an simple backend assignment based on Python + Flask.
It has a User model and a Company model. Some CRUD RestAPIs and authorization are implemented. 



<h1>How to Run</h1>
To execute this program, you should have Python3, Flask, Sqlite3 on your environment. 
You may find some python packages are missing on your environment, just use 'pip install packagename' to
install all needed packages.

Go to the root of this project, then following the instruction below to run the application:

    flask db init
    
    flask db migrate
    
    flask db upgrade
    
    sqlite3 data.db < create.sql
    
    venv\Scripts\python.exe -m flask run

It will create a default User with username = 'admin' and password = 'admin'.
The User object is: 
<p>{'id':1, 'username':'admin', 'password': '8c6976e5b5410415bde908bd4dee15dfb167a9c873fc4bb8a81f6f2ab448a918', 'role_id': 1, 'account_permission': 0}</p>

It will also create a two Role: 

<p>{'id':1, 'name':'admin', 'role_permission': 0x7}</p>
<p>{'id':2, 'name':'user', 'role_permission': 0x1}</p>

The default url of this application is http://127.0.0.1:5000/


<h1>API document</h1>
The following parts are the route and description of all the RestAPI of this application:


    POST: /login
        Input: username<string>, password<string>    
        
        This application uses Session for user authorization. If you login successfully, 
        the users.id will be stored in the session. 
        
        The return value is a json with 2 fields
            if success: code=0, ret_msg<str>
            if fail: code=1, ret_msg<str>

        
        
    POST: /user/create
        Input: username<string>, password<string>, role_id<int>, account_permission<int>
        Admin role needed.
        
        It will basicly create a new User. The username is unique, the role_id must exist in Role table.
        You have 4 legal choice for the account_permission. 
        If the input == 0x0 == 0, nothing happen.
        If the input == 0x2 == 2, this account can access COMPANY_LISTS
        If the input == 0x8 == 8, this account can access COMPANY_INFO (Admin can not access COMPANY_INFO)
        If the input == 0xa == 10, this account can access both COMPANY_LISTS and COMPANY_INFO (Admin can not access COMPANY_INFO)
        
        The return value is a json with 2 fields
            if success: code=0, ret_msg<str>
            if fail: code=1, ret_msg<str>
        
    GET: /user/select
        Input: none
        Admin role needed.
        
        It will basicly return all users. 
        
        The return value is a json with 2 to 3 fields:
            if success: code=0, ret_msg<str>, value = users.json
            if fail: code = 1, ret_msg<str>
        
    GET: /user/select_by_username
        Input: username<str>
        Admin role needed.
        
        It will select a user based on username.
        
        The return value is a json with 2 to 3 fields:
            if success: code=0, ret_msg<str>, value = user.json
            if fail: code = 1, ret_msg<str>
        
    PUT: /user/update: 
        Input: username<str>
        Option Input: password<string>, role_id<int>, account_permission<int>
        Admin role needed.
        
        It will update a user based on username. All the option inputs will be updated. 
        
        The return value is a json with 2 fields:
            if success: code=0, ret_msg<str>
            if fail: code = 1, ret_msg<str>
        
    POST: /user/delete:
        Input: username<str>
        Admin role needed.
        
        It will basically delete a user based on username. 
        
        The return value is a json with 2 fields:
            if success: code=0, ret_msg<str>
            if fail: code = 1, ret_msg<str>
            
    POST: /company/create:
        Input: id<int>
        Admin role needed.
        
        It will create a company with an input id. 
        
        The return value is a json with 2 fields:
            if success: code=0, ret_msg<str>
            if fail: code = 1, ret_msg<str>
    
    POST: /company/delete:
        Input: id<int>
        Admin role needed.
        
        It will dekete a company by id. 
        
        The return value is a json with 2 fields:
            if success: code=0, ret_msg<str>
            if fail: code = 1, ret_msg<str>
            
    GET: /company/read:
        Input: none
        COMPANY_LISTS permission needed. 
        
        It will return all companies. 
        
        The return value is a json with 2-3 fields:
            if success: code=0, ret_msg<str>, value = companies.json
            if fail: code = 1, ret_msg<str>
    
    GET: /company/read_company_info:
        Input: id<int>
        COMPANY_INFO permission needed. User can't have an Admin role. 
        
        It will return the company_info based on id. 
        
        The return value is a json with 2-3 fields:
            if success: code=0, ret_msg<str>, value = company_info.json
            if fail: code = 1, ret_msg<str>
            
    POST: /user_company/create:
        Input: users_id<int>, company_id<int>
        Admin role needed.
        
        It will create a user_company association. users_id and company_id must be existed.
        If you delete a user/company in users/company table, this association will automatically
        be deleted.
        
        The return value is a json with 2 fields:
            if success: code=0, ret_msg<str>
            if fail: code = 1, ret_msg<str>
        
    POST: /user_company/delete:
        Input: users_id<int>, company_id<int>
        Admin role needed.
        
        It will delete a user_company association by input id.
        
        The return value is a json with 2 fields:
            if success: code=0, ret_msg<str>
            if fail: code = 1, ret_msg<str>
    
    GET: /user_company/read:
        Input: none
        Admin role needed.
        
        It will return all user_company associations.
        
        The return value is a json with 2-3 fields:
            if success: code=0, ret_msg<str>, value = user_company.json
            if fail: code = 1, ret_msg<str>
