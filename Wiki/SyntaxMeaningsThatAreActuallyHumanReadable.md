### All up-to-date significant adblockers¹

#### Element removal (a.k.a. cosmetic rules, a.k.a. hiding rules)
* `##.`: Hides parts of a page, based on one or more `class` values in the F12 filetree (separated with full-stops).
* `##`: Hides parts of a page based on the element type, e.g. `a`, `li`, `button`, `iframe`, etc., usually highlighted in purple in the F12 filetree.
* `###`: Hides parts of a page based on the `id` value.
* `#@#.`/`#@#`/`#@##`: Whitelists parts of a page to make them load.
* `[href="text"]`: Finds page elements whose values in the F12 filetree console contains such a value. The value can be `href`, `id`, `class`, `type`, or numerous other things. Does not support RegEx.
* `[href^="text"]`: Finds page elements whose value *begins* with the text.
* `[href$="text"]`: Finds page elements whose value *ends* with the text.
* `[href*="text"]`: Finds page elements whose value contains the text anywhere within it.
* `[href="text" i]`: Save as above, except case-insensitive.
* `:not(.element)`: Finds page elements that doesn't contain a specified element or text string. Can be paired with other syntaxes à la `:not(:-abp-contains(Example text))`.
* `:-abp-contains(text)`: Finds page elements that contains such text within it.
* `:-abp-has(.element)`: Finds page elements that contains such an element within it.
* `:nth-of-type(n)` / `:last-of-type` / `:only-of-type`: Finds page elements that are at a specific numerical position in a set.
* `:before` / `:after`: Removes the pseudo-elements that belong to a page element.
* `>`: Creates chain criteria, in which a selected page element must have a specific element above it in the filetree.
* `+`: Blocks the element that is right below the criteria in the filetree. Example: `##.element + div` blocks that particular `div`.

