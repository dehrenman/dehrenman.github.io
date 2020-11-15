---
layout: post
title:  "The Pink Wojak Index: Measuring Despair in the Cesspool of the Internet"
date:   2020-11-15 14:22:19 -0400
categories: 
---
[wojakindex.com](http://wojakindex.com) gauges the level of stock crash despair using through the cesspool of the internet, [4chan /biz/](https://boards.4channel.org/biz/catalog).

## Inspiration
My inspiration came from two places.

First,
Covid screening is done en-masse using wastewater[^uvacovid]

Second, this video: the pink fiends.
<iframe width="320" height="180" src="https://www.youtube.com/embed/BkX2NAKFM5Q" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Specifically, this part in the video:

<div style="max-width:320px; max-height:180px;"><div class="tenor-gif-embed" data-postid="16567170" data-share-method="host" data-width="100%" data-aspect-ratio="1.7785714285714287"><a href="https://tenor.com/view/wojak-index-pink-levels-gif-16567170">Wojak Index Pink Levels GIF</a> from <a href="https://tenor.com/search/wojakindex-gifs">Wojakindex GIFs</a></div><script type="text/javascript" async src="https://tenor.com/embed.js"></script></div>


[^uvacovid]: https://news.virginia.edu/content/inside-science-testing-wastewater-uva-evidence-covid-19

## Implementation
I used [ApiFlash](apiflash.com) to generate daily screenshots of /biz/, with injected CSS to remove the header and footer of the page and upsize the images. This produces a nice picture-centered textless view of /biz/.

I then used [sklearn](https://scikit-learn.org/stable/) to cluster the pixels based on color, and generate a color distribution diagram. The clustering is trained on a combination between the webpage and a sample of wojak pictures to ensure the pink wojak color has its own cluster. Then the webpage is clustered on its own and the cluster distributions are displayed. The percentage of the page that is the pink wojak color ![#FF99CC](https://via.placeholder.com/15/ff99cc/000000?text=+) `#FF99CC`

Each day's screenshot is saved in a folder in the form hash.png. The url to that image, the date, and the wojak index are all archived in a sqlite database.

The dashboard itself is made using [Django](https://www.djangoproject.com/) and hosted on a [Digitalocean](https://www.digitalocean.com/) droplet.


## Notes

I don't consider myself a member of 4chan, and I don't agree with the conspiracy theorists on 4chan. But the despair is real and palpable in the face of Pink Wojak.
