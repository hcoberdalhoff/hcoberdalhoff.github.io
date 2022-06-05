---
layout: post
title: Having a loosely walled garden
date: 2022-06-05
author: Hans-Carl
categories:
    - Trends
---

# Intro - MS Teams as the new operating system
Have you read [Stratchery](https://stratechery.com/)'s article on [Thin platforms](https://stratechery.com/2022/thin-platforms/)? If not, I can recommend reading it. It basically describes how the war for dominance in enterprise IT shifted from the operating system over browsers to a new place to encompass all your business needs into one ecosystem. He specifically described how Microsoft Teams becomes your new "operating system" as it will integrate and host most of your applications and data. One key difference being that you are not tied to a certain device or actual operating system anymore.  

# The war against the walled garden
Btw, I stand by it (had this a few times already) - This is the same story behind Apple's and others' struggle to keep their app ecosystems either "closed" or trying to pry them "open". Can a company like Apple make sure that all user relevant interactions have to go through their ecosystem and therefore billing system? So, does everything on an iPhone/iPad have to come from the store and everything that comes from the web through their browser - and thus inherently support their payment system, biometrics, account management - in short: Apple's APIs and services? [That's what an Epic Games is fighting for](https://en.wikipedia.org/wiki/Epic_Games_v._Apple).

# Is the game industry doing the same thing?
It seems that the games industry and enterprise app environment are going through the same development in parallel just having different participants.

Example: The companies Valve and Microsoft are trying to achieve basically the same goals with very different means in slightly different arenas - abstraction from the firmly defined access path into the ecosystem. Also, developers should not have to worry about this - as long as they offer their products in the right (market)place.

Microsoft provides the [XBox ecosystem](https://www.xbox.com) for this in the games context. Users can purchase/gain access to apps/games with little (no?) risk. The ecosystem takes care of authentication and also persistence of user data similar to what Office 365/Teams does in a business/enterprise context. This includes data like game saves and settings. Communication services (social sharing, real-time meetings/shared play with audio/video) are also handled by the ecosystem.

Valve offers something similar with the [Steam](https://www.steampowered.com) platform, i.e. identity, trading platform, app marketplace and centralized unified APIs for all these things, including functions inside the games such as cloud saves and achievement systems.

In short, they are also "operating systems" by Stratchery's definition. In detail, one would still have to discuss whether "game engines" like Unreal Engine are possibly still fighting for this title - but not in the view chosen here, since we are concerned with the coexistence and interaction of apps from the user's point of view and also the user's access to app/services.

# What are MS and Valve doing differently?
Is there a difference in the current development between Microsoft's and Valve's approach?

Both companies are trying to enable more users to access these services and also make their services easier and more useful to use in more scenarios. For example, neither company had a sensible answer for how a user can take advantage of the services on the move - for example, while waiting for a train at the station. Not everyone can comfortably unpack a fat gaming laptop to game while waiting for the train at the tracks. Pairing your service primarily with a physical Xbox/Windows PCs/notebooks also becomes an obstacle in this scenario.

## Valve's approach 
Valve's answer is [SteamOS](https://store.steampowered.com/steamos) and [Steam Deck](https://www.steamdeck.com/)

Almost trying to create a square circle: Offering access to games developed for stationary Windows PC on a Linux-based handheld that rivals a Nintendo Switch in form factor. And doing it while not asking developers to make a single adjustment to their products.

As Valve, you gain users from the casual/mobile environment, compete with Nintendo on the device and can use your huge existing app catalog to shake up the market in the portable environment. Any user who gets involved will appreciate the benefits of the full ecosystem including store, authentication and cross-device persistence of data. Existing users can leverage their investment in more situations (and spend even more time in the ecosystem).

Noteably, users are free to use other [marketplaces and software](https://www.xda-developers.com/how-to-install-play-games-epic-games-store-steam-deck/) on Steam Deck or even [install Windows](https://www.tomshardware.com/how-to/install-windows-steam-deck) intead of SteamOS. It just is not as nice as using Steam and SteamOS.

Valve leverages (and massively supports) the open source [Proton virtualization layer](https://github.com/ValveSoftware/Proton) to realize having Windows titles run on Linux.

## Microsoft's approach
Microsoft is taking a similar approach with its [cloud gaming](https://www.xbox.com/en-us/xbox-game-pass/cloud-gaming). A platform that was previously only 100% available on the couch sitting in front of a Xbox and around 80% on the PC is now suddenly sufficiently accessible on almost any device. You just log in to the service - similar to a teams web client and can access your apps, social network and existing data.

In some cases, this works very well - i.e. access via native apps on Windows, Android or even Xbox itself is hardly distinguishable from the local use of the apps/services. Other access paths via e.g. Safari on iOS still have limitations, especially with latency. That is part of the battle that Apple is waging, because technically there is a native app offered by Microsoft that does not have these problems. [You just are not allowed to have it by Apple](https://www.windowscentral.com/apple-responds-xbox-project-xcloud-ios-roadblock-citing-store-policies).

# Final thoughts
Again, it's about nothing less than full dominance in the user experience. A user should in these companies' minds have no advantage to leave the respective ecosystem. 

Valve and MS focus on the advantage side, but leave it open for the user to use other systems as well. I would call this a 'loosely walled garden'. 

You can also compare this to how Uber is trying to incorporate everything that is transportation related (taxi-style transfer, food delivery, car sharing, eScooter-rental) into one ecosystem. Just as a sidenote here.

Apple tries to offer maximum convenience, security and privacy for its users - but actively excludes other systems and access through non-Apple means. 

Google is offering services to private and enterprise customers and also tried to dabble in the [(cloud) gaming space](https://stadia.google.com/) but as so often did not fully find its groove.

And of course there is something like Roblox. Trying to build a [full-circle app-development and social ecosystem for gaming](https://www.thedrum.com/news/2022/03/31/the-most-social-ecosystem-the-planet-roblox-s-new-generation-makers-and-buyers) in a kids space. 

The approaches across all these companies seem immensely similar from a benefit and business side but vary widely in their implementation. We can conclude that users will have a very complete and all-encompassing experience whichever service they will choose to use, be it in gaming or enterprise use. And we will see some weird and completely outlandish experiments on the way. I'm looking forward to some wild times.
