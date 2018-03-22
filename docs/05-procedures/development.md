====================
Development Workflow
====================

Overall Workflow
================

Our overall development workflow is pretty simple:

1. Develop
2. Create test instance
3. Quality Assurance
4. Deployment


Core Project
============

#. :ref:`Create a branch from master <branching_core_project>`. See also :ref:`grouping_small_issues`.
#. Develop.
#. :ref:`Periodically commit code <commit_practices>`
#. Pull Request for review in github
#. Merge feature back to master



.. _branching_core_project:

Branching from the Core Project
===============================

Name the branch the Jira issue key without dashes and all lowercase. For example, if the Jira issue key was EDU-6803:

.. code:: bash

    $ git checkout -b edu6803


.. _grouping_small_issues:

Grouping Small Issues
=====================

Some times many small issues

.. _commit_practices:


Commit Practices
================

The ideas behind our commiting code practices is based on the assumption:

- Someone may need to find the commit again
- Someone may need to undo this commit
- You will be the "someone" dealing with someone else's code.

Therefore:

- Make the commit easy to find
- Keep the changes as small as possible to make undoing the commit easy

Guidelines
----------

#. Make lots of small commits -- they're cheap!
   `HINT:` Use a GUI like `SourceTree`_ to assist in selecting the files and chunks of code.
#. Start your commit message with the Jira issue key.
#. Make the subject line of what the change is doing/fixing/undoing/etc.
#. Use Jira `smart commits`_ to move the ticket to the next phase.
#. Write the commit message as if it finishes the sentence: "This commit will ..."

Examples
--------

.. code:: bash

    $ git commit -m"EDU-6803 Create new Tron program to fight MCP"
    $ git commit -m"EDU-6803 Configure Tron with MCP derezzing instructions"
    $ git commit -m"EDU-6803 #qa Derezz the master control program"

.. _smart commits: https://confluence.atlassian.com/fisheye/using-smart-commits-298976812.html

.. _SourceTree: https://www.atlassian.com/software/sourcetree

New Apps/Projects
=================

1. Create github private/public repo
2. Develop project
3. Push to github
4. Upload to PyPI
5. Add app/feature to Natgeoed if required
6. Test feature in test instance
