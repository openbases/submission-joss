# Open Bases JOSS Submission

![img/preview.png](https://github.com/openbases/builder-pdf/raw/master/img/preview.png)

> Hi friend! :wave:

[![CircleCI](https://circleci.com/gh/openbases/submission-joss.svg?style=svg)](https://circleci.com/gh/openbases/submission-joss)

This is an [openbases](https://openbases.github.io) template to help you
to prepare a submission to the [Journal of Open Source Software](http://joss.theoj.org).

**What does this mean?**

You can:

 1. Fork the repository, add your paper, and connect to CircleCI and Github Pages
 2. The paper markdown and bibliography are [tested and validated](https://openbases.github.io/openbases-python/html/usage.html#validation)
 3. When passing, a rendered preview is available as a [build artifact](https://circleci.com/docs/2.0/artifacts/)
or live paper [like this](https://openbases.github.io/builder-pdf/).

Then you can submit your paper to JOSS in the usual way, and have confidence that your
PDF and references have already been tested.

**What is this not?**

 1. This repository will **not** submit the paper for you. This would be potentially dangerous for the open journal to allow for this kind of programmatic submission.
 2. This template will **not** check for "human" elements of your submission (e.g., that your summary is complete). It can only check for programmatic things like length, and formatting.


**What does it check?**

Take a look at the [paper criteria](https://github.com/openbases/openbases-python/blob/paper/validation/openbases/main/validate/criteria/paper.yml#L31) for the most updated criteria!

**Could I just run this locally?**

Yes you could! You can use the [openbases/openbases](https://openbases.github.io/openbases-python/html/docker.html#validate) Docker image to validate your markdown and references. This won't generate the rendering. If you want to do
that, you could just use the [openbases/openbases-pdf](https://www.github.com/openbases/openbases-pdf) 
builder container to do this.

# Usage

## 1. Preparation

First fork the repostitory to your account, and clone the fork.

```bash
git clone https://www.github.com/<username>/submission-joss
git clone git@github.com:<username>/submission-joss.git
```

Next, add your paper! You should have a submission (in a folder `paper`) called `paper.md`
and a matching `paper.bib`.

```bash
paper
   paper.md
   paper.bib
```

### Custom Logo

If you want a custom logo, add it as `logo.png`. If not, a beautiful icon
will be selected from [openbases-icons](https://openbases.github.io/openbases-icons/preview)

```bash
paper
   paper.md
   paper.bib
   logo.png
```


### Custom Latex Template

The same goes for the latex template! To customize it, just add a "latex.template"
to the same directory

```
paper
    paper.md
    paper.bib
    latex.template
```

If not found, by default, we use the template served 
by [whedon](https://github.com/openjournals/whedon/blob/master/resources/latex.template)
An example is provided here, in [paper](paper). 


### Step 2. Configuration

Next, we will talk about setting up the services! If you are interested
specifically in developing the [openbases-pdf](https://www.github.com/openbases/openbases-pdf) 
doing the heavy lifting to generate the PDF,
take a look as [his code](https://www.github.com/openbases/openbases-pdf).

The hidden folder [.circleci/config.yml](.circleci/config.yml) has instructions for
[CircleCI](https://circleci.com/dashboard/) to automatically discover
and build your paper. If you choose to deploy back to Github pages, there is 
also a [template.html](.circleci/template.html) file that is used as a template. 
The first does most of the steps required for build and deploy, including:

 1.  clone of the repository with your paper folder
 2.  build your pdf using the openbsaes-pdf container
 3.  (optional) generate a container to serve your paper
 4.  (optional) push back to Github pages

Thus, if you have forked the repository and cloned your fork, you should be able to use
the files that are pulled. And if you are an advanced user, you could even customize if you
want.

### Step 3. Generation

This happens all in the CI, and is ready to go for you! If you go under the "build"
step in your workflow, you can click on the "artifacts" tab to see your paper outputs.

### Step 4. (optional) Github Machine User

If you want to deploy the manifests and paper back to Github pages, the easiest option (and
one that doesn't put your entire Github account under risk) is to create a machine
user. This comes down to creating a second Github user account (with a different email)
and then giving the account permission to the repository, and generate an ssh key for it.
You won't need to worry about how the deploy is done - this is handled in the circleCI
recipe included with the template. Here are instructions for setting up credentials, derived
from [this great resource](https://github.com/DevProgress/onboarding/wiki/Using-Circle-CI-with-Github-Pages-for-Continuous-Delivery).

**Why do I need to do this?**

Pushing content back to Github pages requires a deploy key. Although Circle will generate a deploy key for you, it only has read access. We need to generate a machine user with write access. Read more about machine user keys [keys](https://circleci.com/docs/github-security-ssh-keys/#machine-user-keys)

**Instructions**

 1. Open a second browser so you can stay logged into your main Github account in one browser, and [create a new Github account](https://github.com/join) there. You will basically need another email address, and a creative username.
 2. In your main Github account (the primary browser) add this user as a collaborator to your repository. They will need push access.
 3. Accept the invitation in the second browser, or the emali sent to you.
 4. In the second browser, again log in to [Circle CI](https://circleci.com/) with your new Github account. Make sure you log in via your Github machine user account, and that you have accepted the invitation.
 5.  Click on "Add Projects", and select your regular Github username under "Choose Organization". This is the owner of the project.  Then click "Follow Project" next to the repository name on the left of the menu. 
 6. This is important! Once followed, go to the Project Settings -> "Checkout SSH keys", and click on the button to "Authorize with GitHub." You will be taken back to Github, signed in as the machine user, and you should click "Authorize Application." Finally, click the Create and add machine user github name key button on the same page.

**Generate Key**
Follow the instructions [here](https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/#generating-a-new-ssh-key) to generate a new ssh key. The steps to add it to your project are a little weird, but I'll try to be specific:

 - your machine user must first be added as a collaborator to the project
 - you must then log in to CircleCI with your **machine user** and click on Jobs to see the project
 - when you see it, click on any of the steps and click on "Follow Project" in the upper right
 - Under the project settings (gear icon in the upper right) under "Permissions" click on "Checkout SSH keys" and then click the button to "Add user key." If you don't do this, it will give you an error that the key is read only.


### Step 5. Connect to CircleCI

If you do not already have a Circle CI account, head [here](https://circleci.com/signup/) and create one, and
add your project to your Circle CI account.  Here are [instructions](https://circleci.com/docs/getting-started/) if you've never done this before.

Once you have an account, if you navigate to the main [app page](https://circleci.com/dashboard/)
you should be able to click "Add Projects" and then select your
repository. If you don't see it on the list, then select a different
organization in the top left. Once you find the repository, you can
click the button to "Start Building" adn accept the defaults.

Before you push or trigger a build, let's set up the following
environment variables. Also in the project interface on CirleCi, click
the gears icon next to the project name to get to your project settings.
Under settings, click on the "Environment Variables" tab. In this
section, you want to define the following:

 *  `GITHUB_USER` and `GITHUB_EMAIL` should be your machine user Github account
 *  `OPENBASES_PAPER_ARGS` should define any additional argument pairs (`--arg1 value1 --bool`) to pass to the [openbases/openbases-pdf container entrypoint](https://github.com/openbases/openbases-pdf/blob/master/entrypoint.sh). This will be passed as followed:

```bash
/bin/bash entrypoint.sh paper.md "${OPENBASES_PAPER_ARGS}"
```

### Step 6. Push and Deploy!

Once the environment variables are set up, you can push or issue a pull
request to see circle build the workflow. Remember that you only need
the `.circleci/config.yml`, `.circleci/template.html` and not any other files in the repository. If
your notebook is hosted in the same repo, you might want to add these,
along with your requirements.txt, etc.

## FAQ

**How do I customize the build?**

The circle configuration file is the entire workflow that does build, test, and deploy.
This literally means you can edit this text file and change any or all behavior. This could
be as simple as changing some of the text output, to adding an additional set of testing or
deployment options, or more complex like adding entire new steps in the workflow. 

**How do I customize the template?**

The template.html is the same! You can tweak it, completely change it, or throw it out and push something entirely
different back to Github pages. It's populated with environment variables, and so if you want
to add variable content this is how to do it. You can add variables to:

 - an `environment` section defined in the defaults section of the [config.yml](.circleci/config.yml)
 - directly in the step of the manifest section where you see the template.html being generated
 - in CircleCI settings (appropriate for secrets)


**How do I run builds for pull requests?**

By default, new builds on CircleCI will not build for pull requests and
you can change this default in the settings. You can easily add filters
(or other criteria and actions) to be performed during or after the
build by editing the `.circleci/config.yml` file in your repository.


## Credits

 - The beautiful viewer is the slightly modified [ng-pdf-viewer](https://github.com/samrose3/ng-pdf-viewer) from @samrose3.
