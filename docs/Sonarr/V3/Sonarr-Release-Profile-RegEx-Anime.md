# Release Profile RegEx (Anime)

!!! note
    This Guide is Created and Maintained by [Visorask](https://github.com/Visorask)

    It's recommended to run 2 Sonarr's 1 for Anime and 1 for normal tv shows, or you can make use of tags.

    Being that release profiles are global without the use of a tag.

## Dual-Audio Regex

Having tested most of these settings, they work pretty well but Anime is a fickle beast to toy with so be warned.

---

## Anime Delete

??? tip "If you have issues with extra files being download like NCED or NCOP"

    If you have issues with sonarr picking up a bunch of:

    - NCED
    - NCOP
    - Music Videos
    - OP's

    Then this script (linux based only) will delete these files and get rid of them and you don't have to worry about them cluttering up your Auto or Manual import process.

    ```bash
    #!/usr/bin/env bash

    # Set the following location to your completed download location for Anime.
    # Usually one of the following paths:
    # Dockers => /data/{usenet|torrents}/anime
    # Cloudbox => /mnt/local/downloads/nzbs/nzbget/completed/sonarranime
    location="/your/download/path/here"

    find $location -type f \( -iname "*op[0-9]*" -o -iname "*nced*" -o -iname "*ncop*" -o -iname "*music video*" \) -exec rm -rf {} \;
    ```

    This script will be called through a custom connection in sonarr.

    1. Copy the contents of the script into whatever filepath sonarr has access to (`/config/app/` or `/appdata/sonarr/scripts`)
    1. Edit the `location="/your/download/path/here"` and make sure it's in quotes.
    1. Then save it and chmod +x the script.
    1. In Sonarr go to `Settings` => `Connect` => Click on the `+` Symbol => and select `Custom Script`
    1. Change Name to whatever you like.
    1. Select `On Grab` and `On Import`.
    1. Click the Folder icon or type in the path to the script you chose earlier.

    ![!rpa-customscript](images/rpa-customscript.png)

    Now when you download or import any files the script will run and clear out those files that you most likely won't use and if you want them well this script isn't for you.

## Media Management

!!! note

    There a few settings which make life a lot easier when tracking down and keeping certain information for your Anime and with tweaks to make it work better with [HamaAgent](https://github.com/ZeroQI/Hama.bundle){:target="_blank" rel="noopener noreferrer"} for plex.

### Suggested naming scheme

For this I used the [Sonarr Recommended naming scheme](/Sonarr/V3/Sonarr-recommended-naming-scheme/#anime-episode-format){:target="_blank" rel="noopener noreferrer"} for Sonarr with only a small change for a formatting error to be fixed and the `Series Folder Format` for easier matching.

#### Anime Episode Format

Has all necessary information and a little extra if you needed to rebuild.

```bash
{Series TitleYear} - S{season:00}E{episode:00} - {absolute:000} - {Episode CleanTitle} [{Preferred Words }{Quality Full}]{[MediaInfo VideoDynamicRange]}[{MediaInfo VideoBitDepth}bit]{[MediaInfo VideoCodec]}[{Mediainfo AudioCodec} { Mediainfo AudioChannels}]{MediaInfo AudioLanguages}{-Release Group}
```

??? summary "RESULTS:"

    Single Episode:

    `The Series Title! (2010) - S01E01 - 001 - Episode Title 1 [AMZN WEBDL-1080p v2][HDR][10bit][x264][DTS 5.1][FLAC][JA]-RlsGrp`

    Multi Episode:

    `The Series Title! (2010) - S01E01-E02-E03 - 001-002-003 - Episode Title [AMZN WEBDL-1080p v2][HDR][10bit][x264][DTS 5.1][FLAC][JA]-RlsGrp`

#### Series Folder Format

Adding year back in with tvdb id allows matching to be correct. Thanks salty.

```bash
{Series TitleTheYear} [tvdb-{TvdbId}]
```

RESULT:

`The Series Title! (2010) [tvdb-12345]`

#### Season Folder Format

The only one real option to use in my opinion.

```bash
Season {season:00}
```

RESULT:

`Season 01`

#### Multi-Episode Style

```bash
Scene
```

RESULTS:

![results](images/results.png)

??? success "example"

    ![!Example image of above](images/rpa-mediamanage.png)

---

## Quality Profile

We need to add HDTV-1080p into the same group as WEB-DL and WEBRip 1080p so that it will download correctly and not upgrade past HDTV-1080P if Preferred Words does not allow it.

![!rpa-quality-profile-group](images/rpa-quality-profile-group.png)

---

## Release Profiles

!!! important
    We're going to make use of 2 separate release profiles.

### First Release Profile

!!! note

    Check mark `Include Preferred when Renaming` and add `{Preferred Words}` to your renaming scheme else you could get a download loop issues!!!

#### Dual Audio

Add this to your Preferred with a score of [500]

```bash
/(dual[ ._-]?audio)/i
```

---

#### Multi-Audio

Add this to your Preferred with a score of [400]

```bash
/(multi[ ._-]?audio)/i
```

---

#### Optional (uncensored)

Add this to your Preferred with a score of [1000]/[-1000] if you want the uncensored version of anime. If you would like it to be priority then set to 1000. If you do not want uncensored set it to -1000.

```bash
/\b(uncensored)\b/i
```

---

??? success "example"
    ![!rpa-release-profile-1](images/rpa-release-profile-1.png)

---

### Second Release Profile

!!! warning
    DO NOT Check mark `Include Preferred when Renaming`

#### Must Not Contain

The reason most of these are added is due to their shitty quality or just in general are not as good as other stuff that is out there. This can be tweaked as needed. Let me know if you run into any issues here. You should be able to just copy paste these in one go.

!!! note

    If you would like Dub only release then remove the last 3 lines and do not add them.

```bash
/(CuaP|\[EMBER\]|-EMBER|PnPSubs|ICEBLUE|SLAX)/i,
/(Raws-Maji|\bKRP\b|M@nI|Kanjouteki|PuyaSubs)/i,
/\b(Beatrice|ohys|Kawaiika|neko)\b(?=[ ._-]?(raws)\b)/i,
/\bLowPower|Scryous)\b(?=[ ._-]?(raws)\b)/i,
/(eng[ ._-]dub|english[ ._-]dub|funimation)/i,
/(funi[ ._-]dub|\bfuni\b|\bdub\b|\bdubbed\b)/i,
/(Golumpa)/i
```

??? success "example"
    ![!rpa-must-not-contain.png](images/rpa-must-not-contain.png)

#### Preferred

!!! note

    2 lines in one grouping is just there for readability purposes but achieves the same result and score.

Add this to your Preferred with a score of [650]

BluRay-Remux / Dual-Audio / 1st Tier

```bash
/(deanzel|\bZR\b|Kametsu|\bCTR\b|\bSCY\b|\bShir\b)/i
```

```bash
/(\bMK\b|LostYears|BluDragon|KAWAiREMUX|ANThELIa)/i
```

---

Add this to your Preferred with a score of [600]

BluRay-Remux / Dual-Audio / 2nd Tier

```bash
/(iAHD|ANiHLS|MKVULTRA)/i
```

---

Add this to your Preferred with a score of [575]

BluRay / Dual-Audio / Special (Releases they have are in between below releases and remuxes above.)

```bash
/(\bE.N.D\b|\bE-N-D\b)/i
```

---

Add this to your Preferred with a score of [550]

BluRay / Dual-Audio

```bash
/(HAiKU|Exiled-Destiny|E-D|Koten_Gars|Hark0N)/i
```

```bash
/(\[GHOST\]|-GHOST|\[NPC\]|-NPC|\[Prof\]|-Prof)/i
```

---

Add this to your Preferred with a score of [500]

BluRay / Dual-Audio / Scene|P2P Groups

```bash
/\bDHD\b|RedBlade|TENEIGHTY|WaLMaRT)/i
```

---

Add this to your Preferred with a score of [450]

WebSource / Dual|Multi-Audio

```bash
/\b(DragsterPS)\b/i
```

---

Add this to your Preferred with a score of [400]

WebSource / Dual-Audio

```bash
/(xPearse|\[Prof\]|-Prof)/i
```

---

Add this to your Preferred with a score of [350]

DVD / Dual-Audio

```bash
/\b(A-L)\b/i
```

---

Add this to your Preferred with a score of [300]

Subs with multi such as German, French , Spanish, Italian, Russian, Arabic, Portuguese and English

```bash
/\b(Erai-raws)\b/i
```

---

Add this to your Preferred with a score of [250]

Subs

```bash
/(HorribleSubs|SubsPlease|SallySubs)/i
```

---

Add this to your Preferred with a score of [200]

Subs 2nd Tier

```bash
/(Mysteria|Asenshi|AkihitoSubs|\bASW\b)/i
```

---

Add this to your Preferred with a score of [150]

Subs 3rd Tier

```bash
/(Commie|\bGJM\b|kBaraka)/i
```

---

Add this to your Preferred with a score of [100]

FanSubs 1st Tier

```bash
/(\bCH\b|\bFFF\b|Licca|Soldado|SNSbu)/i
```

```bash
/(Coalgirls|BlurayDesuYo|DameDesuYo)/i
```

---

Add this to your Preferred with a score of [50]

FanSubs 2nd Tier

```bash
/(\[Orphan\]|-Orphan|\[Yabai\]|-Yabai|Nii-sama)/i
```

---

!!! note

    These are extra parameters that can help and are not needed but are nice to have.

Add this to your Preferred with a score of [10]

Adds Hi10|Hi10p into a match. Prefers this over some erroneous matches.

```bash
/(10.?bit|hi10p)/i
```

---

Add this to your Preferred with a score of [10]-[20]-[30] in order.

Adds version due to anime groups sometimes fixing issues with their releases.

```bash
/\b(v2)\b/i
```

```bash
/\b(v3)\b/i
```

```bash
/\b(v4)\b/i
```

---

!!! note

    This section can be controversial but most of these releases are micro/mini encodes or crappy quality. I personally add them into the do not want section but I will leave them here with negative values so you can still grab them if all else fails.

Add this to your Preferred with a score of [-1000]

Sub-Par Releases 1st Tier (Re-encoded to crap or really small filesizes) (Little less worse than the rest)

```bash
/(\[Judas\]|-Judas|\[Cleo\]|-Cleo)/i
```

```bash
/(AnimeRG|bonkai77|URANiME)/i
```

---

Add this to your Preferred with a score of [-1500]

Sub-Par Releases 2nd Tier (Re-encoded to crap or really small filesizes) (Seperated onto separate lines for readability)

```bash
/(\bDB\b|Man.K|phazer11|\[Pixel\]|-Pixel|\bHR\b)/i
```

```bash
/(\[Ranger\]|-Ranger|SpaceFish|DeadFish)/i
```

```bash
/(BakedFish|NoobSubs|NewbSubs|Anime Time)/i
```

```bash
/(AnimeKaizoku|Kaizoku|\bCBB\b|\[EDGE\]|-EDGE)/i
```

```bash
/(iPUNISHER|MiniTheatre|MiniFreeza|Mr.Deadpool)/i
```

```bash
/(NemDiggers|Project-gxs|youshikibi)/i
```

??? tip "If you would like the above to be in DO NOT WANT in an easy format here it is."

    ```bash
    /(\bDB\b|Man.K|phazer11|\[Pixel\]|-Pixel|\bHR\b)/i,
    /(\[Ranger\]|-Ranger|SpaceFish|DeadFish)/i,
    /(BakedFish|NoobSubs|NewbSubs|Anime Time)/i,
    /(AnimeKaizoku|Kaizoku|\bCBB\b|\[EDGE\]|-EDGE)/i,
    /(iPUNISHER|MiniTheatre|MiniFreeza|Mr.Deadpool)/i,
    /(NemDiggers|Project-gxs|youshikibi)/i
    ```

---

??? success "example"
    ![!rpa-release-profile-2.png](images/rpa-release-profile-2.png)

---

### Acknowledgements

Most of my information and knowledge came from:

- Kaiser (Guidance on anime groups)

- rg9400 (Guidance on regex fixes, anime groups and general knowledge share.)

- Drazzilb (Guidance on regex fixes, anime groups and general knowledge share.)

- [TRaSH](https://trash-guides.info/) (For allowing me to utilize his website for my guide and general knowledge share.)

Thanks a bunch!!