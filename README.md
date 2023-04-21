# Testing Privacy on Popular Browsers
## Project Goals
In the modern day, internet browsers are increasingly advertising their privacy-preserving features, with browsers such as Brave touting themselves as being ["on a mission to protect your privacy online."](https://brave.com/about/) Our project aims to investigate popular browsers' evasiveness against cross-site tracking. We also aim to answer how well some of these tools prevent a website from recognizing returning users to their site, how strong certain identifiers like cookies can be, and what the best combination of privacy and browser settings are to preserve a user's privacy.

## Methodology
### Evercookie
We first needed a way to measure a feature's privacy, which is a qualitative measure. We selected [Evercookie](https://github.com/samyk/evercookie), created by Samy Kamkar in 2010. Evercookie Evercookie's goal is ["to identify a client even after they've removed standard cookies" by "produc[ing] extremely persistent cookies in a browser"](https://samy.pl/evercookie) Evercookie is able to persist even after manually deleting cookies. It accomplishes this by storing the cookie using as many mechanisms as possible, and if it is able to recover the cookie from one mechanism, it restores the cookie to all other mechanisms that were previously deleted (if there are any).

#### Mechanisms
Evercookie (originally) supported 17 mechanisms. There are simpler mechanisms such as standard HTTP cookies (which Evercookie calls `cookieData`), and more sophisticated ones such as HTML5 IndexedDB (`idbData`). Some mechanisms have since been deprecated, such as Flash Local Shared Objects (`lsoData`), as most major browsers ended Flash support in 2021. A similar situation applies to the two Java mechanisms, as the Java Applet API [was deprecated in 2017](https://openjdk.org/jeps/289). We also encountered issues with mechanisms requiring a backend server, so we decided not to test those either. Lastly, the CSS history knocking mechanism is noted to be "network intensive," and crashed the browsers we tested, so we skipped this mechanism as well.

Below is a table (adapted from the [Evercookie Github](https://github.com/samyk/evercookie)) of the mechanisms used by Evercookie, and their relation to this project:
| Name | Long Name | Used | Reason |
| ---- | --------- | ---- | ------ |
| `cookieData` | Standard HTTP cookies | :heavy_check_mark: ||
| `lsoData` | Flash local shared objects | :x: | Deprecated |
| `slData` | Silverlight isolated storage | :x: | IE not tested |
| N/A | CSS history knocking | :x: | Buggy |
| `pngData` | Force-cached PNGs | :x: | Backend server required |
| `etagData` | HTTP ETags | :x: | Backend server required |
| `cacheData` | Web cache | :x: | Backend server required |
| N/A | HTTP Strict Transport Security (HSTS) pinning | :x: | Buggy |
| `windowData` | window.name caching | :heavy_check_mark: ||
| `userData` | IE userData storage | :x: | IE not tested |
| `sessionData` | HTML5 session storage | :heavy_check_mark: ||
| `localData` | HTML5 local storage | :heavy_check_mark: ||
| `globalData` | HTML5 global storage | :x: | Deprecated |
| `dbData` | HTML5 database storage | :heavy_check_mark: ||
| `idbData` | HTML5 IndexedDB | :heavy_check_mark: ||
| N/A | Java JNLP PersistenceService | :x: | Deprecated |
| N/A | Java CVE-2013-0422 exploit | :x: | Deprecated |

### Browsers and Tools Tested
TODO


### Website Endpoint Evaluator

### Reproducing

The steps to reproduce the study are as follows (**TODO: CAN YOU ADD SCREENSHOTS FOR EACH STEP OF THE LIST**):

1. Spin up Docker container and Nginx server
2. Open browser, turn on any privacy features to test
3. Visit website endpoint & create the Evercookie
4. Leave webpage, see what mechanisms persisted
5. Close browser, see what mechanisms persisted
6. Repeat for different browsers and features

From there, comparisons between browsers and privacy features can be made by looking at which mechanisms persisted in the different options versus which got properly removed.

## Literature Review

## Results

The whole dataset collected can be found at this [link](googlesheet). Below are some of the interesting trends we found:

### Vanilla Browsers
Most of the vanilla browsers, or browsers without any additional privacy feature or setting, performed functionally the same across the board: they failed to delete cookies on both page leave and browser close. An example of what mechanisms persisted for vanilla browsers can be found below by looking at Chrome vanilla browser:

PICTURE OF CHROME VANILLA TABLE

An interesting note here is that session and window mechanisms do get deleted on a page leave or on closing the browser. We suspect this is due to how these mechanisms are stored: through an open instance of a tab on the browser. Once the tab is closed, these mechanisms that store the cookie data get deleted.

While most vanilla browsers did not delete cookies on closing it, there is one exception: Tor Browser. Tor Browser was the only browser that, with default settings, deleted the Evercookie successfully through closing its browser. Upon further inspection, it was found that Tor Browser actually has a setting that allows the user to be able to choose whether or not cookies should be deleted on browser close.

PICTURE OF TOR BROWSER SETTING 

### Incognito
For incognito (besides Tor Browser since there was no incognito mode available), we found that the results of the browsers were the same across the board. In contrast to vanilla browsers, every browser in incognito mode was able to delete Evercookie on browser close. However, they all similarly failed to stop any mechanisms on page leave besides the session and window data mechanisms. Below are the mechanisms that persisted using Chrome incognito as an example:

PICTURE CHROME INCOGNITO TABLE

As you can see, the bottom row had no mechanisms that persisted, indicating that Evercookie was successfully deleted upon browser close. 

Interestingly enough, Tor Browser performed identical to Incognito Firefox. Upon further inspection, we found that Tor Browser is actually built on top of Firefox, so the performance similarities makes sense. In essense, we think Tor Browser can act as a "private" version of Firefox browser, alongside the benefits of using a Tor network to further increase a user's privacy.

### Privacy Extensions
In order to expand the project beyond just browsers, we decided to test common privacy extensions a user might add to their browser. We choose these extensions based on what was found to be popular on the Internet or the top search result in extension download stores.

#### The Extensions Tested TODO: FINISH LINKS
[Privacy Badger](link): a privacy extension that learns to ID and block trackers based on browser history.
[Cookie Remover](link): an extension that deletes cookies manually through a click of a button.
[Cookie Autodelete](link): an extension that deletes cookies automatically.
[Ghostery](link): a privacy extension that blocks common trackers.

#### The Results TODO: Screenshots?
**Privacy Badger**: We found that Privacy Badger does not block Evercookie, nor does it block any of its various persistence mechanisms. TODO: FIX??? -> We believe this occurs because Privacy Badger has to learn through browser history, but we were using it fresh without any training. It is possible that if Privacy Badger visited sites that also had Evercookie, it would learn to properly block it.

**Cookie Remover**: Cookie Remover was able to delete the localData and sessionData mechanisms.

**Cookie Autodelete**: In contrast to Cookie Remove, Cookie Autodelete failed to delete any of the persistence mechanisms at all.

**Ghostery**: We found that Ghostery prevented Evercookie from being set in the first place using its malicious script detection software. Thus, since Evercookie is never set, none of the persistence mechanisms work as well.

From the general results, our consensus for the privacy extensions is that Ghostery performed well ahead of the rest being that it was able to stop Evercookie in its entirety. However, further testing may be required to properly judge the rest of the extensions, especially in regards to Privacy Badger.
## General Conclusions

### Best way to stay private!
The safest way based on the results of the study to keep oneself private is to use a privacy extension like Ghostery that prevents a cookie from being set in the first place. 

However, extensions like Ghostery that rely on an arbitrary guess of whether the underlying JavaScript in the page is malicious could intefere with functionalities of non-malicious scripts, and thus affect the user's smooth experience in the process. As such, if a user does not want to deal with potential blocking of safe JavaScript, using an incognito browser OR Tor Browser should get rid of persistent cookies like Evercookie fairly easily. The only catch is that the user MUST close the browser or Evercookie will continue to persist using its many mechanisms.

It's also important to note this study, and Evercookie's use in general, is a study of the worse case scenario. Most cookies that are set on webpages are not going to be as persistent as Evercookie. However, we believe it's good to look at the severity of what might happen in the case that a person does want to develop a persistent cookie, and for far more malicious purposes in the future.

## Future Extensions
There are some interesting extensions that could be drawn from this study in the future:

For one, some of the working mechanisms of Evercookie did not work with the Docker setup. It is very possible that the missing mechanisms could have greatly swayed the results of this study, so it would be important in the future to setup environments where these mechanisms could be properly tested. Additionally in regards to the mechanisms, another extension might be to stress test each of the mechanisms individually, finding the risks and solutions to each. This could prove useful as privacy techniques could take into account each of these mechanisms and prevent them from persisting if solutions were found to each one.

An further extension could be made by creating an Evercookie that DOES track data. Samy Kamkar stated while creating Evercookie that it has no capability of tracking a user's information and that it only has relevance on the specific showcase website. Creating an Evercookie that did have the capability to track a user's information could prove to be useful as it would prove that it is possible to make an Evercookie that tracks information, drawing more attention to the potential privacy risks it creates.
