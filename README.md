# dtappsdev.github.io

The developer website for **My HoBeh** (`com.myhobeh`), published by DTapps.io.
Three files, and each one is load-bearing for something outside this repo.

| File | Why it exists |
| --- | --- |
| `app-ads.txt` | Google's crawler reads the **Developer website** URL from the Play listing, takes the hostname, and fetches `/app-ads.txt` from the ROOT of it. Without it AdMob reports "we couldn't complete app verification" and buyers who require an authorized-sellers file will not bid. |
| `privacy.html` | The privacy policy URL the Play listing points at. It has to stay reachable for as long as the app is listed. |
| `index.html` | The Developer website itself. It is what makes the app-ads.txt hostname resolvable, and a listing whose developer site is a blank page reads badly to a reviewer. |

## Why this lives in an organisation and not a personal account

A GitHub Pages *user* site takes its hostname from the **account name**, not the
repo name. The site was first put up at `darrentan90.github.io`, which published
a real name on the Play listing; the fix was an organisation called `dtappsdev`,
because that is the only way to get `dtappsdev.github.io`.

**Renaming the `darrentan90` account was NOT the fix, and must not be
attempted.** `src/config.ts` in the app ships
`https://raw.githubusercontent.com/darrentan90/hobeh-draws/main/latest.json`,
and every installed copy of My HoBeh fetches that exact URL for new draws.
GitHub's post-rename redirects are not worth betting shipped apps on, and the
failure mode is silent: pull-to-refresh simply stops finding draws.

That URL is also the limit of what this move achieves. The handle is off the
Play listing, which is the surface users see, but it is still inside every
build for anyone who inspects the APK.

## Four things not to do

1. **Do not move `app-ads.txt` into a subdirectory.** The spec only ever looks
   at the root of the hostname. `…github.io/app-ads.txt` is found;
   `…github.io/anything/app-ads.txt` is not, and nothing will tell you.
2. **Do not rename this repo, or the organisation.** A Pages user site serves at
   the root of `dtappsdev.github.io` only because the org is named `dtappsdev`
   and the repo is named exactly `dtappsdev.github.io`. Change either and Pages
   moves everything under a subpath, which silently breaks rule 1.
3. **Do not change `f08c47fec0942fa0`** in `app-ads.txt`. It is Google's own
   certification authority ID and is the same constant for every publisher.
   Only the `pub-…` part is ours.
4. **Do not make org membership public.** If `darrentan90` shows as a public
   member of `dtappsdev`, the name this move exists to keep off the listing is
   one click away again. Organisation → People → your row → **Private**.

## After changing app-ads.txt

The crawl is not instant. Give it up to 24 hours, then AdMob → app-ads.txt →
**Check for updates**. `curl -sI https://dtappsdev.github.io/app-ads.txt`
confirms our end is serving it before going to look at Google's — it must answer
`200` with `content-type: text/plain`.
