## 1.0 Motivation for Hackystat Projects ##

Before going through this material, we suggest that you read the [Guided Tour](Tutorial_GuidedTour.md) so that you have a high level overview of Hackystat.

There are two primary motivations for the "Project" representation in Hackystat:

  1. Many people work on more than one task at a time. It is sometimes useful to be able to analyze the work you are doing on Foo separately from the work you are doing on Bar.  Of course, it might be nice to also analyze both projects together.

  1. Many people work on tasks with other people.  In this case, it is sometimes useful to aggregate data collected from your work activities with the data collected by others.  For example, you might be developing software with three other people and be interested in the total number of unit tests invoked by all members of the project.  Or, you may have a daily build mechanism that automatically runs tests, computes coverage, and generates size data.  It would be nice if that sensor data can be shared by all members of the project, rather than forcing each person to generate it individually.

The Project representation in Hackystat is designed to satisfy these two situations (as well as the combination of the two, such as a project Foo worked on by Bob and Sue, and a project Bar worked on by Bob and Alice.)

Projects also have start and end dates, which allow you to analyze data associated with a single increment of development, for example.

## 2.0 Concepts ##

There are just a few basic concepts you need to understand in order to use Hackystat Projects.

### 2.1 The Default project ###

Every user in Hackystat has an automatically generated Project called "Default" created for them at the time that their account is registered.  The Default project is defined to specify all of the data you have ever sent to Hackystat for this account.  If you don't work with other people, and if you don't care about analyzing portions of your Hackystat data separately from other portions, then the Default project is all you need.

### 2.2 UriPatterns ###

If you want to specify a project that includes only a portion of your sensor data, you do that using UriPatterns.  A UriPattern looks like a file path, except it can include a `*` to indicate zero or more characters.  For example, the UriPattern

```
*/Foo/*
```

matches:

```
/Foo/
/Foo/Bar.java
/users/johnson/projects/Foo/Bar.java
```

Usefully, it also matches:

```
\Foo\
\Foo\Bar.java
c:\projects\Foo\Bar.java
```

