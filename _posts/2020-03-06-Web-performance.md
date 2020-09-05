---
layout: post
title: "My Learnings about web-performance"
date: 2020-03-06 07:14:20 +0530
categories: web performance
---

The Main Components of the web performance:- 

**Network Time** + **Generation Time** + **Render Time**

The Main Goal here is to cut down on the HTML , CSS, Javascript, Images etc. for the pages recieved which would lower the **Network Time** and the **Render Time**. 

Some of the Strategies Include :- 

* Cutting down the Cookie Bytes/page
* Cutting Down on the Javascript (storing it client side, using component library for common features)
* Caching and Reusing CSS bytes per page since components share CSS rules 
* PIPELINING THE WHOLE THING:-  Sending partial responce at certain time interval (say 10-15 ms) to the browser , thus reducing overall **Time-To-Interact(TTI)**  [Overlapping Render time with the Generation time ]



# Important links to refer for above topic

1. [the-secret-formula-hidden-tips-for-improved-web-performance](https://medium.com/engineering-housing/the-secret-formula-hidden-tips-for-improved-web-performance-a7dc37e2f24f)
2. [instant. page](https://instant.page/intensity)
3. [yuppi. es](https://yuppi.es/)
4. [facebook note about web performance](https://www.facebook.com/note.php?note_id=307069903919)
5. [facebook BigPipe](https://www.facebook.com/notes/facebook-engineering/bigpipe-pipelining-web-pages-for-high-performance/389414033919/)
6. [google developers web fundamantals](https://developers.google.com/web/fundamentals/)
7. [lazy loading](https://addyosmani.com/blog/lazy-loading/)
8. [web dev measure](https://web.dev/measure/)
9. [logrocket rethinking frontend apm](https://blog.logrocket.com/rethinking-frontend-apm/)
10. [divjot singh housing SDE3 slides on web performance](https://docs.google.com/presentation/d/1snxo-EKx6PseQeznhfr-VVTEQ71EAMOdbDQ0pNR5jEA/edit#slide=id.g6b8bf89bb2_0_43)

*More links to be added soon*

## Advice for best learning such thing is to try all the learned concepts on nay website , build some projact around these learnings


