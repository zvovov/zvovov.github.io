---
title: A wild python package appeared
tag: [python, web-scraping]
excerpt: "Find and listen to songs featured in TV shows"
header:
  teaser: /assets/images/pypi.png
  overlay_image: /assets/images/pypi.png
  overlay_filter: 0.9
  actions:
    - label: "pip install tvsongs"

---

# Idea was conceived

With the onslaught of affordable sophisticated hardware putting a mobile device in every person's hands (and lap), the content was bound to come. And it came, no holds barred. There are many quality TV shows to watch. More are being produced everyday.

I was watching one such by-product of this content creation phenomenon. [^1] So impressed by the placement of a song in a scene, I googled to find more about it. A couple minutes later, I managed to find the song.

Knowing a song's name shouldn't take this long, I thought. And began to turn the *shouldn't* into *doesn't.*

# Tools of the trade were chosen

**Python** because it is fun and I was familiar with the web scraping library [beautifulsoup](https://www.crummy.com/software/BeautifulSoup/).

# Keyboard bashing began

I didn't start with a goal to create a python package. Development began with a single .py file. Just one script to do the job.

The first commit sums up what I thought the job was initially.

<script src="https://gist.github.com/zvovov/581d5ff1edf425544015b009d078295b.js"></script>

I was aware of the TuneFind API, but rather than using it, I seized an opportunity to scrape the web, and I love me some web-scraping. ;)

So after storing tunefind's HTML response for a show, [^2] locally, beautifulsoup's services were called for.

Walking through the HTML tag jungle was getting murkier but I carried on anyway. Meanwhile, the thought of changing the code everytime tunefind decided to alter the UI, however slightly, made me cringe. Still, I was beginning to come to terms with it.

That was before I noticed this in the response.

```javascript
 window.__INITIAL_STATE__={"activeQuestion":0,"activeSong":0,"activeSongEvent":0,
 ...
 {"id":35422,"event_group_id":3024,"name":"Gym","number":12,"original_air_date":1283817600,"locked":false,"episode_description":"Louis tries to get healthy.","question_count":0,"song_count":0,"airdate_day":"07","airdate_month":"09","airdate_month_short":"Sep","airdate_year":"2010","is_tombstone":false,"tombstone_conflict":false}
```

Now, I'm no js expert, but it was clear [^3] that this data was being used by React to populate the appropriate fields while rendering the UI. It contains all the show and song related data.

This js code is less likely to change than the HTML code of the page. So I loaded this in python using ``` json.loads() ``` and soaked in the momentary bliss.

What was previously:

```python
_season_count = int((soup.find_all('ul')[1].find_all('li')[-1].find('a')['href'])[-1])
```

became:

```python
_season_count = len(_json['show']['result']['seasons'])
```

Code readability increased, which reduced the development time. Great!

List of all songs from any TV show's any season's any episode could now be summoned, on demand. A link to each song's YouTube search page was also displayed, so one could listen to the song right away.

At this point, the script had become worthy of sharing. But setting up the environment (installing 3 python modules) to run the script was too much effort. I wouldn't install 3 modules to get some script working. There had to be a better way...

# The better way - Python Packaging

The thought that my code would be on PyPI caused much excitement.

I had the misconception that registering one's package on PyPI, which would make it accessible via a ``` pip install ``` command, involved some sort of code verification by distinguished python developers, and the process took days. But, fortunately, that is not the case. Anyone can upload their package on PyPI and it's available via pip instantly.

Finally, **tvsongs** ended up on PyPI with the help of this great guide. [^4] Shoutout to [storborg](https://github.com/storborg), the guide's creator. Shoutout to [sharad5](https://github.com/sharad5) for contributing to the enhancement of tvsongs.

> Install:
> ``` pip install tvsongs ```
>
> Know more:
> [github.com/zvovov/tvsongs](https://github.com/zvovov/tvsongs)
>
> PyPI: 
> [pypi.python.org/pypi/tvsongs](https://pypi.python.org/pypi/tvsongs)


[^1]: [Orange Is the New Black](http://www.imdb.com/title/tt2372162/)
[^2]: [Louie](http://www.imdb.com/title/tt1492966/)
[^3]: Hindsight is 20/20
[^4]: [python-packaging](https://python-packaging.readthedocs.io/en/latest/)

*[PyPI]: Python Package Index
*[js]: javascript
*[UI]: User Interface
*[API]: Application Programming Interface
*[HTML]: HyperText Markup Language
