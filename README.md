# Using Pagerank to create a search engine for lawfareblog.com

## Update:
pagerank.py file now uses further developed --search_query argument. Program now uses the gensim glove-twitter-25 model to identify the top 5 most similar words.

Running the update file on a search for 'weapons' gives us:
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='weapons'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=4.5715e-03 url=www.lawfareblog.com/why-did-you-wait-moral-emptiness-and-drone-strikes
INFO:root:rank=1 pagerank=3.1107e-03 url=www.lawfareblog.com/dc-district-court-dismisses-journalists-drone-lawsuit
INFO:root:rank=2 pagerank=2.0231e-03 url=www.lawfareblog.com/revived-cia-drone-strike-program-comments-new-policy
INFO:root:rank=3 pagerank=1.9667e-03 url=www.lawfareblog.com/us-court-appeals-dc-circuit-dismisses-suit-over-us-drone-strike
INFO:root:rank=4 pagerank=1.1788e-03 url=www.lawfareblog.com/iran-shoots-down-us-drone-domestic-and-international-legal-implications
INFO:root:rank=5 pagerank=1.1620e-03 url=www.lawfareblog.com/slaughterbots-and-other-anticipated-autonomous-weapons-problems
INFO:root:rank=6 pagerank=1.1276e-03 url=www.lawfareblog.com/german-courts-weigh-legal-responsibility-us-drone-strikes
INFO:root:rank=7 pagerank=8.3738e-04 url=www.lawfareblog.com/shift-jsoc-drone-strikes-does-not-mean-cia-has-been-sidelined
INFO:root:rank=8 pagerank=7.8704e-04 url=www.lawfareblog.com/atomwaffen-division-member-pleads-guilty-firearms-charge
INFO:root:rank=9 pagerank=7.8570e-04 url=www.lawfareblog.com/waiving-imminent-threat-test-cia-drone-strikes-pakistan
```



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

I then used the `--search_query` option, which takes in a string as a paramter. I used the strings 'corona', 'trump', and 'iran', respectively.

```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='corona'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.001003776676952839 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=1 pagerank=0.0008922395063564181 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
INFO:root:rank=2 pagerank=0.0007039029151201248 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=3 pagerank=0.0006915341946296394 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=4 pagerank=0.000670412031468004 url=www.lawfareblog.com/israeli-emergency-regulations-location-tracking-coronavirus-carriers
INFO:root:rank=5 pagerank=0.0006625585374422371 url=www.lawfareblog.com/why-congress-conducting-business-usual-face-coronavirus
INFO:root:rank=6 pagerank=0.0006504578050225973 url=www.lawfareblog.com/congressional-homeland-security-committees-seek-ways-support-state-federal-responses-coronavirus
INFO:root:rank=7 pagerank=0.0006361958803609014 url=www.lawfareblog.com/paper-hearing-experts-debate-digital-contact-tracing-and-coronavirus-privacy-concerns
INFO:root:rank=8 pagerank=0.000612482544966042 url=www.lawfareblog.com/house-subcommittee-voices-concerns-over-us-management-coronavirus
INFO:root:rank=9 pagerank=0.0006018723943270743 url=www.lawfareblog.com/livestream-house-oversight-committee-holds-hearing-government-coronavirus-response


$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='trump'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.005782557651400566 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=0.005233839154243469 url=www.lawfareblog.com/document-trump-revokes-obama-executive-order-counterterrorism-strike-casualty-reporting
INFO:root:rank=2 pagerank=0.005129670724272728 url=www.lawfareblog.com/trump-administrations-worrying-new-policy-israeli-settlements
INFO:root:rank=3 pagerank=0.004659898113459349 url=www.lawfareblog.com/dc-circuit-overrules-district-courts-due-process-ruling-qasim-v-trump
INFO:root:rank=4 pagerank=0.004593398422002792 url=www.lawfareblog.com/donald-trump-and-politically-weaponized-executive-branch
INFO:root:rank=5 pagerank=0.004307133611291647 url=www.lawfareblog.com/how-trumps-approach-middle-east-ignores-past-future-and-human-condition
INFO:root:rank=6 pagerank=0.0040934765711426735 url=www.lawfareblog.com/why-trump-cant-buy-greenland
INFO:root:rank=7 pagerank=0.0037590833380818367 url=www.lawfareblog.com/oral-argument-summary-qassim-v-trump
INFO:root:rank=8 pagerank=0.003450872143730521 url=www.lawfareblog.com/dc-circuit-court-denies-trump-rehearing-mazars-case
INFO:root:rank=9 pagerank=0.0034484383650124073 url=www.lawfareblog.com/second-circuit-rules-mazars-must-hand-over-trump-tax-returns-new-york-prosecutors


