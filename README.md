# Using Pagerank to create a search engine for lawfareblog.com

## Task 1: the power method

I implemented the `WebGraph.power_method` function in` pagerank.py` to compute the pagerank vector.

**Part 1:**
I first ran the programon the small.csv.gz graph (example graph from the *Deeper Inside Pagerank* paper. I got the following output for my implementation: 
```$ python3 pagerank.py --data=./small.csv.gz --verbose
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=6.0257e-01 url=4
INFO:root:rank=1 pagerank=4.6414e-01 url=6
INFO:root:rank=2 pagerank=3.4544e-01 url=5
INFO:root:rank=3 pagerank=1.9498e-01 url=2
INFO:root:rank=4 pagerank=9.9210e-02 url=3
INFO:root:rank=5 pagerank=8.9347e-02 url=1
```

**Part 2:**

I then used the `search_query` option, which takes in a string as a paramter. I used the strings 'corona', 'trump', and 'iran', respectively.

```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='corona'
INFO:root:rank=0 pagerank=4.5861e-03 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=1 pagerank=4.0460e-03 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
INFO:root:rank=2 pagerank=2.6116e-03 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=3 pagerank=2.5390e-03 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=4 pagerank=2.3557e-03 url=www.lawfareblog.com/israeli-emergency-regulations-location-tracking-coronavirus-carriers
INFO:root:rank=5 pagerank=2.2895e-03 url=www.lawfareblog.com/why-congress-conducting-business-usual-face-coronavirus
INFO:root:rank=6 pagerank=2.2727e-03 url=www.lawfareblog.com/livestream-house-oversight-committee-holds-hearing-government-coronavirus-response
INFO:root:rank=7 pagerank=2.2520e-03 url=www.lawfareblog.com/congressional-homeland-security-committees-seek-ways-support-state-federal-responses-coronavirus
INFO:root:rank=8 pagerank=2.1878e-03 url=www.lawfareblog.com/paper-hearing-experts-debate-digital-contact-tracing-and-coronavirus-privacy-concerns
INFO:root:rank=9 pagerank=2.0339e-03 url=www.lawfareblog.com/cyberlaw-podcast-how-israel-fighting-coronavirus

$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='trump'
INFO:root:rank=0 pagerank=6.6243e-02 url=www.lawfareblog.com/donald-trump-and-politically-weaponized-executive-branch
INFO:root:rank=1 pagerank=6.0194e-02 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=2 pagerank=3.4969e-02 url=www.lawfareblog.com/trump-administrations-worrying-new-policy-israeli-settlements
INFO:root:rank=3 pagerank=3.2193e-02 url=www.lawfareblog.com/document-trump-revokes-obama-executive-order-counterterrorism-strike-casualty-reporting
INFO:root:rank=4 pagerank=3.0971e-02 url=www.lawfareblog.com/dc-circuit-overrules-district-courts-due-process-ruling-qasim-v-trump
INFO:root:rank=5 pagerank=2.8460e-02 url=www.lawfareblog.com/how-trumps-approach-middle-east-ignores-past-future-and-human-condition
INFO:root:rank=6 pagerank=2.5252e-02 url=www.lawfareblog.com/why-trump-cant-buy-greenland
INFO:root:rank=7 pagerank=2.2457e-02 url=www.lawfareblog.com/oral-argument-summary-qassim-v-trump
INFO:root:rank=8 pagerank=2.1462e-02 url=www.lawfareblog.com/dc-circuit-court-denies-trump-rehearing-mazars-case
INFO:root:rank=9 pagerank=2.1103e-02 url=www.lawfareblog.com/second-circuit-rules-mazars-must-hand-over-trump-tax-returns-new-york-prosecutors

$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='iran'
INFO:root:rank=0 pagerank=6.6131e-02 url=www.lawfareblog.com/praise-presidents-iran-tweets
INFO:root:rank=1 pagerank=2.9199e-02 url=www.lawfareblog.com/how-us-iran-tensions-could-disrupt-iraqs-fragile-peace
INFO:root:rank=2 pagerank=1.7709e-02 url=www.lawfareblog.com/cyber-command-operational-update-clarifying-june-2019-iran-operation
INFO:root:rank=3 pagerank=1.4604e-02 url=www.lawfareblog.com/aborted-iran-strike-fine-line-between-necessity-and-revenge
INFO:root:rank=4 pagerank=8.4512e-03 url=www.lawfareblog.com/iranian-hostage-crisis-and-its-effect-american-politics
INFO:root:rank=5 pagerank=8.3989e-03 url=www.lawfareblog.com/parsing-state-departments-letter-use-force-against-iran
INFO:root:rank=6 pagerank=8.2581e-03 url=www.lawfareblog.com/announcing-united-states-and-use-force-against-iran-new-lawfare-e-book
INFO:root:rank=7 pagerank=8.0561e-03 url=www.lawfareblog.com/trump-moves-cut-irans-oil-revenues-whats-his-endgame
INFO:root:rank=8 pagerank=7.1939e-03 url=www.lawfareblog.com/us-names-iranian-revolutionary-guard-terrorist-organization-and-sanctions-international-criminal
INFO:root:rank=9 pagerank=5.9405e-03 url=www.lawfareblog.com/iran-shoots-down-us-drone-domestic-and-international-legal-implications

```
**Part 3:**

I then retreieved a list of the pages that have the largest page rank by running:
 ```
 $ python3 pagerank.py --data=./lawfareblog.csv.gz
INFO:root:rank=0 pagerank=8.4156e+00 url=www.lawfareblog.com/litigation-documents-related-appointment-matthew-whitaker-acting-attorney-general
INFO:root:rank=1 pagerank=8.4156e+00 url=www.lawfareblog.com/lawfare-job-board
INFO:root:rank=2 pagerank=8.4156e+00 url=www.lawfareblog.com/documents-related-mueller-investigation
INFO:root:rank=3 pagerank=8.4156e+00 url=www.lawfareblog.com/litigation-documents-resources-related-travel-ban
INFO:root:rank=4 pagerank=8.4156e+00 url=www.lawfareblog.com/subscribe-lawfare
INFO:root:rank=5 pagerank=8.4156e+00 url=www.lawfareblog.com/masthead
INFO:root:rank=6 pagerank=8.4156e+00 url=www.lawfareblog.com/topics
INFO:root:rank=7 pagerank=8.4156e+00 url=www.lawfareblog.com/our-comments-policy
INFO:root:rank=8 pagerank=8.4156e+00 url=www.lawfareblog.com/upcoming-events
INFO:root:rank=9 pagerank=8.4156e+00 url=www.lawfareblog.com/about-lawfare-brief-history-term-and-site
 ```
 However, in order to get a more interesting output,I found the most important articles by modifying the P matrix by removing all links to non-article pages. The `--filter_ratio` argument removes all pages that have more links than the specified fraction. With this option, I estimated the most important articles on hte domain with the following command:
 ```
 $ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2
INFO:root:rank=0 pagerank=4.2773e+00 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=2.7717e+00 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=2 pagerank=2.7533e+00 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=3 pagerank=1.8721e+00 url=www.lawfareblog.com/senate-examines-threats-homeland
INFO:root:rank=4 pagerank=1.7418e+00 url=www.lawfareblog.com/what-make-first-day-impeachment-hearings
INFO:root:rank=5 pagerank=1.7411e+00 url=www.lawfareblog.com/livestream-house-armed-services-committee-hearing-f-35-program
INFO:root:rank=6 pagerank=1.7348e+00 url=www.lawfareblog.com/whats-house-resolution-impeachment
INFO:root:rank=7 pagerank=1.6384e+00 url=www.lawfareblog.com/congress-us-policy-toward-syria-and-turkey-overview-recent-hearings
INFO:root:rank=8 pagerank=1.5597e+00 url=www.lawfareblog.com/summary-david-holmess-deposition-testimony
INFO:root:rank=9 pagerank=9.1265e-01 url=www.lawfareblog.com/events
 ```
 **Part 4:**
 I run the following four commands: 
 ```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose 
$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --alpha=0.99999
$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2
$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2 --alpha=0.99999
 ```
 
These are the outputs for each command, respectively:
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose 
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=8.4156e+00 url=www.lawfareblog.com/litigation-documents-related-appointment-matthew-whitaker-acting-attorney-general
INFO:root:rank=1 pagerank=8.4156e+00 url=www.lawfareblog.com/lawfare-job-board
INFO:root:rank=2 pagerank=8.4156e+00 url=www.lawfareblog.com/documents-related-mueller-investigation
INFO:root:rank=3 pagerank=8.4156e+00 url=www.lawfareblog.com/litigation-documents-resources-related-travel-ban
INFO:root:rank=4 pagerank=8.4156e+00 url=www.lawfareblog.com/subscribe-lawfare
INFO:root:rank=5 pagerank=8.4156e+00 url=www.lawfareblog.com/masthead
INFO:root:rank=6 pagerank=8.4156e+00 url=www.lawfareblog.com/topics
INFO:root:rank=7 pagerank=8.4156e+00 url=www.lawfareblog.com/our-comments-policy
INFO:root:rank=8 pagerank=8.4156e+00 url=www.lawfareblog.com/upcoming-events
INFO:root:rank=9 pagerank=8.4156e+00 url=www.lawfareblog.com/about-lawfare-brief-history-term-and-site

$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --alpha=0.99999
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=1.0624e+01 url=www.lawfareblog.com/snowden-revelations
INFO:root:rank=1 pagerank=1.0624e+01 url=www.lawfareblog.com/lawfare-job-board
INFO:root:rank=2 pagerank=1.0624e+01 url=www.lawfareblog.com/masthead
INFO:root:rank=3 pagerank=1.0624e+01 url=www.lawfareblog.com/litigation-documents-resources-related-travel-ban
INFO:root:rank=4 pagerank=1.0624e+01 url=www.lawfareblog.com/subscribe-lawfare
INFO:root:rank=5 pagerank=1.0624e+01 url=www.lawfareblog.com/litigation-documents-related-appointment-matthew-whitaker-acting-attorney-general
INFO:root:rank=6 pagerank=1.0624e+01 url=www.lawfareblog.com/documents-related-mueller-investigation
INFO:root:rank=7 pagerank=1.0624e+01 url=www.lawfareblog.com/our-comments-policy
INFO:root:rank=8 pagerank=1.0624e+01 url=www.lawfareblog.com/upcoming-events
INFO:root:rank=9 pagerank=1.0624e+01 url=www.lawfareblog.com/topics

$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=4.2773e+00 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=2.7717e+00 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=2 pagerank=2.7533e+00 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=3 pagerank=1.8721e+00 url=www.lawfareblog.com/senate-examines-threats-homeland
INFO:root:rank=4 pagerank=1.7418e+00 url=www.lawfareblog.com/what-make-first-day-impeachment-hearings
INFO:root:rank=5 pagerank=1.7411e+00 url=www.lawfareblog.com/livestream-house-armed-services-committee-hearing-f-35-program
INFO:root:rank=6 pagerank=1.7348e+00 url=www.lawfareblog.com/whats-house-resolution-impeachment
INFO:root:rank=7 pagerank=1.6384e+00 url=www.lawfareblog.com/congress-us-policy-toward-syria-and-turkey-overview-recent-hearings
INFO:root:rank=8 pagerank=1.5597e+00 url=www.lawfareblog.com/summary-david-holmess-deposition-testimony
INFO:root:rank=9 pagerank=9.1265e-01 url=www.lawfareblog.com/events

$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2 --alpha=0.99999
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=4.7947e+01 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=4.7947e+01 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=7.2709e+00 url=www.lawfareblog.com/cost-using-zero-days
INFO:root:rank=3 pagerank=2.1691e+00 url=www.lawfareblog.com/lawfare-podcast-former-congressman-brian-baird-and-daniel-schuman-how-congress-can-continue-function
INFO:root:rank=4 pagerank=1.4214e+00 url=www.lawfareblog.com/events
INFO:root:rank=5 pagerank=1.0863e+00 url=www.lawfareblog.com/water-wars-increased-us-focus-indo-pacific
INFO:root:rank=6 pagerank=1.0863e+00 url=www.lawfareblog.com/water-wars-drill-maybe-drill
INFO:root:rank=7 pagerank=1.0863e+00 url=www.lawfareblog.com/water-wars-disjointed-operations-south-china-sea
INFO:root:rank=8 pagerank=1.0863e+00 url=www.lawfareblog.com/water-wars-us-china-divide-shangri-la
INFO:root:rank=9 pagerank=1.0863e+00 url=www.lawfareblog.com/water-wars-sinking-feeling-philippine-china-relations
```


## Task 2: the personalizagion vector

