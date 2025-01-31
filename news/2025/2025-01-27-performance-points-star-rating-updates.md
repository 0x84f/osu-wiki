---
layout: post
title: Performance Points & Star Rating Updates
date: 2025-01-27 10:00:00 +0000
---

We are back with another set of changes!

![](/wiki/shared/news/banners/pp-sr-pippi.jpg)

This time around the osu!, osu!taiko and osu!catch game modes are receiving updates to difficulty and performance calculations. This news post will discuss what has changed in a format that should be understandable to you, the player!

All changes made in this round are intended to keep the general understanding of per-score PP values the same. This means that if a score's (or beatmap's max) PP value has decreased, it is due to that beatmap or score being overweighted until now. As a result, individual users may see a large jump in their PP, in either direction.

If you find yourself scratching your head when reading, consider consulting the [performance points (PP)](/wiki/Performance_points) wiki article to gain a greater understanding of the topics.

## Release schedule

### ⏳ Star rating recalculation (24 - 48 hours)

To begin, we will be recalculating the star rating of every beatmap across every game mode.

This will be done live, so as the new SR values are applied to beatmaps, new scores arriving will get calculated using them. As a result, the PP you achieve may slightly deviate from expectations. Any PP values which don't match will be fixed in the following steps.

Importantly, **the old PP algorithm will still be used, so the PP values achieved will not be final**.

### ⏳ Freeze global rank graph updates

We are about to reprocess every score in existence. Before doing this, we need to disable user rank updates. This means your current global / country rank (and graph) will be frozen in time until we're done reprocessing things.

That said, if you play, your **total PP** will still immediately be updated.

If we didn't do this, users would see their rank jump all over the place, as we have no way of ensuring every score PP and user total PP are updated all at once.

### ⏳ Reprocess performance values of all scores (3 - 10 days)

We now need to reprocess over 3 billion scores (this means **any scores you can currently see on the website** will get a new PP value, including scores set on osu!(stable) and osu!(lazer)). This is the most time-consuming part of the deploy process.

During this period, scores in "best performance" may look to be out of order or not be visible at all. Also, **your global rank might jump around** momentarily as not all users will be recalculated at once.

### ⏳ Reprocess total PP values for all users (12 hours)

### ⏳ Re-enabling of global rank history updates

Rank history graphs will be enabled and updated again. At this point, all users' global leaderboard ranks will be stable going forward.

### ⏳ Reindexing (2 - 3 days)

This will fix scores being out of order on profiles under "best performance", and in some rare cases not being displayed at all.

## osu!

### Acute angle bonus rework

The acute angle bonus is a bonus inside of aim calculations for BPMs greater than 300. It is used to reward fast, difficult aim patterns.

