﻿<a name="title" />
# Building and Deploying an ASP.NET Application #

---
<a name="Overview" />
## Overview ##
 
In this demo you will create a new ASP.NET MVC application using the new One ASP.NET template, explain how routing works in MVC and show the default HomeController and Views. Then you will walk through the process of creating GeekQuiz object model (TriviaQuestion and TriviaOption) and leverage MVC scaffolding to create the controllers and views. Finally, you'll deploy the site to a new Windows Azure web site created from within Visual Studio using the new tooling.

<a id="goals" />
### Goals ###
In this demo, you will see how to:

1. Create a new MVC application using the new One ASP.NET tooling
1. Create GeekQuiz object model
1. Use MVC Scaffolding to create controllers and views for your model
1. Create a new web site in Windows Azure and deploy

<a name="technologies" />
### Key Technologies ###

- [Microsoft Visual Studio 2013][1]
- [ASP.NET MVC][2]
- [Windows Azure][3]

[1]: http://www.microsoft.com/visualstudio/eng/visual-studio-2013
[2]: http://www.asp.net/mvc
[3]: http://www.windowsazure.com

<a name="Setup" />
### Setup and Configuration ###
In order to execute this demo you need to set up your environment.

1. Follow the steps detailed in [this link](http://docs.nuget.org/docs/creating-packages/hosting-your-own-nuget-feeds) to setup local sources for the following directories:

	1. **C:\Program Files (x86)\Microsoft Web Tools\Packages**
	1. **C:\Program Files (x86)\Microsoft ASP.NET\ASP.NET Web Stack 5\Packages**

	![Nuget Sources](images/nuget-source.png?raw=true)

1. Visual Studio 2013 running

1. Windows Azure Tools for Visual Studio

1. Windows Azure Subscription

1. Windows Azure subscription imported in Visual Studio

<a name="Demo" />
## Demo ##
This demo is composed of the following segments:

1. [Creating the project](#segment1).
1. [Creating the Controllers using Scaffolding](#segment2)
1. [Running the site locally](#segment3)
1. [Deploying to Windows Azure Web Sites](#segment4).

<a name="Segment1" />
### Creating the project ###

1. Open the **File / New Project** dialog and show the options in the **Visual C# / Web** section.
	
	![Simplified File/New experience with a single ASP.NET Web application template](images/file-new-experience-single-aspnet-web-application-template.png?raw=true "Simplified File/New experience with a single ASP.NET Web application template")

	_Simplified File/New experience with a single ASP.NET Web application template_

	> 	**Speaking Point:** 
	>
	Explain that instead of having to select from many Web templates, now is only "ASP.NET Web Application". Compare this to the set of templates available in Visual Studio 2012.

	> ![Old Visual Studio 2012 set of templates](images/old-visual-studio-2012-set-of-templates.png?raw=true "Old Visual Studio 2012 set of templates")

1. Name the application _GeekQuiz_ and click **OK**.

1. Select new MVC project and check the Web API option.

	![New ASP.NET template options for other project types](images/new-aspnet-template-options-for-other-project-types.png?raw=true "New ASP.NET template options for other project types")

	_New ASP.NET template options for other project types_

	> 	**Speaking Point:** 
	>
	Explain the options in the new ASP.NET Project dialog. There are still base templates, but you can include folders and core references for other project types.

1. Click **OK** to create the project.

	> 	**Speaking Point:** 
	>
	As the project is loading, talk about changes to the project template, including Bootstrap, updates to user authentication, and decoupling the tooling from project types (so Web Forms applications can easily use ASP.NET MVC controllers, etc.)

1. Explore the generated solution in the **Solution Explorer**.

	![Exploring the solution](images/exploring-the-solution.png?raw=true "Exploring the solution")

	_Exploring the solution_

1. Open the **Controllers/HomeController.cs** file and show the generated code.

	![Exploring the controllers](images/exploring-the-controllers.png?raw=true "Exploring the controllers")

	_Exploring the controllers_

1. Expand to the **Views/Home** folder and show the generated files.

	![Exploring the views](images/exploring-the-views.png?raw=true "Exploring the views")

	_Exploring the views_

1. Open the **Index.cshtml** file located in the **Views/Home** folder.

1. Open the **App_Start/RouteConfig.cs** file and explain routing.

	> 	**Speaking Point:** 
	>
	Note that when accessing to the url ending with /home/index you will be calling the home controller and the index action of that controller. Additionally, the index action is mapped to the cshtml file with the same name inside the home folder. 

1. Create the GeekQuiz model classes. To do that, right-click the **Model** folder and expand the **Add** menu and select **Class**.

	![Creating a new model class](images/creating-a-new-model-class.png?raw=true "Creating a new model class")

	_Creating a new model class_

1. Name the file _TriviaQuestion.cs_ and click **Add**.

	![Creating the TriaviaQuestion class](images/creating-the-trivia-question-class.png?raw=true "Creating the TriaviaQuestion class")

	_Creating the TriaviaQuestion class_

1. Add the following code to the **TriviaQuestion** class.

	<!-- mark:3-8 -->
	````C#
    public class TriviaQuestion
    {
        public int Id { get; set; }

        [Required]
        public string Title { get; set; }

        public virtual List<TriviaOption> Options { get; set; }
    }
````

1. Add the following using statement to the **TriviaQuestion.cs** file.

	````C#
	using System.ComponentModel.DataAnnotations;
````

1. Repeat steps 10 and 11 to add a new class named _TriviaOption_.

1. Add the following code to the **TriviaOption** class.

	<!-- mark:3-16 -->
	````C#
    public class TriviaOption
    {
        [Key, DatabaseGenerated(DatabaseGeneratedOption.Identity)]
        public int Id { get; set; }

        [ForeignKey("TriviaQuestion")]
        public int QuestionId { get; set; }

        public virtual TriviaQuestion TriviaQuestion { get; set; }

        [Required]
        public string Title { get; set; }

        public bool IsCorrect { get; set; }
    }
````

1. Include all the missing using statements.

1. Build the solution.

<a name="Segment2" />
### Creating the Controllers using Scaffolding ###

1. Create the controller to manage the model's CRUD operations. To do that, right-click the **Controllers** folder and expand the **Add** menu and select **Controller...&**

	![Creating a new Controller](images/creating-a-new-controller.png?raw=true "Creating a new Controller")

	_Creating a new Controller_

	> 	**Speaking Point:** 
	>
	Let's create the controllers and views using scaffolding for each model class. 

1. Select the **MVC 5 Controller with views, using Entity Framework** option in the **Add Scaffold** dialog and click **Add**.

	![Selecting the MVC 5 Controller with views and Entity Framework](images/selecting-mvc5-controller.png?raw=true "Selecting the MVC 5 Controller with views and Entity Framework")

	_Selecting the MVC 5 Controller with views and Entity Framework_

1. Change the name of the controller to **QuestionController**.

	![Changing the controller name](images/changing-the-controller-name.png?raw=true "Changing the controller name")

	_Changing the controller name_

1. Select **TriviaQuestion** from the **Model class** list.

	![Selecting the TriviaQuestion model](images/selecting-the-triviaquestion-model.png?raw=true "Selecting the TriviaQuestion model")

	_Selecting the TriviaQuestion model_

1. Create a new data context named **TriviaContext**.

	![Creating the new TriviaContext](images/creating-the-new-triviacontext.png?raw=true "Creating the new TriviaContext")

	_Creating the new TriviaContext_

1. Check the **Use async controller actions** checkbox.

	![Checking the use async controller actions checkbox](images/checking-the-async-option.png?raw=true "Checking the use async controller actions checkbox")

	_Checking the use async controller actions checkbox_

1. Finally, click **Add** to create the views and the controller with the default actions.

	![Creating the QuestionController](images/creating-the-questioncontroller.png?raw=true "Creating the QuestionController")

	_Creating the QuestionController_

1. Expand to the **Controller** folder and show the **QuestionController.cs** file, which was just created.

1. Open the **QuestionController.cs** file to show the generated content.

1. Expand the **Views** folder and show the new views under the **Question** folder.

1. Repeat steps 1 through 7 to create the **OptionController** for the **TriviaOption** class using the already existing **TriviaContext**.

<a name="Segment3" />
### Running the site locally ###

1. Press **F5** to run the web site.

1. Navigate to **/question**.

	![Navigating to question](images/navigating-to-question.png?raw=true "Navigating to question")

	_Navigating to question_

1. Add a new question.

	![Adding a new question](images/adding-a-new-question.png?raw=true "Adding a new question")

	_Adding a new question_

1. Navigate to **/option** and add a new option selecting the recently created question in the **QuestionId** field.

	![Adding a new option](images/adding-new-option.png?raw=true "Adding a new option")

	_Adding a new option_

1. Close the browser to stop the solution.

<a name="Segment4" />
### Deploying to Windows Azure Web Sites ###

1. Right-click the **GeekQuiz** project and select **Publish…**

	![Publishing the Website](images/publishing-the-site.png?raw=true "Publishing the Website")

	_Publishing the Website_

1. In the Publish dialog box, click **Import…**.

	![Importing the publish profile](images/importing-a-publish-profile.png?raw=true "Importing the publish profile")

	_Importing the publish profile_

1. Select **Import from a Windows Azure web site**. To create a new Windows Azure Web Site, click on **New...**.

	![Creating a Windows Azure Website](images/creating-a-windows-azure-website.png?raw=true "Creating a Windows Azure Website")

	_Creating a Windows Azure Website_

1. The _Create site on Windows Azure_ dialog box will appear. Fill the Site name field and select **Create new server** in the **Database server** list (or use an existing one).

	![Create site on Windows Azure dialog](images/creating-a-new-website-dialog.png?raw=true "Create site on Windows Azure dialog")

	_Create site on Windows Azure dialog_

	> 	**Speaking Point:** 
	>
	You can create a new web site without a database, or a new web site with a new database on an existing database server, or a new web site with a new database on a new database server. Fill in all the required information and voila… your new Windows Azure Web Site is ready and you can deploy your web site project there. 

1. Click **Create** and wait until the site is created.

	![Waiting the site to be created](images/waiting-the-site-to-be-created.png?raw=true "Create site on Windows Azure dialog")

	_Waiting for the site to be created_

1. Back in the **Import Publish Profile** dialog, select the newly created site and click **OK**.

	![Importing the Publish Settings file from the new site](images/importing-the-publish-settings-from-the-new-site.png?raw=true "Importing the Publish Settings file from the new site")

	_Importing the Publish Settings file from the new site_

1. Back in the **Publish Web** dialog, click **Next >**.

	![Reviewing the connection settings to deploy](images/reviewing-the-connection-settings-to-deploy.png?raw=true "Reviewing the connection settings to deploy")

	_Reviewing the connection settings to deploy_

1. In the settings tab, select the new connection string for each context.

	![Updating the connection strings](images/updating-the-connection-strings.png?raw=true "Updating the connection strings")

	_Updating the connection strings_

1. Finally, click **Publish** to publish the site.

	![Publishing the site to the new Windows Azure Website](images/publishing-the-site-to-azure.png?raw=true "Publishing the site to the new Windows Azure Website")

	_Publishing the site to the new Windows Azure Website_

---

<a name="summary" />
## Summary ##

By completing this demo you should have:

* Created a new MVC application using the new One ASP.NET template
* Leared how MVC routing, views and controllers work
* Created GeekQuiz object model (TriviaQuestion and TriviaOption)
* Scaffolded the views and controllers for your model
* Created a new web site in Windows Azure and Deployed to it from within Visual Studio

---