##### Advanced examples:
* The first two `##` of an element entry, are not used for elements written after e.g. `>`, `+` or `:-abp-has`. In those cases, the `##` in `##element` gets removed, `##.class` becomes `.class`, and `###id` becomes `#id`.
* `##element.element2`: Hide something both based on its element (##element1) and `class` value (.element2). Note the placement/absence of fullstops.
* While they're based on the same `class` values, `##.element1` will match any `class` (sub-)value, whereas `##div[class="element1"]` and their modifiers are based on the *entire* `class` string in the F12 filetree.
* `##.` / `##` / `###` entries can either be *generic*, in which they have no domains in front of them; or domain-specific, where they have one or more domains in front of them, separated by commas. Only Nano and uBO support wildcard asterisks (`*`) in such domains, while other adblockers do not.

#### File blocking (a.k.a. blocking rules)
* `||`: Blocks resources from domains or parts thereof from being loaded. For non-domain-specific resources, no pre-emption is needed at all.
* `@@`: Whitelists resources from specific URLs to make them load.
* `^`: Wildcard for anything that isn't alphanumerical or "_-.%" . Often used to cover both slash ( / ) and non-slash domain name endings at the same time.
* `$third-party`: Ensures that resources from a domain are only blocked if you're not visiting the domain itself.
* `$~third-party`: Ensures that resources from a domain are only blocked if you're visiting the domain itself.
* `$domain=`: Ensures that resources from a domain are only blocked if you're visiting a specified website. Multiple domains are separated with `|` (Vertical line) and not commas.
* `@@||` + `$generichide`: Prevents all non-domain-specific (a.k.a. generic) hiding entries from working on a website. On Nano/uBO it prevents *all* non-domain-specific entries from working.
* `@@||` + `$specifichide`: Prevents all domain-specific hiding entries from working on a website. On Nano/uBO it seems to prevent *all* domain-specific entries from working.
* `@@||` + `$elemhide`: Combines `$generichide` and `$specifichide`. Also completely breaks the element picker in Nano/uBO on that site as of the 14th of December 2019.
* `$script`: Blocks resources from domains or parts thereof from being loaded, but only if it's a script, e.g. a JavaScript runtime.
* `$csp`: Inserts additional *Content Security Policies* into the page.
* `$xmlhttprequest` / `$websocket` / `$dom`: Prevents such resources from being downloaded through the titular JavaScript APIs.
* `$popup` / `$image` / `$object` / `$font` / `$other`: These ones should hopefully be self-explanatory (Give me a heads-up in an issue report if it isn't).
* `$match-case`: Makes the criteria case-sensitive.
* `|text`: Matches URLs that *begin* with the text.
* `text|`: Matches URLs that *end* with the text.

#### Universal
* `! ` / `# `: Marks the start of a comment that shall not be interpreted as an entry.
* `~`: Means that an entry does *not* apply to a specific domain.
* `/\/\/\/` and similar: Text detections in RegEx format.
* `[Adblock Plus n.n]`: Used by Adblock Plus, AdBlock, and forks of them to determine if they should load the filterlist. Number is the intended minimum ABP version. `2.0` and `1.1` are most common; `3.1` and higher is on the rise and can be used to block support for old or low-quality forks. This has no effect on uBO or its forks.
* `! Title:` Specifies the intended name of the list. Required to make the name automatically show up in the settings of most adblockers, instead of the URL or of manual text input.
* `! Version:` The version number/alphanumeric of the list. Unofficially used to distinguish which version of a list a user is using. Used administratively by Adblock Plus' list report system (which requires a number-only version value). Has significant overlap with `! Last modified`.
* `! Expires:`: Determines the timespan between each automated sync attempt with the list's source. Values are given in "n day/days". ABP also supports "hour/hours".

### Nano Adblocker, uBlock Origin, Adblock Plus and AdBlock only:
* `:scope`: Used alongside `:-abp-has` to make it only find elements whose criteria match their immediate subelements.

### Nano Adblocker, uBlock Origin and AdGuard only:
#### Hiding
* `:style`: Changes the CSS values of an element, in much the same way as what userstyle extensions like Stylish would've done.
* `{ }`: Same as above.
* `:has-text`: Same as `:-abp-contains`.
* `:has`: Same as `:-abp-has`.
* `!#if`: Specifies that a section of entries only applies to specific platforms or extensions. Closed out by `!#endif`.
* `:matches-css`: Looks for page elements whose existing native (i.e. non-inherited) CSS values match those of the criteria.
* `:matches-css-before`: Same as above, but looks for CSS values in its pseudo-elements instead.
* `$redirect`: Redirects resources to a neutered version that has been embedded in those extensions. Possible options are listed in [this file](https://github.com/gorhill/uBlock/blob/master/src/js/redirect-engine.js) (AdGuard has a [slightly smaller selection](https://github.com/AdguardTeam/AdguardBrowserExtension/blob/master/Extension/lib/filter/rules/scriptlets/redirects.yml)).
#### Blocking
* `$badfilter`: Deactivates a resource-blocking entry, even if it is present in another list.
* `$important`: Makes a resource-blocking entry take precedence over another whitelisting entry.

### Nano Adblocker and uBlock Origin only:
#### Hiding
* `!#include`: Embeds another filterlist that is hosted on the same domain (with numerous restrictions).
* `##+js` (prev. `##script:inject`): Invokes a script that is embedded in those extensions, and usually using the script to modify a value on the site. Possible options are listed in [this file](https://github.com/gorhill/uBlock/blob/master/assets/resources/scriptlets.js) (The top strings of each paragraph).
* `:xpath`: An entry written with the very advanced Xpath syntax.
* `##^`: Blocks resources before they've even been loaded, based on their values in *View source* instead of their F12 ones.
* `:nth-ancestor`: Looks for elements that are a certain amount of indentations (i.e. filetree floors) above the criteria in the F12 filetree.
#### Blocking
* `127.0.0.1` / `0.0.0.0` / `::1` / `0` / `::`: Used by "*hosts*" system files to signify that network requests to such a domain shall be redirected to a local-only IP address, thus preventing it from loading. Nano and uBO treats it the same as `||`. It only supports whole domains; using `/` or any other non-alphanumeric-or-period characters is not accepted.
* `||` + `$document`: Guarantees a danger warning when loading a page, which is not 110% guaranteed otherwise.
* `$3p`: Same as `$third-party`.
* `$1p` / `$first-party`: Same as `$~third-party`.
* `$xhr`: Same as `$xmlhttprequest`.
* `$all`: Officially combines all other non-party `$` values. In practice it combines the use of no `$` values at all + `$popup`.

### Adblock Plus and AdBlock only:

* `! Redirect: `: Tells the adblocker to look for list updates from a new URL from that point on.
* `#?#`: Required to make entries with `:-abp-has`, `:-abp-contains` and `:-abp-properties` work in those particular extensions, and to make `:style` entries not break the list extremely heavily.
* `@@||` + `$document`: Turns off adblocking entirely while on that domain.
* `@@||` + `$genericblock`: Prevents all non-domain-specific blocking entries from working on a website.
* `:-abp-properties`: A highly modified version of `:matches-css[-before]`, with some syntax differences. Can also select text encodings (à la Base64) and a few other non-CSS traits.
* `$rewrite=abp-resource:`: Similar to `$redirect`, but with a rather different selection of neutered files. Possible options are listed on [this help page](https://help.eyeo.com/adblockplus/how-to-write-filters#rewrite).
* `#$#`: Similar to, but incompatible with, `##+js`. Possible options are listed in [this file](https://gitlab.com/eyeo/adblockplus/adblockpluscore/blob/next/lib/content/snippets.js) (text-search `@alias`).

### AdGuard only:

* `#%#//scriptlet`: Similar to, but only partially compatible with, `##+js` and ABP's `#$#`. Possible options are listed in [this file](https://github.com/AdguardTeam/AdguardBrowserExtension/blob/master/Extension/lib/filter/rules/scriptlets/scriptlets.js) (text-search ".names").
* `#%#AG_`: A few extra scriptlets for whom documentation appears to be non-existent.
* `#%#` without `//scriptlet`: Appears to insert JavaScript code that is written into the list, as opposed to from an embedded file.
* `$empty`: Results in a fake empty page being loaded, instead of an error page.
* `:properties`: Claims to be similar to `:-abp-properties`, but is incompatible with it.
* `$$script`: Uses very advanced criteria to block scripts that meet them.
* `$cookie`: Blocks cookies.
* `$cookie=`: Blocks cookies with specific names.
* `$cookie=` + `maxAge`: Changes the cookie to have an expiration time in seconds.
* `$cookie=` + `same-site`: Changes the cookie to use the "Lax" mode of `samesite` known from the `Set-Cookie` browser HTTP response system.

### AdGuard for [Windows/Mac/Android] only:

* `! Description:`: Shows a description of the list's purpose, when the question mark next to the list in the AdGuard settings is hovered over. That being said, a description is convenient for users of all adblockers, if they're willing to look up a list's raw content.
* `$network`: When applied to an IP address, it blocks all incoming requests from it, and not just when it's typed into a browser address bar. Individual ports can be specified. IPv6 addresses must be surrounded by square brackets. Can very easily break legitimate sites as collateral damage, and should be used very sparingly.
* `@@` + `$jsinject`: Prevents `#%#` entries from working on that site.
* `@@` + `$extensions`: Prevents AdGuard userscripts from working on that site.
* `@@` + `$content`: Prevents `$$script` entries from working on that site.
* `@@` + `$stealth`: Turns off Stealth Mode on that site.
* `$mp4`: Seems to be equivalent to `$redirect=noopmp4`, but does not require any AdGuard trust rights.
* `$replace`: Changes the text of text elements on a site. Supports and requires use of RegEx. Requires ridiculous amounts of trust rights and cannot be used in web-hosted lists.

# Other particularly important usage notes

* To make the text detection for `:-abp-contains` and `:has-text` case-insensitive, wrap the paranthesised text into `(/Example text/i)`.
* The `"` in `[href="text"]` is optional, but *only* if the criteria text is only a single word.
* `:style` and `{ }` does not allow changing `background-image` into a URL value.
* It is claimed in [this comment](https://github.com/DandelionSprout/adfilt/issues/7#issuecomment-481978609) that Safari does not properly accept the use of `$third-party`.
* Amazingly, using `! Redirect: ` in the intended target link's list, will cause an infinite loop that prevents the list from being loaded.
* In Opera, the F12 filetree is not actually opened with F12 by default, but instead with Ctrl+Shift+I (Capital İ).

¹ = Includes Nano Adblocker, uBlock Origin ≥1.14.0, AdGuard, AdNauseum, Adblock Plus version ≥3.1, and AdBlock. It does **not** include AdGuard Home, Brave Browser, Slimjet, uBlock non-Origin, Tracking Protection List, or Blokada, whose syntax supports are considerably inferior to the above list.
