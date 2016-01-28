---
layout: post
status: publish
title: 'Spot the bug #2'
slug: "Spot-the-bug-2"
---
I came across this issue just at quitting time yesterday and was blown away when I realized what was happening.

**The UsersController Index View (pre submit)**
![image][1]

**The UsersController Code**

    public class UsersController : Controller
    {

        List&lt;string&gt; users = new List&lt;string&gt; ()
            {
                "mace.windu",
                "yoda",
                "senator.amidala",
                "anakin.skywalker",
                "obiwan.kenobi"
            };

        public ActionResult Index()
        {
            return View( users );
        }


        [HttpPost]
        public ActionResult Delete ( string[] userstodelete )
        {

            if ( userstodelete == null || userstodelete.Length == 0 )
            {
                throw new ArgumentException ( 
                    "argument must contain at least one entry", 
                    "userstodelete" );
            }

            // code could go here to
            // call out to some service to
            // delete these users

            TempData["deletedUsers"] = userstodelete;

            foreach ( var user in userstodelete )
                users.Remove ( user );

            return View ("Index", users);
        }
    }


**Problem**
Looks like it should all work perfectly right? That's what I thought. However, clicking "Delete Users" will only "delete" our pre-darth user "anakin.skywalker".

Why?

*Hint: Everything here is [working exactly as it should.][2] *

  [1]: http://codeimpossible.com/wp-content/uploads/2010/08/listbox_post_problem.png
  [2]: http://www.w3.org/TR/html401/interact/forms.html#edef-SELECT
