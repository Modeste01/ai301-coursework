I recently opened my first open-source pull request to Arbeit-Studio/apikit, a Python library for building typed HTTP clients. My fix catches a typo that used to vanish silently, and makes sure required checks can't get skipped by accident.

If someone accidentally misspelled an argument like "timout=5" instead of "timeout=5", the library didn't throw an error. It quietly absorbed the typo, leaving outgoing HTTP requests with no timeout at all. Custom request headers were also getting dropped along the way, and essential URL checks relied on bare Python assert statements that vanish when Python runs with its optimization flag enabled (-O).

I tracked the root cause to an unvalidated **kwargs catch-all in the gateway constructor. To fix it, I removed the loose kwargs, wired custom headers directly into the network adapter, and replaced the assertions with explicit ValueError checks. I also added new unit tests, bringing the suite to 100 passing tests at 99.09% coverage.

Before I opened the PR, my own pre-merge check stopped me, cleanly, over a stray whitespace line and a plan I hadn't fully followed. Good catch, by me, against myself. The pull request is currently open and awaiting maintainer review.

Working through this showed me how easily silent bugs slip through when APIs accept loose keyword arguments.

https://github.com/Arbeit-Studio/apikit/pull/34 #Python #OpenSource #SoftwareEngineering #CodeQuality
