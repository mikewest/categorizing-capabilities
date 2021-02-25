# Categorizing Capabilities

## Assumptions

Three assumptions have been floating through my head, and I think they might have interesting implications on the way we ask users to make decisions around a given site's capability. For the sake of argument, I'll posit the following:

1.  People can generally categorize the web applications they encounter into categories like "word processor", "map", "photo editor", "news", "video conferencing", "virtual desktop" and so on.

2.  This categorization is meaningfully related to the set of capabilities that people might expect an app to require. It's natural to associate a video conferencing app with microphone and camera access, and less natural to associate them with MIDI or Bluetooth. Likewise, word processors are reasonably likely to make good use of clipboard access, while news sites are unlikely to do the same.

3.  People could generally agree on the broad strokes of this mapping between categories and capabilities.

These assumptions feel reasonable to me (though they're based on nothing but personal anecdote). If we accept them, however, they lead to a view of the world where apps are clustered into categories, and capabilities are clustered within those categories. Identifying which category a given origin falls into would give us a pretty good idea of the capabilities we could expect it to ask for, which hopefully maps onto users' expectations of the capabilities it _ought_ to be able to ask for.

If that view of the world is a reasonable one, then it seems reasonable to me to take a site's category into account when deciding how to respond to its requests for capability. I have some half-baked ideas about the ways that might influence browsers' behavior:

## Implications

Categories could feed into permission decisions in a few ways, some more sane than others:

1.  We could use the category to frame the strings we present to users. "The weather application at `weather.example` wants to know your location.", "The news application at `news.example` wants to use your MIDI devices.", "The virtual desktop app at `remotedesktop.example` wants to see text and images copied to the clipboard." (This would also give us the opportunity to solicit feedback from users about miscategorization).

2.  We could use the category to evaluate how we present a given permission request to users. Perhaps strongly-associated permissions ought to flow through something like the current UX, while weakly-associated permissions ought to flow through something more like Chromium's [quieter permissions UI](https://blog.chromium.org/2020/01/introducing-quieter-permission-ui-for.html) (or be denied entirely).

3.  More interestingly, we could bundle permission requests entirely in terms of the category. For example, rather than granting permission to "Use your camera" and "Use your microphone" and "Share the contents of your screen", we could ask the user whether they would like to "Use `meetings.example` for videoconferencing". This would boil the decision down to something like "Do you expect (trust?) this site to _be_ X.", which seems quite comprehensible.

4.  We could restrict some capabilities to sites within a given category. Perhaps [Web Serial](https://wicg.github.io/serial/) or [WebUSB](https://wicg.github.io/webusb/) could be limited to origins in categories like "device configuration", or [pointer lock](https://w3c.github.io/pointerlock/) to "gaming" or "remote desktop" categories.

Programmatically determining the right category for a site is challenging. Perhaps we can divine good answers based on local analysis, perhaps [federated learning](https://ai.googleblog.com/2017/04/federated-learning-collaborative.html) and [private membership](https://goto.google.com/private-membership-service) checks could share those answers with our friends.

Perhaps we could instead just ask the site to tell us what category it would like to be considered (via an entry in its app manifest, or `<meta>`-delivered microdata, or etc). That would be simpler, and depending on the UI we choose, could be somewhat self-regulated. "Hey!", a user might say to themselves, "`random-website.example` is not a videoconferencing app I chose to use!", which would be a reasonable observation indeed, and could guide them instinctively towards the "Deny" button.

## FAQ

### So, what application archetypes exist?

This is a question that requires more than a little research. [Jason Miller](https://jasonformat.com) has taken [a stab at such a list](https://jasonformat.com/application-holotypes/#meettheholotypes), which looks plausible.


### Surely you're not the first person to have this take?

Nope. [Thomas Steiner](https://blog.tomayac.com/) proposed something vaguely similar in [w3c/permissions#191](https://github.com/w3c/permissions/issues/191), and I'd be shocked if others hadn't done the same.
