# Live Project
## Project Introduction
From April 25th - June 1st, 2021 I worked in a team setting on a full scale ASP.NET MVC web application at Prosper I.T. Consulting. We worked with the Entity Framework to help build and update our website. We worked with a local Portland movie company to work on a website they could use without existing coding knowledge. Working on an existing codebase was a great way to get comfortable debugging unfamiliar code, refactoring working code, as well as implementing new features. This experience has shown me a lot about agile methodologies and has given me an appreciation for working on code collaboratively in a team setting. During this project we were given the opportunity to work on both [front end] and [back end](#Back-End-and-Mixed-Stories) stories so I took advantage of this and spent time on both types of stories, including some that were a mixture of the two. This was a very valuable experience for me and because of this I am confident working with both front and back end code and using them in conjunction to create web applications. Along with practical coding knowledge, I also learned many useful [skills](#Other-Skills-Learned) that will help me in my career as a software developer.

Featured below are some stories I worked on over the live project with code snippets to demonstrate what was accomplished.

## Back End and Mixed Stories
- [Creating Cast Members CRUD pages and functionality](#Cast-Members)
- [Creating Casting Director and seeding it to database](#Cast-Director)
- [Word Limit Method](#Word-Limit-Method)

### Cast Members
I was set with the task to create an Entity Model for the Cast Members pages. I did this with some reaserch on the entity framework and with a little time was able to complete the story. The next story was to create CRUD functionilty which was genereated pretty easily using Entity Framework. After completing the CRUD pages I needed to create an Index page that would be able to show these functionalities to the user. Below are the snippets of code from these projects minus the CRUD pages as those were generated with the Entity Framework
```C#
 public class CastMember
    {
        //Created an Enum for the MainRole property
        public enum PositionEnum
        {
            Actor,
            Director,
            Technician,
            StageManager,
            Other
        }

        public int CastMemberId { get; set; }

        public string Name { get; set; }

        public int? YearJoined { get; set; }

        public PositionEnum MainRole { get; set; }

        public string Bio { get; set; }


        //public Byte[] Photo { get; set; }

        public bool CurrentMember { get; set; }

        public string Character { get; set; }

        public int? CastYearLeft { get; set; }

        public int? DebutYear { get; set; }
    } 
```
```C#
@model IEnumerable<TheatreCMS3.Areas.Prod.Models.CastMember>

@{
    ViewBag.Title = "Index";
    Layout = "~/Views/Shared/_Layout.cshtml";
}

<h2>Index</h2>

<p>
    @Html.ActionLink("Create New", "Create")
</p>
<table class="table">
    <tr>
        <th>
            @Html.DisplayNameFor(model => model.Name)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.YearJoined)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.MainRole)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Bio)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.CurrentMember)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.Character)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.CastYearLeft)
        </th>
        <th>
            @Html.DisplayNameFor(model => model.DebutYear)
        </th>
        <th></th>
    </tr>

@foreach (var item in Model) {
    <tr>
        <td>
            @Html.DisplayFor(modelItem => item.Name)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.YearJoined)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.MainRole)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.Bio)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.CurrentMember)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.Character)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.CastYearLeft)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.DebutYear)
        </td>
        <td>
            @Html.ActionLink("Edit", "Edit", new { id=item.CastMemberId }) |
            @Html.ActionLink("Details", "Details", new { id=item.CastMemberId }) |
            @Html.ActionLink("Delete", "Delete", new { id=item.CastMemberId })
        </td>
    </tr>
}

</table>


```
### Cast Director
For this story I needed to create a user called Cast Director and then seed it to our database. Creating the user wasnt to complicated but when I was trying to learn how to seed the user to our database it took some time, a little trial and error, and the help of my team to get it to the spot I wanted. Below are snippets of my user and my seed method for this story. The issue I worked through was learning about the role manager and identity roles used in the Entity Framework.
```C#
 public class CastDirector : ApplicationUser
    {
        public int HiredCastMembers { get; set; }
        public int FiredCastMembers { get; set; }

        public static void CastDirectorSeed(ApplicationDbContext context)
        {
           
            var CastDirectorManager = new UserManager<ApplicationUser>(new UserStore<ApplicationUser>(context));
            var RoleManager = new RoleManager<IdentityRole>(new RoleStore<IdentityRole>(context));
                       
            var castDirectors = new CastDirector
            {
                HiredCastMembers = 4, FiredCastMembers = 2,
                Email = "castingDirectors@gmail.com",
                UserName = "CDirector", 
            };


            IdentityRole identityRole = new IdentityRole("CastDirector");
            

            RoleManager.Create(role: identityRole);
            CastDirectorManager.Create(user: castDirectors, password: "123-ABC");
            context.SaveChanges();

            var user = CastDirectorManager.FindByEmail("castingDirectors@gmail.com");
            var userId = user.Id;


            var RoleConnection = CastDirectorManager.AddToRole(userId: userId, role: "CastDirector");
        }
    }  
```
### Word Limit Method
For this story I was tasked with creating a method to limit words up to a specified point and end it with an ellipses.
```C#
 public static string LimitWords(string inputText, int wordLimit)
        {
            // Sets our return value to empty
            string returnedSentence = string.Empty;
            // Getting all the words from the string array using the space
            string[] words = inputText.Split(' ');

            // If the words is less than the length or less than or equal to 0 we want we return all the words
            if (words.Length < wordLimit || wordLimit <= 0)
                return inputText;

            // Returns our return sentence with a space from the index value of our array
            for (int i = 0; i < wordLimit; i++)
            {
                returnedSentence = string.Concat(returnedSentence, " ", words[i]);
            }
            // Adding the ellipses to our returned sentence
            if (words.Length > wordLimit)
                returnedSentence += "...";

            // Trimming the first space from our concatination
            return returnedSentence.Trim();
        }
```
## Other Skills Learned
- Collaborated with other developers to find bugs in order to improve user experience.
  - Finding issues with the pages I created before production.
  - Finding unhandled exceptions.
  - Fixing undesired outputs.
- Improved workflow efficiency by learning DevOps processes.
- Continuously learning new ideas and methologies from other developers to improve personally as a developer.
- Learned more on Entity Framework
