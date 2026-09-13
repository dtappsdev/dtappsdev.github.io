# darrentan90.github.io

The developer website for **My HoBeh** (`com.myhobeh`). Three files, and each
one is load-bearing for something outside this repo.

| File | Why it exists |
| --- | --- |
| `app-ads.txt` | Google's crawler reads the **Developer website** URL from the Play listing, takes the hostname, and fetches `/app-ads.txt` from its ROOT. Without it AdMob reports "we couldn't complete app verification" and buyers who require an authorized-sellers file will not bid. |
| `privacy.html` | The privacy policy URL the Play listing points at. It has to stay reachable for as long as the app is listed. |
| `index.html` | The Developer website itself. It is what makes the app-ads.txt hostname resolvable, and a listing whose developer site is a blank page reads badly to a reviewer. |

## Three things not to do

1. **Do not move `app-ads.txt` into a subdirectory.** The spec only ever looks
   at the root of the hostname. `…github.io/app-ads.txt` is found;
   `…github.io/anything/app-ads.txt` is not, and nothing will tell you.
2. **Do not rename this repo.** A GitHub Pages *user* site is served at the
   root of `darrentan90.github.io` only because the repo is named exactly that.
   Rename it and Pages moves everything under a subpath, which silently breaks
   app-ads.txt by rule 1.
3. **Do not change `f08c47fec0942fa0`** in `app-ads.txt`. It is Google's own
   certification authority ID and is the same constant for every publisher.
   Only the `pub-…` part is yours.

## After changing app-ads.txt

The crawl is not instant. Give it up to 24 hours, then AdMob → app-ads.txt →
**Check for updates**. `curl -sI https://darrentan90.github.io/app-ads.txt`
confirms your end is serving it before you go looking at Google's.