$ python3 pagerank.py --data=./lawfareblog.csv.gz --search_query='iran'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.005129670724272728 url=www.lawfareblog.com/trump-administrations-worrying-new-policy-israeli-settlements
INFO:root:rank=1 pagerank=0.005016769748181105 url=www.lawfareblog.com/update-military-commissions-continued-health-issues-recusal-motion-and-new-cell-al-iraqi
INFO:root:rank=2 pagerank=0.004574609454721212 url=www.lawfareblog.com/praise-presidents-iran-tweets
INFO:root:rank=3 pagerank=0.004417411983013153 url=www.lawfareblog.com/how-us-iran-tensions-could-disrupt-iraqs-fragile-peace
INFO:root:rank=4 pagerank=0.004365929868072271 url=www.lawfareblog.com/haftar-attacking-tripoli-us-needs-re-engage-libya
INFO:root:rank=5 pagerank=0.0034236714709550142 url=www.lawfareblog.com/france-makes-play-try-foreign-fighters-iraq
INFO:root:rank=6 pagerank=0.0026927897706627846 url=www.lawfareblog.com/cyber-command-operational-update-clarifying-june-2019-iran-operation
INFO:root:rank=7 pagerank=0.002256679581478238 url=www.lawfareblog.com/document-sens-kaine-and-young-introduce-bill-revoke-iraq-force-authorizations
INFO:root:rank=8 pagerank=0.0019391420064494014 url=www.lawfareblog.com/aborted-iran-strike-fine-line-between-necessity-and-revenge
INFO:root:rank=9 pagerank=0.0018262730445712805 url=www.lawfareblog.com/its-not-only-iraq-and-syria
```
**Part 3:**

I then retreieved a list of the pages that have the largest page rank by running:
 ```
$ python3 pagerank.py --data=./lawfareblog.csv.gz 
INFO:root:rank=0 pagerank=0.2874051630496979 url=www.lawfareblog.com/about-lawfare-brief-history-term-and-site
INFO:root:rank=1 pagerank=0.2874051630496979 url=www.lawfareblog.com/lawfare-job-board
INFO:root:rank=2 pagerank=0.2874051630496979 url=www.lawfareblog.com/masthead
INFO:root:rank=3 pagerank=0.2874051630496979 url=www.lawfareblog.com/litigation-documents-resources-related-travel-ban
INFO:root:rank=4 pagerank=0.2874051630496979 url=www.lawfareblog.com/subscribe-lawfare
INFO:root:rank=5 pagerank=0.2874051630496979 url=www.lawfareblog.com/litigation-documents-related-appointment-matthew-whitaker-acting-attorney-general
INFO:root:rank=6 pagerank=0.2874051630496979 url=www.lawfareblog.com/documents-related-mueller-investigation
INFO:root:rank=7 pagerank=0.2874051630496979 url=www.lawfareblog.com/our-comments-policy
INFO:root:rank=8 pagerank=0.2874051630496979 url=www.lawfareblog.com/upcoming-events
INFO:root:rank=9 pagerank=0.2874051630496979 url=www.lawfareblog.com/topics
 ```
 However, in order to get a more interesting output,I found the most important articles by modifying the P matrix by removing all links to non-article pages. The `--filter_ratio` argument removes all pages that have more links than the specified fraction. With this option, I estimated the most important articles on hte domain with the following command:
 ```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2
