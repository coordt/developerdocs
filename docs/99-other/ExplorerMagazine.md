Explorer Magazine

Product
    Name
    Description
    Price
    Editions


Subscription
    User
    Parent Subscription (for "group" subscriptions, where the parent user invites other users)
    Product
    Start date
    End date


Product Issue
    Product
    Issue Date
    Cover Image
    Description
    Print Password (password printed in magazine to allow print subscribers to download content)


Issue File
    Product Issue
    Edition
    Issue file
    Is Interactive Edition
    For Print Subscriber (is this file available for print subscribers)


Product Manager Story:

    Products would be the Scout, Voyager, Pioneer, Trailblazer, Pathfinder and Adventurer.

    Each Product would have Editions of Interactive, Teacher, Projectable, Whiteboard.

    Content producers will create a Product Issue record for each Product when an issue is released.

    For each Product Issue record, they can upload one or more Issue Files, assigning each file an Edition.


Digital Subscriber Story:

    A subscriber goes to a special login page that asks for their email address and password.

    Validation is that the email address is a valid address and the password matches a configured value.

    Upon a successful validation, a new User is created with:
        - a username of the pre-@ of the email address (checking first for uniqueness),
        - a password of the default configured password
        - an email of their email address

    Then a Subscription record is created for each Product/User with a start date of today and and end date of today + 1 year.

    The new User is logged in and redirected to their subscription page.


Print Subscriber Story

    A print subscriber goes to a special login page that asks only for the issue password.

    The issue password is looked up in the Product Issue table.

    The results page is the same for all results.
    No caching for the results page.
    The files that have For Print Subscriber equal to True are displayed on the results page.

Navigation:
    Month
        Shows Products with Issues in that Month with brief descriptions

    Products
        Shows descriptions of each product, its editions, etc.

    My Subscriptions
        Not cached (since the same URL is used for every user)
        Requires log in
        Shows listing of subscribed issues descending by issue date with links for available edition files