This bonus should now be more fair towards difficult patterns, while some overweighted patterns such as repeated angles are now awarded less of this bonus, thanks to a [set](https://github.com/ppy/osu/pull/30902) [of](https://github.com/ppy/osu/pull/31245) [various](https://github.com/ppy/osu/pull/31320) [changes](https://github.com/ppy/osu/pull/31566) proposed by [StanR](https://osu.ppy.sh/users/7217455) (with some contributions by [tsunyoku](https://osu.ppy.sh/users/11315329)).

There was also a change to uncap velocity constraints that used to exist on the bonus allowing for some very hard *acute*-angled jumps in high BPM beatmaps to receive a greater buff.

As part of this change, the wiggle bonus which used to be part of the acute angle bonus is now applied regardless of BPM — beatmaps with difficult wiggle-like patterns below 300 BPM may receive a (slight) new buff:

![](/wiki/shared/news/2025-01-27-performance-points-star-rating-updates/wiggle-graph.png)

Some examples of beatmaps buffed by these changes:

- [DJ TOTTO - Crystalia [Meal's Ultra]](https://osu.ppy.sh/beatmapsets/691220#osu/1475722) with DT: 11.19* --> 11.38*
- [Light - Anoneanoneanoneanoneanoneanoneanoneanone Kouji ga Okureteruno [Anone]](https://osu.ppy.sh/beatmapsets/822716#osu/1724319) with DT: 10.62* --> 11.22*
- [nameless - Milk Crown on Sonnetica [Milked]](https://osu.ppy.sh/beatmapsets/550414#osu/1166087) with DT: 10.73* --> 11.17*

Some examples of beatmaps nerfed by these changes:

- [toby fox - Song That Might Play When You Fight Sans [Genocide]](https://osu.ppy.sh/beatmapsets/2252729#osu/4791387) with DT: 10.09* --> 9.82*
- [Middle Kids - R U 4 Me? (Cut Ver.) [Together Forever]](https://osu.ppy.sh/beatmapsets/2299895#osu/4914437) with DT: 11.13* --> 10.95*
- [Yousei Teikoku - Zetsubou plantation (Cut Ver.) [PikA's Extra]](https://osu.ppy.sh/beatmapsets/2250663#osu/4793476) with DT: 10.46* --> 10.22*

### Slider drop penalty fixes

Aim calculations reward beatmaps for having difficult-to-follow slider patterns, with a penalty for dropped slider ends to ensure that plays only receive their full buff if the sliders were hit correctly.

A [change](https://github.com/ppy/osu/pull/31055) proposed by [StanR](https://osu.ppy.sh/users/7217455) has been created in order to ensure this penalty is applied correctly, and uses a more concrete estimation of the difficult sliders in a beatmap.

Prior to this change, the penalty applied to aim PP for missed slider ends was unintentionally nerfing much less than it was supposed to. This meant beatmaps with difficult sliders could be abused for easy PP by missing a lot of slider ends. This has been corrected, which causes large nerfs for a lot of scores set on beatmaps such as [t+pazolite - Oshama Scramble! (IOException Edit) [Special]](https://osu.ppy.sh/beatmapsets/1376308#osu/2844649).

Furthermore, the game would previously assume that 10% of a beatmap's sliders are difficult, but that is no longer the case due to the aforementioned new estimation of difficult sliders. This makes the slider drop penalty fairer across the board.

### Speed PP punishment for scores with high tapping deviation

A [change](https://github.com/ppy/osu/pull/30907) proposed by [Givikap120](https://osu.ppy.sh/users/10560705) has been created in order to nerf speed PP on scores with a very high tapping deviation.

Tapping deviation is a statistic similar to unstable rate created by [Frostium](https://osu.ppy.sh/users/8202998), combining a score's hit statistics, OD and amount of difficult "speed notes" (high-strain notes relevant to speed PP) in a beatmap.

This deviation is used to punish speed PP if the deviation is deemed "too high" for a beatmap's difficulty and is assumed to have been tapped improperly. This primarily addresses scores using the "rake tapping" technique while also nerfing other forms of improper tapping.

This nerf works by deciding a cut-off point for speed PP, scaled by the deviation, where anything above it is considered to be tapped improperly — scores which go above this cut-off point are then re-adjusted based on their deviation to give a more representative value of their tapping difficulty.

The nerf has a threshold for deviation that must be reached before it nerfs anything at all in order to ensure the majority of scores which were tapped correctly are either not nerfed or nerfed insignificantly.

As a result of this change, the old penalty applied in speed PP for 50s has now been removed as this proposal better serves its purpose. You may see noticable buffs on plays with a lot of 50s if they're not deemed to be tapped improperly.

### Minor changes

- A [set](https://github.com/ppy/osu/pull/31456) of [rebalances](https://github.com/ppy/osu/pull/31515) proposed by [StanR](https://osu.ppy.sh/users/7217455) and [tsunyoku](https://osu.ppy.sh/users/11315329) in order to align final values with community expectations
- A [fix](https://github.com/ppy/osu/pull/31525) proposed by [molneya](https://osu.ppy.sh/users/8945180) to ensure that the time between spinners are correctly accounted for in Flashlight calculations
- A [fix](https://github.com/ppy/osu/pull/31447) proposed by [StanR](https://osu.ppy.sh/users/7217455) to ensure ODs below 0 cannot increase PP
- A [fix](https://github.com/ppy/osu/pull/30544) proposed by [Finadoggie](https://osu.ppy.sh/users/14182048) to ensure estimated slider drops cannot go below zero
- A [fix](https://github.com/ppy/osu/pull/30618) proposed by [Natelytle](https://osu.ppy.sh/users/17607667) to ensure osu!(lazer)'s PP counter does not display `NaN` while star rating is zero
- A [set](https://github.com/ppy/osu/pull/30534) [of](https://github.com/ppy/osu/pull/30536) [refactors](https://github.com/ppy/osu/pull/31520) proposed by [StanR](https://osu.ppy.sh/users/7217455), [ltca](https://osu.ppy.sh/users/11475208) and [Natelytle](https://osu.ppy.sh/users/17607667) to aid with development
- A [change](https://github.com/ppy/osu/pull/31449) proposed by [StanR](https://osu.ppy.sh/users/7217455) to simplify the angle bonus formulas in aim calculations
- A [change](https://github.com/ppy/osu/pull/21211) proposed by [tsunyoku](https://osu.ppy.sh/users/11315329) to implement basic difficulty and performance calculations for the Autopilot mod

## osu!taiko

In order to aid understanding of the changes to osu!taiko, these are the skills in difficulty calculation which will be referenced throughout:

- **Stamina**: the speed at which you hit notes, based on an assumed finger count of 2 per colour
- **Colour**: the frequency or variability of which the beatmap changes between a don or kat
- **Rhythm**: the complexity of the beatmap's rhythm in relation to notes' independent rhythm ratios

Throughout these changes, we will be using the following terminology:

- **Monos** refer to repeated notes of the same colour
- **Intervals** refer to the snap interval of notes (such as 1/1, 1/4 and 1/8)

### osu!taiko committee

Since the last performance point post, there are some new faces in the osu!taiko pp committee:

- [YaniFR](https://osu.ppy.sh/users/11260982)
- [BabySnakes](https://osu.ppy.sh/users/4669728)
- [rloseise](https://osu.ppy.sh/users/6793778)

### Rhythm rewrite

A [set](https://github.com/ppy/osu/pull/31284) [of](https://github.com/ppy/osu/pull/31339) [changes](https://github.com/ppy/osu/pull/31573) proposed by [ltca](https://osu.ppy.sh/users/11475208) (with contributions from [rloseise](https://osu.ppy.sh/users/6793778) and [YaniFR](https://osu.ppy.sh/users/11260982)) has been created in order to rewrite the rhythm skill to address shortcomings of the previous implementation.

The new rhythm skill works by creating ratios to assess difficulty. These ratios are dictated by the time between note changes — simple rhythms such as 1/2 will receive less of a bonus than more complicated rhythms such as 1/6 and 1/8, especially when frequently placed. In order to ensure this does not overly award small sections of difficulty, the frequency of these ratios are assessed to decide how difficult the ratio changes are.

To handle patterns where the time between notes doesn't change much, we group evenly spaced notes into a pattern. These are often the "streams" you see in fast beatmaps. If the timing between notes is consistent, those notes go into the same group. 

When the timing changes, the system shifts notes into the group with the shorter interval:

- In **●● ●●●**, the third note joins the later group (**●●●**) because it fits better with the shorter timing.
- In **●●● ●●**, the third note sticks with the earlier group (**●●●**) for the same reason.

This prevents evenly spaced notes from being unfairly treated as difficult, whilst also helping to measure how rhythm and colour difficulty interact with eachother.

The new rhythm skill also uses hit windows to decide if the current pattern can be hit without requiring any adjustments to your timing. If a pattern does not require any timing adjustments, then rhythm difficulty is not awarded and those patterns are grouped together:

- **● ● ● ● ●●●** can be played evenly as **● ● ● ● ● ● ●**, so it has no rhythm difficulty.
- **● ● ● ● ●●●●●●** can't be played the same way because the later notes fall outside the hit window, so the rhythm still counts as challenging.

Repeating patterns, such as triples or 4 note patterns, receive a lower bonus in this new system due to their constant rhythm and predictability.

### Reading skill

A [set](https://github.com/ppy/osu/pull/31208) [of](https://github.com/ppy/osu/pull/31510) [changes](https://github.com/ppy/osu/pull/31512) proposed by [ltca](https://osu.ppy.sh/users/11475208) (with contributions from [rloseise](https://osu.ppy.sh/users/6793778)) has been created in order to add a new measure of difficulty assessment - reading. This new skill aims to award added bonuses depending on the difficulty of reading patterns.

The reading skill assesses the **effective BPM** (calculated as the base BPM of the beatmap multiplied by the per-object slider velocity multiplier) of each object. Drum rolls and swells are not required to be hit, so these object types are exempt from any reading bonuses.

The new skill also uses object density which evaluates how close a note appears to the previous one using its effective BPM and the time between the 2 notes. This is used to penalise dense high-velocity notes that are generally easier to read.

The combination of effective BPM and density evaluation ensures that fast, spaced-out sections feel accurately represented in the difficulty system while avoiding inflation for simpler sections at high BPM:

![](/wiki/shared/news/2025-01-27-performance-points-star-rating-updates/reading-graph.png)

As part of this change, fixed HR buffs in performance calculations were removed to allow difficulty calculation to handle the difficulty increase.

### Ratio considerations in colour

A [change](https://github.com/ppy/osu/pull/31285) proposed by [ltca](https://osu.ppy.sh/users/11475208) has been created in order to consider the ratios of patterns when awarding colour difficulty.

The colour skill now applies a penalty based on the number of consecutive *consistent* intervals, meaning stream-like patterns with a lack of rhythmic and colour difficulty are penalised appropriately. This also means that diverse beatmaps with less predictable colour changes receive an added bonus.

Some examples of beatmaps buffed by this change:

- [Babuchan - 13 stairs [Unneeded]](https://osu.ppy.sh/beatmapsets/1093671#taiko/3878430): 9.43* --> 10.00*
- [GTS Sound Team - <</nttld.:beings>> \~Truth in Uncertainty\~ [<</eclectic.:genesis>>]](https://osu.ppy.sh/beatmapsets/1859338#taiko/3822143): 9.10* --> 9.79*
- [Ludicin - Fallen Symphony [The Symphony of the Fallen Angel]](https://osu.ppy.sh/beatmapsets/1957129#taiko/4054390): 8.90* --> 9.60*

Some examples of beatmaps nerfed by this change:

- [Null Specification - Aletheia (fake lover, fake summer) [fake promise]](https://osu.ppy.sh/beatmapsets/1895850#taiko/3907074): 7.47* --> 6.82*
- [DJ SHARPNEL - Pacific Girls [Divine Orchestra]](https://osu.ppy.sh/beatmapsets/2176923#taiko/4597047): 7.37* --> 6.68*
- [USAO - Try Again [Inner Oni]](https://osu.ppy.sh/beatmapsets/1875585#taiko/3859766) with DT: 7.61* --> 6.83*

### Stamina improvements

A [change](https://github.com/ppy/osu/pull/31337) proposed by [ltca](https://osu.ppy.sh/users/11475208) has been created in order to better factor speed into stamina calculations:

- Monos now have an additional buff to acknowledge the stamina required to keep up with rapid single-colour patterns, especially at higher BPMs
- Stamina now considers the density and BPM of consecutive objects, awarding extra bonuses for faster patterns

### Convert changes

Star rating for convert beatmaps have been improved, thanks to a [set](https://github.com/ppy/osu/pull/31196) of [changes](https://github.com/ppy/osu/pull/31546) proposed by [ltca](https://osu.ppy.sh/users/11475208). There is no longer any blanket star rating nerf for converts, because they're instead now more properly addressed in various areas:

- The accuracy curve for monos is harsher on lower accuracies
- Difficult converts usually increase the amount of available fingers due to techniques such as "TL tapping", thus stamina difficulty is decreased proportionally to match this added availability
- The previously mentioned stamina buff to monos is disabled for converts to address mapping techniques not present in osu!taiko beatmapsets

### Minor changes

- A [set](https://github.com/ppy/osu/pull/31338) of [rebalances](https://github.com/ppy/osu/pull/31556) proposed by [ltca](https://osu.ppy.sh/users/11475208) in order to align final values with community expectations
- A [change](https://github.com/ppy/osu/pull/31195) proposed by [ltca](https://osu.ppy.sh/users/11475208) to weight accuracy better on lower ODs
- A [change](https://github.com/ppy/osu/pull/31499) proposed by [Natelytle](https://osu.ppy.sh/users/17607667) to remove the lower cap on accuracy calculations, providing a larger nerf to lower accuracies
- A [change](https://github.com/ppy/osu/pull/30591) proposed by [ltca](https://osu.ppy.sh/users/11475208) to implement basic difficulty calculations for the Relax mod
- A [change](https://github.com/ppy/osu/pull/31067) proposed by [YaniFR](https://osu.ppy.sh/users/11260982) to scale star rating on lower difficulties more harshly
- A [fix](https://github.com/ppy/osu/pull/31579) proposed by [tsunyoku](https://osu.ppy.sh/users/11315329) to ensure rhythm uses the correct hit windows
- A [refactor](https://github.com/ppy/osu/pull/31191) proposed by [ltca](https://osu.ppy.sh/users/11475208) to aid with development

## osu!catch

## Buzz slider fixes

A [change](https://github.com/ppy/osu/pull/31126) proposed by [bastoo0](https://osu.ppy.sh/users/4864877) has been created in order to ensure buzz sliders are treated correctly.

A buzz slider is a back-and-forth horizontal slider that has a width smaller than the size of the catcher, but still large enough to be counted as a movement. This resulted in difficulty calculations interpreting the pattern as a very fast movement instead of a stand-still pattern. Back-and-forth movements caused by buzz sliders are now detected and no longer award excessive difficulty. This most notably affects beatmaps such as [100 gecs - hand crushed by a mullet [g3X_x_Xtr^@]](https://osu.ppy.sh/beatmapsets/1253992#fruits/3208604).

---

A huge thanks to the contributors of these changes as well as the community of people who helped by providing their feedback. If you'd like to learn more about the development of performance points, you may want to take a look in the `#difficulty-osu` channel of the [osu! Discord server](https://discord.gg/ppy), or even join the [Performance Points Discord server](https://discord.gg/aqPCnXu) dedicated to developing and discussing performance points.

—the performance point committees