INFO:root:rank=0 pagerank=0.3469613492488861 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=0.29521211981773376 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=2 pagerank=0.29039666056632996 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=3 pagerank=0.15178653597831726 url=www.lawfareblog.com/lawfare-podcast-ben-nimmo-whack-mole-game-disinformation
INFO:root:rank=4 pagerank=0.15098513662815094 url=www.lawfareblog.com/todays-headlines-and-commentary-1964
INFO:root:rank=5 pagerank=0.15098513662815094 url=www.lawfareblog.com/todays-headlines-and-commentary-1963
INFO:root:rank=6 pagerank=0.15071173012256622 url=www.lawfareblog.com/lawfare-podcast-week-was-impeachment
INFO:root:rank=7 pagerank=0.14956679940223694 url=www.lawfareblog.com/todays-headlines-and-commentary-1962
INFO:root:rank=8 pagerank=0.14366623759269714 url=www.lawfareblog.com/cyberlaw-podcast-mistrusting-google
INFO:root:rank=9 pagerank=0.14239734411239624 url=www.lawfareblog.com/lawfare-podcast-bonus-edition-gordon-sondland-vs-committee-no-bull
 ```
 **Part 4:**
 I ran the following four commands: 
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
INFO:root:rank=0 pagerank=0.2874051630496979 url=www.lawfareblog.com/about-lawfare-brief-history-term-and-site
INFO:root:rank=1 pagerank=0.2874051630496979 url=www.lawfareblog.com/lawfare-job-board
INFO:root:rank=2 pagerank=0.2874051630496979 url=www.lawfareblog.com/masthead
INFO:root:rank=3 pagerank=0.2874051630496979 url=www.lawfareblog.com/litigation-documents-resources-related-travel-ban
INFO:root:rank=4 pagerank=0.2874051630496979 url=www.lawfareblog.com/subscribe-lawfare
INFO:root:rank=5 pagerank=0.2874051630496979 url=www.lawfareblog.com/litigation-documents-related-appointment-matthew-whitaker-acting-attorney-general
INFO:root:rank=6 pagerank=0.2874051630496979 url=www.lawfareblog.com/documents-related-mueller-investigation
INFO:root:rank=7 pagerank=0.2874051630496979 url=www.lawfareblog.com/our-comments-policy
INFO:root:rank=8 pagerank=0.2874051630496979 url=www.lawfareblog.com/upcoming-events
INFO:root:rank=9 pagerank=0.2874051630496979 url=www.lawfareblog.com/topics

$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --alpha=0.99999
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=0.28859302401542664 url=www.lawfareblog.com/snowden-revelations
INFO:root:rank=1 pagerank=0.28859302401542664 url=www.lawfareblog.com/lawfare-job-board
INFO:root:rank=2 pagerank=0.28859302401542664 url=www.lawfareblog.com/documents-related-mueller-investigation
INFO:root:rank=3 pagerank=0.28859302401542664 url=www.lawfareblog.com/litigation-documents-resources-related-travel-ban
INFO:root:rank=4 pagerank=0.28859302401542664 url=www.lawfareblog.com/subscribe-lawfare
INFO:root:rank=5 pagerank=0.28859302401542664 url=www.lawfareblog.com/topics
INFO:root:rank=6 pagerank=0.28859302401542664 url=www.lawfareblog.com/masthead
INFO:root:rank=7 pagerank=0.28859302401542664 url=www.lawfareblog.com/our-comments-policy
INFO:root:rank=8 pagerank=0.28859302401542664 url=www.lawfareblog.com/upcoming-events
INFO:root:rank=9 pagerank=0.28859302401542664 url=www.lawfareblog.com/litigation-documents-related-appointment-matthew-whitaker-acting-attorney-general

$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=0.3469613492488861 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=0.29521211981773376 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=2 pagerank=0.29039666056632996 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=3 pagerank=0.15178653597831726 url=www.lawfareblog.com/lawfare-podcast-ben-nimmo-whack-mole-game-disinformation
INFO:root:rank=4 pagerank=0.15098513662815094 url=www.lawfareblog.com/todays-headlines-and-commentary-1964
INFO:root:rank=5 pagerank=0.15098513662815094 url=www.lawfareblog.com/todays-headlines-and-commentary-1963
INFO:root:rank=6 pagerank=0.15071173012256622 url=www.lawfareblog.com/lawfare-podcast-week-was-impeachment
INFO:root:rank=7 pagerank=0.14956679940223694 url=www.lawfareblog.com/todays-headlines-and-commentary-1962
INFO:root:rank=8 pagerank=0.14366623759269714 url=www.lawfareblog.com/cyberlaw-podcast-mistrusting-google
INFO:root:rank=9 pagerank=0.14239734411239624 url=www.lawfareblog.com/lawfare-podcast-bonus-edition-gordon-sondland-vs-committee-no-bull


$ python3 pagerank.py --data=./lawfareblog.csv.gz --verbose --filter_ratio=0.2 --alpha=0.99999
DEBUG:root:computing indices
DEBUG:root:computing values
INFO:root:rank=0 pagerank=0.7014895677566528 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=0.7014873623847961 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=0.10551629960536957 url=www.lawfareblog.com/cost-using-zero-days
INFO:root:rank=3 pagerank=0.03175504133105278 url=www.lawfareblog.com/lawfare-podcast-former-congressman-brian-baird-and-daniel-schuman-how-congress-can-continue-function
INFO:root:rank=4 pagerank=0.022039713338017464 url=www.lawfareblog.com/events
INFO:root:rank=5 pagerank=0.01602667197585106 url=www.lawfareblog.com/water-wars-increased-us-focus-indo-pacific
INFO:root:rank=6 pagerank=0.016025930643081665 url=www.lawfareblog.com/water-wars-drill-maybe-drill
INFO:root:rank=7 pagerank=0.016022762283682823 url=www.lawfareblog.com/water-wars-disjointed-operations-south-china-sea
INFO:root:rank=8 pagerank=0.016019931063055992 url=www.lawfareblog.com/water-wars-song-oil-and-fire
INFO:root:rank=9 pagerank=0.016019923612475395 url=www.lawfareblog.com/water-wars-sinking-feeling-philippine-china-relations
```


