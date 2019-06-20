# Live Project
## Project Introduction
From February 25th - March 8th, 2019 I worked in a team setting on a full scale ASP.NET MVC web application at Prosper I.T. Consulting. Working on an existing codebase was a great way to get comfortable debugging unfamiliar code, refactoring working code, as well as implementing new features. This experience has shown me a lot about agile methodologies and has given me an appreciation for working on code collaboratively in a team setting. During this project we were given the opportunity to work on both [front end](#Front-End-Stories) and [back end](#Back-End-and-Mixed-Stories) stories so I took advantage of this and spent time on both types of stories, including some that were a mixture of the two. This was a very valuable experience for me and because of this I am confident working with both front and back end code and using them in conjunction to create web applications. Along with practical coding knowledge, I also learned many useful [skills](#Other-Skills-Learned) that will help me in my career as a software developer.

Featured below are some stories I worked on over the live project with code snippets to demonstrate what was accomplished.

## Back End and Mixed Stories
- [Deny Time Off Method](#Deny-Time-Off)
- [Clock in Indicator](#Clock-in-Indicator)

### Deny Time Off
Initially the method was not working correctly, I was tasked with refactoring the method so that an admin could deny a time off request from a user. I accomplished this by chcecking if the User's id matches the UserId property of the TimeOffEvent.
```C#
public ActionResult Deny(Guid id)
        {
            if (User.IsInRole("Admin"))
            {
                TimeOffEvent timeOffEvent = db.TimeOffEvents.Find(id);
                timeOffEvent.ActiveSchedule = false;

                ApplicationUser currentUser = System.Web.HttpContext.Current.GetOwinContext()
                    .GetUserManager<ApplicationUserManager>()
                    .FindById(System.Web.HttpContext.Current.User.Identity.GetUserId());

                List<TimeOffEvent> timeOffEvents = db.TimeOffEvents.Where(x => x.UserId == id).ToList();
                Message message = new Message()
                {
                    MessageId = Guid.NewGuid(),
                    DateSent = DateTime.Now,
                    Sender = db.Users.Find(currentUser.Id),
                    RecipientList = new List<Guid>(timeOffEvents.Select(x => x.UserId))
                };
                db.Messages.Add(message);
                db.TimeOffEvents.Remove(timeOffEvent);
                db.SaveChanges();

                return RedirectToAction("InboxAdmin");
            }
            else return View("AdminError");
        }
```
### Clock in Indicator
For this story I had to create a method that would check if a user was clocked in or not and display that to the user.
```C#
public bool IsUserClockedIn(Guid studentId)
        {
            bool clockedIn = db.WorkTimeEvents.Any(x => x.End == null
                                                        && x.UserId == studentId);
            return clockedIn;
        }

        public PartialViewResult ClockStatus()
        {
            bool isClockedIn;

            if (User.Identity.IsAuthenticated)
            {
                Guid id = Guid.Parse(User.Identity.GetUserId());
                isClockedIn = IsUserClockedIn(id);  
            }
            else { isClockedIn = false; }

            return PartialView("_ClockStatus", isClockedIn);
        }
```
## Front End Stories
- [Navbar Styling](#Navbar-Styling)
### Navbar Styling
I was given the task to change the background color and text color of the menu navabar. This seemed very simple at first but I ran into issues overriding bootstrap classes. This experience helped me understand how to leverage bootstrap in a more intuitive way and to customize bootstrap styling.
## Other Skills Learned
- Collaborated with other developers to find bugs in order to improve user experience.
  - Redirecting the user to the appropriate page.
  - Finding unhandled exceptions.
  - Fixing undesired outputs.
- Improved workflow efficiency by learning DevOps processes.
- Continuously learning new ideas and methologies from other developers to improve personally as a developer.

