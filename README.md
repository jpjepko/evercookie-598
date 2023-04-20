# Testing Privacy on Popular Browsers
## Project Goals
In the modern day, internet browsers are increasingly advertising their privacy-preserving features, with browsers such as Brave touting themselves as being ["on a mission to protect your privacy online."](https://brave.com/about/) Our project aims to investigate popular browsers' evasiveness against cross-site tracking. We also aim to answer how well some of these tools prevent a website from recognizing returning users to their site, how strong certain identifiers like cookies can be, and what the best combination of privacy and browser settings are to preserve a user's privacy.

## Methodology
### Evercookie
We first needed a way to measure a feature's privacy, which is a qualitative measure. We selected [Evercookie](https://github.com/samyk/evercookie), created by Samy Kamkar in 2010. Evercookie's goal is ["to identify a client even after they've removed standard cookies."](https://samy.pl/evercookie) Evercookie is able to persist even after manually deleting cookies. It accomplishes this by storing the cookie using as many mechanisms as possible, and if it is able to recover the cookie from one mechanism, it restores the cookie to all other mechanisms that were previously deleted (if there are any).

#### Mechanisms
Evercookie (originally) supported 17 mechanisms. There are simpler mechanisms such as standard HTTP cookies (which Evercookie calls `cookieData`), and more sophisticated ones such as HTML5 IndexedDB (`idbData`). Some mechanisms have since been deprecated, such as Flash Local Shared Objects (`lsoData`), as most major browsers ended Flash support in 2021. A similar situation applies to the two Java mechanisms, as the Java Applet API [was deprecated in 2017](https://openjdk.org/jeps/289). We also encountered issues with mechanisms requiring a backend server, so we decided not to test those either. Lastly, the CSS history knocking mechanism is noted to be "network intensive," and crashed the browsers we tested, so we skipped this mechanism as well.

Below is a table (adapted from the [Evercookie Github](https://github.com/samyk/evercookie)) of the mechanisms used by Evercookie, and their relation to this project:
| Name | Long Name | Used | Reason |
| ---- | --------- | ---- | ------ |
| `cookieData` | Standard HTTP cookies | :heavy_check_mark: ||
| `lsoData` | Flash local shared objects | :x: | Flash deprecated |
| `slData` | Silverlight isolated storage | :x: | IE not tested |
| TODO | CSS history knocking | :x: | Buggy |
| `etagData` | HTTP ETags | :x: | Backend server required |
| `cacheData` | Web cache | :x: | Backend server required |
| TODO | HTTP Strict Transport Security (HSTS) pinning | :x: | TODO |
| `windowData` | window.name caching | :heavy_check_mark: ||
| `userData` | IE userData storage | :x: | IE not tested |
| TODO | TODO | TODO | TODO: do the rest of them |

### Browsers and Tools Tested
TODO

### Website Endpoint Evaluator
TODO

### Reproducing
TODO

## Literature Review
TODO

## Results
TODO

### Vanilla Browsers
TODO

### Incognito
TODO

### Privacy Extensions
TODO

## General Conclusions
TODO

## Extensions
TODO