## Task 2: the personalization vector

**Part 1:**

I implemented the WebGraph.make_personalization_vector function which enables the `--personalization_vector_query` command line argument. This provides an alternative method for searching by filtering by the personalization vector.

I got the results:
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='corona'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.6321316361427307 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=0.6321078538894653 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=0.1496182531118393 url=www.lawfareblog.com/chinatalk-how-party-takes-its-propaganda-global
INFO:root:rank=3 pagerank=0.11625612527132034 url=www.lawfareblog.com/rational-security-my-corona-edition
INFO:root:rank=4 pagerank=0.11625612527132034 url=www.lawfareblog.com/brexit-not-immune-coronavirus
INFO:root:rank=5 pagerank=0.08883301168680191 url=www.lawfareblog.com/trump-cant-reopen-country-over-state-objections
INFO:root:rank=6 pagerank=0.08544307202100754 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=7 pagerank=0.08544307202100754 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=8 pagerank=0.07188324630260468 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=9 pagerank=0.06896847486495972 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response

```

These results are significantly different than when using the `--search_query` option:
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --search_query='corona'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.008131962269544601 url=www.lawfareblog.com/house-oversight-committee-holds-day-two-hearing-government-coronavirus-response
INFO:root:rank=1 pagerank=0.007790825795382261 url=www.lawfareblog.com/lawfare-podcast-united-nations-and-coronavirus-crisis
INFO:root:rank=2 pagerank=0.005226220469921827 url=www.lawfareblog.com/livestream-house-oversight-committee-holds-hearing-government-coronavirus-response
INFO:root:rank=3 pagerank=0.0039583845064044 url=www.lawfareblog.com/britains-coronavirus-response
INFO:root:rank=4 pagerank=0.003811446251347661 url=www.lawfareblog.com/prosecuting-purposeful-coronavirus-exposure-terrorism
INFO:root:rank=5 pagerank=0.003397284774109721 url=www.lawfareblog.com/paper-hearing-experts-debate-digital-contact-tracing-and-coronavirus-privacy-concerns
INFO:root:rank=6 pagerank=0.0033633282873779535 url=www.lawfareblog.com/cyberlaw-podcast-how-israel-fighting-coronavirus
INFO:root:rank=7 pagerank=0.0033556800335645676 url=www.lawfareblog.com/israeli-emergency-regulations-location-tracking-coronavirus-carriers
INFO:root:rank=8 pagerank=0.0032159897964447737 url=www.lawfareblog.com/congress-needs-coronavirus-failsafe-its-too-late
INFO:root:rank=9 pagerank=0.0031036492437124252 url=www.lawfareblog.com/why-congress-conducting-business-usual-face-coronavirus
```
**Part 2:**

