# Attribution

## Image Sources

Images used in this demonstration were obtained from the internet for educational purposes only. Not for commercial use.

Original album cover art images have been scaled, cropped to 384x384 pixels (96 DPI), and converted to PNG format.

### The Beatles Logo
#### Wikimedia Commons
  - [File:The Beatles logo.svg](https://commons.wikimedia.org/wiki/File:The_Beatles_logo.svg)

### The Beatles Album Art
#### The Beatles
1. [Please Please Me](https://www.thebeatles.com/sites/default/files/styles/responsive_thumbnail_mobile/public/2021-06/Please%20Please%20Me.jpg?itok=tzOPWi8O)
3. [With the Beatles](https://www.thebeatles.com/sites/default/files/styles/responsive_thumbnail_mobile/public/2021-06/With%20the%20Beatles.jpg?itok=3cGBeX7f)
5. [A Hard Day's Night](https://www.thebeatles.com/sites/default/files/styles/responsive_thumbnail_mobile/public/2021-06/A%20Hard%20Days%20Night.jpg?itok=gDdWFeas)
6. [Beatles for Sale](https://www.thebeatles.com/sites/default/files/styles/responsive_thumbnail_mobile/public/2021-06/The%20Beatles%20for%20Sale.jpg?itok=wCTmlGz3)
7. [Help!](https://www.thebeatles.com/sites/default/files/styles/responsive_thumbnail_mobile/public/2021-06/Help.jpg?itok=Jz2wnyjj)
8. [Rubber Soul](https://www.thebeatles.com/sites/default/files/styles/responsive_thumbnail_mobile/public/2021-06/Rubber%20Soul.jpg?itok=TsRSHu1Q)
9. [Revolver](https://www.thebeatles.com/sites/default/files/styles/responsive_thumbnail_mobile/public/2021-06/Revolver.jpg?itok=J0Q8YaGs)
10. [Sgt. Pepper's Lonely Hearts Club Band](https://www.thebeatles.com/sites/default/files/styles/responsive_thumbnail_mobile/public/2021-06/Sgt%20Pepper.jpg?itok=0CcJLuzl)
11. [The Beatles (White Album)](https://www.thebeatles.com/sites/default/files/styles/responsive_thumbnail_mobile/public/2021-06/WA_Press_26.jpg?itok=WwQjTuUy)
12. [Yellow Submarine](https://www.thebeatles.com/sites/default/files/styles/responsive_thumbnail_mobile/public/2021-06/Yellow%20Sub.jpg?itok=aj5oY5EQ)
13. [Abbey Road](https://www.thebeatles.com/sites/default/files/styles/responsive_thumbnail_mobile/public/2021-06/Abbey%20Road.jpg?itok=OWcQY3Ee)
14. [Let It Be](https://www.thebeatles.com/sites/default/files/styles/responsive_thumbnail_mobile/public/2021-06/CoverLetItBe.jpg?itok=giGHBt2f)


#### Wikipedia
1. [Please Please Me](https://en.wikipedia.org/wiki/Please_Please_Me)
2. [Introducing... The Beatles](https://en.wikipedia.org/wiki/Introducing..._The_Beatles)
3. [With the Beatles](https://en.wikipedia.org/wiki/With_the_Beatles)
4. [Meet the Beatles!](https://en.wikipedia.org/wiki/Meet_the_Beatles!)
5. [A Hard Day's Night](https://en.wikipedia.org/wiki/A_Hard_Day%27s_Night_(album)#North_American_release)
6. [Beatles for Sale](https://en.wikipedia.org/wiki/Beatles_for_Sale)
7. [Help!](https://en.wikipedia.org/wiki/Help!#North_American_Capitol_release)
8. [Rubber Soul](https://en.wikipedia.org/wiki/Rubber_Soul)
9. [Revolver](https://en.wikipedia.org/wiki/Revolver_(Beatles_album))
10. [Sgt. Pepper's Lonely Hearts Club Band](https://en.wikipedia.org/wiki/Sgt._Pepper%27s_Lonely_Hearts_Club_Band)
11. [The Beatles (White Album)](https://en.wikipedia.org/wiki/The_Beatles_(album))
12. [Yellow Submarine](https://en.wikipedia.org/wiki/Yellow_Submarine_(album))
13. [Abbey Road](https://en.wikipedia.org/wiki/Abbey_Road)
14. [Let It Be](https://en.wikipedia.org/wiki/Let_It_Be_(album))

### Stereo and Mono icons
  - [Stereo icons created by Tanah Basah - Flaticon](https://www.flaticon.com/free-icons/stereo)

## Discography

The following information provided in the Excel datasource was obtained via interaction with [Microsoft Copilot](https://copilot.microsoft.com/).

### Releases

A **Release** is a specific physical or commercial manifestation of an album.
Releases differ by Country (UK, US), Label (Parlophone, Capitol, Vee‑Jay, Apple), Mix (mono, stereo), Tracklist, Cover art, Catalog number, and Release date. 
Releases capture the real‑world diversity of Beatles albums across markets and formats.
- Album Id
- Album Number
- Country Code
- Canonical Album ID (FK)
- Album Name
- Release Year
- Label
- Mix

---

### Releases to Song

This is the relational bridge table that connects a release_id to the songs that appear on that release with the track_number, track_title, and writer. Each row represents one track on one release.
- Release ID
- Track Number
- Track Title
- Write

---

### Canonical Album

A **Canonical Album** represents the artistic work as defined by the Beatles’ official UK studio album sequence. It is **not** a physical product. It is an abstract, stable entity that groups together all releases derived from the same underlying album.
- Canonical Album ID (PK)
- Album Name
- Canonical Release Year
- Notes

---

### Canonical Song

A **Canonical Song** is the unique, abstract representation of a Beatles composition or recording. It is not tied to any specific release, mix, or track number. Canonical Songs allows all releases, mixes, and tracklists to point back to a single authoritative song entity.
- Canonical Song ID (PK)
- Song Title
- Writer
