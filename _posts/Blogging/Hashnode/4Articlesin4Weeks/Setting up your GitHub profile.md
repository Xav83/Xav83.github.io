# How you can create an awesome GitHub profile

Hello ! I'm Xavier Jouvenot and this is my [fourth blog post on Hashnode](https://10xlearner.hashnode.dev/how-you-can-create-an-awesome-github-profile), and also the latest for the [Writeathon "4 Articles in 4 Weeks"](https://townhall.hashnode.com/4-articles-in-4-weeks-hashnode-writeathon)! And since the topics is free, I will tell you how I ended up creating my personal custom GitHub profile.

_Self promotion_:
Here are a few social networks where you can follow me and check my work as a programmer and a writer 😉
[Personal blog](www.10xlearner.com), [Twitter](https://twitter.com/10xLearner), [GitHub](https://github.com/Xav83/)

## Create a custom profile

By default, every user has a GitHub profile with some elements displayed, like the popular repositories of the user and its personal contributions.
But, as you may have seen on some other GitHub users, it is possible to customize your GitHub profile in order to add any information.
So, now that I can officially be a Freelancer, I wanted to improve my online image/presence!
And I found out that having a nice GitHub profile, describing who I am, what I did and what I can do, is a good way to achieve that goal 🙂

To create your own custom GitHub profile, all you need to do is to create a new repository named like your own GitHub username.
When doing so, GitHub will warn you that you are creating a `special repository`.
And once you will have validated your choice, you will face a fresh repository with a `README` file with the message `Hi there 👋`, message that is now displayed on your own personal profile, as you now have a custom personal profile up and ready.

The only thing you have to do now that your custom profile is set up, is to fill the `README` file with whatever you like to actually make it yours.
In the rest of this article, we are going to focus on some elements that you can display in your profile to make it reflect information about you.

## Technologies and tools

If you go on my profile, you will find a ["Language and Tools" section](https://github.com/Xav83#languages-and-tools), listing the one I am familiar with. It is not innovative in any way, as I have inspired myself from what other people have been doing on their own profile. But instead of making a long text detailing all the elements, I have opted to list them using the logo of each of those tools/languages. I found this way of displaying the information more visual as people in the industry will more easily point at the logo of the technologies they are also familiar with. 🙂

Let's look at an example of what integrated one logo looks like in the Markdown file:
```
<div>
 <img src="https://github.com/devicons/devicon/blob/master/icons/git/git-original.svg" title="Git" alt="Git" width="40" height="40"/>
 <!-- other logos -->
</div>
```
As you can see, even if we are in a Markdown, GitHub allows us to use a little bit of HTML in order to customize our profile a little bit more.
This logo is the one of `Git`, and I got it from the [devicon GitHub reposity](https://devicon.dev) which stores a lot of logos related to all kinds of technologies.

*Note*: As you can see, for now, I am directly using the resources from the GitHub repository, but `devicon` offers CDN links, which would be more reliable to use. I will change that in a near future 😉

## Social Networks and Other Links

The next part on my profile on which we are going to take a look is [the section about my personal social network accounts](https://github.com/Xav83#lets-connect), where people can connect with me. This is really useful as GitHub doesn't provide any way to directly message someone.
So if you want people to be able to give you a shout out about one of the projects you are working on, or if you want a recruiter to contact you after looking at your wonderful GitHub profile, I highly recommend that you add yours on your GitHub profile.

To do so, we are, once again, going to use HTML:
```
<div>
 <a href="https://twitter.com/10xLearner">
   <img src="https://img.shields.io/badge/Twitter-1DA1F2?style=for-the-badge&logo=twitter&logoColor=white"/>
 </a>
 <!-- other social network badges -->
</div>
```
Here, you are looking at the code which allows me to display a `badge` about my [Twitter account ](https://twitter.com/10xLearner) (that you can also follow 😉🫶). It is composed of one image generated using the [website Shields.io](https://shields.io/category/social) which allows you to generate various badges displaying different information depending on what you are referencing with this badge. Personally, I have opted to display only the social network, but you can generate a badge displaying also the number of people following you, if you prefer.

Another great resource, for such badges, I have found when creating my GitHub profile is the repository [a11y-badges](https://github.com/a11y-badges/a11y-markdown-badges). In that repository, I found a `Hashnode` badge 😊

The image is then wrapped in an anchor element which allows people to click on the image to be redirected on my personal Twitter account. That way, people are one click away to contact me, if they ever feel like it 😉

## GitHub stats and activity

Now, we are entering the shiniest element in my profile, in my opinion ✨✨

Indeed, we can display, in our GitHub profile, some dynamic information about our activity on GitHub.
I can, for example, show how many Pull Requests and commits I have made this year, and show what are the languages I mostly use.

In order to be able to display that information, we are going to use the [following GitHub repository](https://github.com/anuraghazra/github-readme-stats). From the service provided by this repository and all the documentation in the README, we can generate some simple markdown images to display all the information we want. Here are what the code for those images looks for my GitHub profile.:
```markdown
![My personal GitHub Stats](https://github-readme-stats.vercel.app/api?username=Xav83&show_icons=true&count_private=true&theme=react&hide_border=true&bg_color=0D1117)
![My personal Top Langs](https://github-readme-stats.vercel.app/api/top-langs/?username=Xav83&langs_count=8&count_private=true&layout=compact&theme=react&hide_border=true&bg_color=0D1117)
![My personal Activity Graph](https://activity-graph.herokuapp.com/graph?username=Xav83&bg_color=0D1117&color=5BCDEC&line=5BCDEC&point=FFFFFF&hide_border=true)
```

If you want to integrate the same element in your profile, you can directly copy-paste the same code I use and change the `username` field with your GitHub username 😉

But you can also tweak the code in order to customize each element and make them more personal and suits your GitHub profile. All the options available to you to do so are explained really well on the [GitHub repository](https://github.com/anuraghazra/github-readme-stats) providing this service.

## Latest blog and GitHub activities

For the latest part of this article, we are going to take a foot in the landscape of GitHub Action and what they can bring to your GitHub profile.

Indeed, in order to display some up to date information about our GitHub activity, for example, we need to use a system which will be able to update our profile on a regular basis. The simpler way to do it, is to use [GitHub Actions](https://docs.github.com/en/actions), which is directly provided with your GitHub account.

Let's start with displaying a GitHub Action allowing you (and me 😉) to display the latest article you posted on your personal blog/website. This GitHub Action is rightfully named ["Blog post workflow"](https://github.com/gautamkrishnar/blog-post-workflow), created by [@gautamkrishnar](https://github.com/gautamkrishnar), and can easily be setup by adding anchors in your README, and adding a simple configuration file in the **GitHub action workflow folder**. Here are what the "code" each of those elements looks like for my personal profile:
- The 2 anchors in the README, where the list of elements linking to your blog will be displayed

```
<!-- BLOG-POST-LIST:START -->
<!-- BLOG-POST-LIST:END -->
```

- The configuration of the GitHub Actions ([update-blog-posts.yml](https://github.com/Xav83/Xav83/blob/main/.github/workflows/update-blog-posts.yml)), where you can see the field `feed_list` specifying the link to the RSS feed of my personal blog

```
name: Latest blog post workflow
on:
 schedule: # Run workflow automatically
   - cron: '0 * * * *' # Runs every hour, on the hour
 workflow_dispatch: # Run workflow manually (without waiting for the cron to be called), through the GitHub Actions Workflow page directly

jobs:
 update-readme-with-blog:
   name: Update this repo's README with latest blog posts
   runs-on: ubuntu-latest
   steps:
     - name: Checkout
       uses: actions/checkout@v2
     - name: Pull my personal blog posts
       uses: gautamkrishnar/blog-post-workflow@v1
       with:
         feed_list: "https://10xlearner.com/feed/"
```

*Note*: I encourage you to explore and experiment with this GitHub Actions as there are a lot of options to customize the rendering of the elements you will be displaying

The second, and last, GitHub Action I want you to take a look at is named ["GitHub Activity in Readme"](https://github.com/jamesgeorge007/github-activity-readme), created by [@jamesgeorge007](https://github.com/jamesgeorge007), which, as its name suggests, displays your latest GitHub activities in your profile (the Pull Request you worked on, the issue you commented, ...). The setup is very similar to the previous GitHub Action. Indeed, you will need to add 2 anchors in the README file, and a GitHub Action configuration file, in order to be fully setup:
- The 2 anchors in the README, where the list of activities will be displayed

```
<!--START_SECTION:activity-->
<!--END_SECTION:activity-->
```

- The configuration of the GitHub Actions ([latest-github-activity.yml](https://github.com/Xav83/Xav83/blob/main/.github/workflows/latest-github-activity.yml)), which will be updated every 30 minutes, in this case

```
name: Latest GitHub Activity

on:
 schedule:
   - cron: "*/30 * * * *"
 workflow_dispatch:

jobs:
 build:
   runs-on: ubuntu-latest
   name: Update this repo's README with recent GitHub activity

   steps:
     - uses: actions/checkout@v2
     - uses: jamesgeorge007/github-activity-readme@master
       env:
         GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

## Conclusion

With this article, I gave you a glimpse of what you can do to personalize your GitHub profile. When writing, I found all sorts of profile, from really neat one like the one of [@gautamkrishnar](https://github.com/gautamkrishnar), the creator of the first GitHub Action mentioned in this article to funny one like the GitHub profile of [@HFO4](https://github.com/HFO4) on which you can play Pokemon with the community! ✨✨

All of that to say that I really enjoyed creating and customizing my personal GitHub profile, and I hope that this article will get you started to do so as well. I will probably experiment even more with it in the future, and maybe, publish a part 2 to this article 😉

And for the people who had the courage to read through this long article, here is a little gift: a [website referencing awesome GitHub profile](https://zzetao.github.io/awesome-github-profile/). Personally, I spent too much time on it already 😆

--------------

Thank you all for reading this article,
And until my next article, have a splendid day 😉