I was also able to find webpages that are related to the coronavirus but don't directly mention the coronavirus. This is also accomplishes using the `--personalization_vector_query' flag. The following query ranks all webpages by their corona importance, but removes webpages mentioning corona from the results:

```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='corona' --search_query='-corona'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.6321316361427307 url=www.lawfareblog.com/covid-19-speech-and-surveillance-response
INFO:root:rank=1 pagerank=0.6321078538894653 url=www.lawfareblog.com/lawfare-live-covid-19-speech-and-surveillance
INFO:root:rank=2 pagerank=0.1496182531118393 url=www.lawfareblog.com/chinatalk-how-party-takes-its-propaganda-global
INFO:root:rank=3 pagerank=0.08883301168680191 url=www.lawfareblog.com/trump-cant-reopen-country-over-state-objections
INFO:root:rank=4 pagerank=0.06856223940849304 url=www.lawfareblog.com/lawfare-podcast-mom-and-dad-talk-clinical-trials-pandemic
INFO:root:rank=5 pagerank=0.06583793461322784 url=www.lawfareblog.com/fault-lines-foreign-policy-quarantined
INFO:root:rank=6 pagerank=0.06138917803764343 url=www.lawfareblog.com/limits-world-health-organization
INFO:root:rank=7 pagerank=0.05593918263912201 url=www.lawfareblog.com/chinatalk-dispatches-shanghai-beijing-and-hong-kong
INFO:root:rank=8 pagerank=0.054060198366642 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
```
**Part 3:**
I then picked a different national security topic. I chose articles that are important to the `afghanistan` but do not contain the word `afghanistan`. This is especially currently relevant because the Afghan government and the Taliban just began peace talks. I ran the following:
```
$ python3 pagerank.py --data=./lawfareblog.csv.gz --filter_ratio=0.2 --personalization_vector_query='afghanistan' --search_query='-afghanistan'
INFO:gensim.models.keyedvectors:precomputing L2-norms of word weight vectors
INFO:root:rank=0 pagerank=0.3140121102333069 url=www.lawfareblog.com/trump-asks-supreme-court-stay-congressional-subpeona-tax-returns
INFO:root:rank=1 pagerank=0.30366256833076477 url=www.lawfareblog.com/omphalos
INFO:root:rank=2 pagerank=0.19725105166435242 url=www.lawfareblog.com/livestream-nov-21-impeachment-hearings-0
INFO:root:rank=3 pagerank=0.19724050164222717 url=www.lawfareblog.com/opening-statement-david-holmes
INFO:root:rank=4 pagerank=0.19681087136268616 url=www.lawfareblog.com/haftar-attacking-tripoli-us-needs-re-engage-libya
INFO:root:rank=5 pagerank=0.19675233960151672 url=www.lawfareblog.com/how-us-iran-tensions-could-disrupt-iraqs-fragile-peace
INFO:root:rank=6 pagerank=0.18543751537799835 url=www.lawfareblog.com/cancellation-algerias-elections-opportunity-democratization
INFO:root:rank=7 pagerank=0.18190492689609528 url=www.lawfareblog.com/yemen-houthi-strategy-has-promise-and-risk
INFO:root:rank=8 pagerank=0.18189232051372528 url=www.lawfareblog.com/how-trumps-approach-middle-east-ignores-past-future-and-human-condition
INFO:root:rank=9 pagerank=0.14915604889392853 url=www.lawfareblog.com/depoliticizing-foreign-interference
```





