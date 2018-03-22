# Principles

## Modular instead of Monolithic
Build software with distinct pieces that have known dependencies. Ideally these modules can be replaced with better ones or improved independently of other modules.

## Systems instead of Snowflakes
Instead of writing a feature that only works in one special way, is there a way, with a little abstraction or redefinition, to make something useful in more places.

## Composition instead of Inheritance
Forcing one class to inherit another class just gain functionality makes it difficult to make one open source project to work with another open source project without modifying one or the other.

Using the principles of composition, especially with using object adapters, can make it easy to integrate disparate software projects. This means we don't have to fork other software projects just to make them work with ours.

## Less work instead of more work
Our code should positively affect other people. One easy way to do this is to make their job easier. While it may take more code to provide some shortcuts for our users, it is worth the effort.

## Pragmatism instead of Idealism

* Using the right tool for the job and willing to change the tool if (and only if) there is sufficient reason to do so
* Focus on what we do best and let third parties do the rest
* Find the balance between a hacked-together solution and an over-engineered solution

## Shipping beats perfection
Finding the absolute minimal viable product, shipping it and iterating on it ends up with better products. Shipping it allows us to gain more feedback and grow products in ways we hadn't thought.

## Strong opinions, weakly heldWe value the ability to [simultaneously have conviction and be open to doubt.](http://www.saffo.com/02008/07/26/strong-opinions-weakly-held/)  Anyone’s allowed to argue their opinions — even in areas other than their own. But we also listen, seek conflicting opinions, and flex in the face of new info.

## Leave it better than you found it
If you open a file and see some other issue go ahead and fix it.