---
title: "The Rise of Design Engineers"
description: "An underlying tactonic shift that all tech teams need to realise and the urgent need to ignore specialization"
publishDate: "30 Apr 2024"
tags: ["design", "dev", "future"]
updatedDate: 8 April 2024
---

## TLDR

1. There is new hot job profile in tech industry and it's here to stay and probably disrupt the way we build products
2. Generalist rule and specialization is now for insects

## The Inception

The idea of design engineers branched out from the OG quote and now has became the fundamental of how we see the darkspot between design and engineering
> A human being should be able to change a diaper, plan an invasion, butcher a hog, conn a ship, design a building, write a sonnet, balance accounts, build a wall, set a bone, comfort the dying, take orders, give orders, cooperate, act alone, solve equations, analyze a new problem, pitch manure, program a computer, cook a tasty meal, fight efficiently, die gallantly. Specialization is for insects.

-Robert A. Heinlein

## The tech world takes notice

Your going to have to create a couple of accounts to get things up-and-running. But, the first thing you need to ensure is that your social links are correct.

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Build good relationships with your fellow developers, they can help you to become a sr designer 👇<a href="https://t.co/GAVTujnfIa">https://t.co/GAVTujnfIa</a><a href="https://t.co/08iRaPQaUY">https://t.co/08iRaPQaUY</a> <a href="https://t.co/jbIK7EKhUa">https://t.co/jbIK7EKhUa</a> <a href="https://t.co/fLMzO3a1jD">pic.twitter.com/fLMzO3a1jD</a></p>&mdash; Joshua Guo (@JoshGuoDesign) <a href="https://twitter.com/JoshGuoDesign/status/1724249311397966092?ref_src=twsrc%5Etfw">November 14, 2023</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

### Add link(s) to your profile(s)

Firstly, you need to add a link on your site to prove ownership. If you have a look at [IndieLogin's](https://indielogin.com/setup) instructions, it gives you 2 options, either an email address and/or GitHub account. I've created the component `src/components/SocialList.astro` where you can add your details into the `socialLinks` array, just include the `isWebmention` property to the relevant link which will add the `rel="me authn"` attribute. Whichever way you do it, make sure you have a link in your markup as per IndieLogin's [instructions](https://indielogin.com/setup)

```html
<a href="https://github.com/your-username" rel="me">GitHub</a>
```

### Sign up to Webmention.io

Next, head over to [Webmention.io](https://webmention.io/) and create an account by signing in with your domain name, e.g. `https://astro-cactus.chriswilliams.dev/`. Please note that .app TLDs don't function correctly. Once in, it will give you a couple of links for your domain to accept webmentions. Make a note of these and head over to `src/site.config.ts` and add them to `siteConfig.webmentions`.

Quick note: You don't have to include the pingback link. Maybe coincidentally, but after adding it I started to receive a higher frequency of spam in my mailbox, informing me that my website could be better. Tbh they're not wrong. I've now removed it, but it's up to you.

### Adding your api key

Next is to add your api key, also from [Webmention.io](https://webmention.io/), to a `.env` file. Rename the `.example.env`, or create your own, with `WEBMENTION_API_KEY=` and then your personal key. Please try not to publish this to a repository!

### Sign up to Brid.gy

You're now going to have to use [brid.gy](https://brid.gy/). As the name suggests, it links your website to your social media accounts. For every account you want to set up (e.g. Mastodon), click on the relevant button and connect each account you want brid.gy to search. Just to note again, brid.gy currently has an issue with .app TLDs.

## Testing everything works

With everything set, it's now time to build and publish your website. **REMEMBER** to set your environment variable `WEBMENTION_API_KEY` with your host, I also forgot this part.

You can check to see if everything is working by sending a test webmention via [webmentions.rocks](https://webmention.rocks/receive/1). Log in with your domain, enter the auth code, and then the url of the page you want to test. For example, to test this page I would add `https://astro-cactus.chriswilliams.dev/posts/webmentions/`. To view it on your website, rebuild or (re)start dev mode locally, and you should see the result at the bottom of your page.

You can also view any test mentions in the browser via their [api](https://github.com/aaronpk/webmention.io#api).

## Things to add, things to consider

- At the moment, fresh webmentions are only fetched on a rebuild or restarting dev mode, which obviously means if you don't update your site very often you wont get a lot of new content. It should be quite trivial to add a cron job to run the `getAndCacheWebmentions()` function in `src/utils/webmentions.ts` and populate your blog with new content. This is probably what I'll add next as a github action.

- I have seen some mentions have duplicates. Unfortunately, they're quite difficult to filter out as they have different id's.

- I'm not a huge fan of the little external link icon for linking to comments/replies. It's not particularly great on mobile due to its size, and will likely change it in the future.

## Acknowledgements

Many thanks to [Kieran McGuire](https://github.com/chrismwilliams/astro-theme-cactus/issues/107#issue-1863931105) for sharing this with me, and the helpful posts. I'd never heard of webmentions before, and now with this update hopefully others will be able to make use of them. Additionally, articles and examples from [kld](https://kld.dev/adding-webmentions/) and [ryanmulligan.dev](https://ryanmulligan.dev/blog/) really helped in getting this set up and integrated, both a great resource if you're looking for more information!