In other words, UriPatterns are designed to ignore the difference between the path separator character on Unix ("/") and Windows ("\").

UriPatterns match against the Resource field associated with each sensor data instance.  If the match succeeds, then the sensor data instance is included in the analysis.

You can specify multiple UriPatterns. They form an implicit "or".   For example:

```
*/Foo/Bar/*
*/Foo/Baz/*
```

will match both the Resource string "/Foo/Bar/bar.txt" and the Resource string "/Foo/Baz/baz.txt", but it will not match the Resource string "/Foo/Qux/quz.txt".

### 2.3 Project Ownership, Invitations, Membership, and Spectators ###

By joining a project, you implicitly agree to allow your sensor data to be accessible to other members of the project.  Since this has privacy implications, Hackystat has an "opt-in" policy toward project membership.  Here's how it works:

  * The person who creates a project is the project "Owner".  Only the owner of a Project can delete it.  The owner of a Project can also remove any current Members from the Project.  However, the owner cannot add new Members directly. Instead, the owner can "invite" members by putting their Hackystat user name (email) in the Invitations field associated with the Project.

  * When you have been invited to a Project by its Owner, then that Project will show up in the Projects page of your ProjectBrowser. You will be listed as an "Invitee".   You now have the option of "accepting" that invitation and becoming a Project member, or "declining" the invitation, and thus being removed from the invitation list.

  * Once you are a member of a Project, you can perform analyses using data from that Project. Your data will now be used in Project analyses along with other Project members (and the Project owner).  At any time, you can remove yourself from the Project.

  * Note that the Default project is, in a sense, read-only.  You cannot make any changes to that Project.

  * A Project owner can also add "Spectators".  Spectators are Hackystat users who can run analyses related to a Project but whose sensor data is not utilized in the analysis.  Spectators do not go through the "invitation" process; a Project owner can add (or delete) Spectators at any time and they immediately get (or rescind) that capability.

## 3.0 The Project User Interface ##

### 3.1 Project Descriptions and User Roles ###

Hackystat provides a user interface to Projects through the Projects page in the ProjectBrowser.  The public ProjectBrowser is available at http://dasha.ics.hawaii.edu:9879/projectbrowser/.  Here is a screen shot that illustrates the Project page after the user joe.simpletelemetry@hackystat.org logs in:

![http://hackystat.googlecode.com/svn/wiki/projectviewer01.gif](http://hackystat.googlecode.com/svn/wiki/projectviewer01.gif)

As you can see, this user is associated with two projects: the "Default" project which is defined by the system for all users, and a user-defined project called "simpletelemetry".   The definition of each project is contained in a single row, and there are a set of buttons on the left side that indicate the possible actions that the user can take on a project.

The current user's role in the Project is made more apparent by highlighting their username where it appears. In this case, it is easy to see that Joe owns both of the projects with which he is associated.

You can see that there are no buttons associated with the Default project, which means that the user has no control over that project definition.

The user-defined "simpletelemetry" project has three buttons associated with it: "edit", "rename", and "delete".  Those are the three top-level actions that a user can take on a project that they own.

You can also see that the simpletelemetry has a member named bob.simpletelemetry@hackystat.org and a spectator named kim.simpletelemetry@hackystat.org.  As noted above, data from members is included in project-based analyses, while data from spectators is not.

Let's look at Bob's Project page for an interesting contrast:

![http://hackystat.googlecode.com/svn/wiki/projectviewer02.gif](http://hackystat.googlecode.com/svn/wiki/projectviewer02.gif)

First, you can see that Bob's username is highlighted under the Owner column for his Default project. For the simpletelemetry project, his username appears as a Member.

Like Joe's Project page, Bob cannot edit his Default project.  Unlike Joe's Project page, Bob has a single button associated with the simpletelemetry project, which is "leave".  This indicates the only legal action that a member can take on a project: they can stop being a member.

Finally, let's look at Kim's Project page:

![http://hackystat.googlecode.com/svn/wiki/projectviewer03.gif](http://hackystat.googlecode.com/svn/wiki/projectviewer03.gif)

As you can see, Kim owns her own Default project, and is a spectator on the simpletelemetry project.  Kim cannot change her Spectator status: she would have to contact Joe and ask him to remove her if she no longer wanted that role on that project.

### 3.2 Defining new projects and inviting members ###

Let's say that Kim and Bob are working on a new travel website project and they want to perform Hackystat analyses over their combined data.  They decide that Kim will define the project and invite Bob as a member. Joe is interested in following their progress, but will not be doing any work, so he will be listed as a spectator.  Kim presses the "New" button to define a new Project, and the following screen appears:

![http://hackystat.googlecode.com/svn/wiki/projectviewer04.gif](http://hackystat.googlecode.com/svn/wiki/projectviewer04.gif)

Kim fills in the Project name, start and end dates, and description fields. She invites Bob to be a member, and sets the URI Pattern to match only that project's data:

![http://hackystat.googlecode.com/svn/wiki/projectviewer05.gif](http://hackystat.googlecode.com/svn/wiki/projectviewer05.gif)

After pressing the Save button, the newly created Project is listed:

![http://hackystat.googlecode.com/svn/wiki/projectviewer06.gif](http://hackystat.googlecode.com/svn/wiki/projectviewer06.gif)

Kim now sends Bob an IM to tell him that he's been invited to the AwayWeGoWebsite project and to please login and accept the invitation.  Bob gets the IM, logs in, and sees the following on his Projects page:

![http://hackystat.googlecode.com/svn/wiki/projectviewer07.gif](http://hackystat.googlecode.com/svn/wiki/projectviewer07.gif)

Bob presses the "Reply" button and gets the following screen:

![http://hackystat.googlecode.com/svn/wiki/projectviewer08.gif](http://hackystat.googlecode.com/svn/wiki/projectviewer08.gif)

He presses the "accept" button, which establishes him as a Member and returns him to the updated Projects page:

![http://hackystat.googlecode.com/svn/wiki/projectviewer09.gif](http://hackystat.googlecode.com/svn/wiki/projectviewer09.gif)

As you can see, Bob is now a member of the project with an "(m)" following his name.  The button associated with that project is now "leave", which reflects the action associated with that role.

### 3.3 Editing an existing project ###

If Kim wishes to change the details of the AwayWeGoWebsite project, she can press the "edit" button to get the following page:

![http://hackystat.googlecode.com/svn/wiki/projectviewer10.gif](http://hackystat.googlecode.com/svn/wiki/projectviewer10.gif)

This page enables her to change various fields.  Note that to delete any existing members, she can highlight their names and then click the "Delete selected members(s)" link.

To change the list of Invited members or Spectators, she can just edit those text boxes directly, adding and subtracting usernames, one per line.

Remember: only the owner of a project can edit its definition.


## 4.0 Miscellaneous issues ##

  * Note that the system does not send emails or any other notification when the users associated with a project change roles in some way.  It is up to the users to employ their favorite communication mechanisms to inform team members of status updates.

  * The "Properties" fields associated with a Project is not currently used in Hackystat analyses, and can be ignored for the present.




















