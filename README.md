<div id="top"></div>

<div align="center">

<h3 align="center">A CI/CD workflow template for projects in shared hosting servers.</h3>

</div>

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#about-the-project">About The Project</a>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#installation">Installation</a></li>
      </ul>
    </li>
    <li><a href="#usage">Usage</a></li>
    <li><a href="#license">License</a></li>
    <li><a href="#contact">Contact</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->

## About The Project

This repo will serve as a basic started pack for setting up a CI/CD flow when a site is hosted in a shared hosting server, for example, Namecheap etc.

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- GETTING STARTED -->

## Getting Started

You will have to set up the Git flow in your shared hosting Cpanel first. Basically, start a git repo there. Add a special hook so that pushing the code there will trigger git to copy the chnages from git folder into your live or staging folder.

### Installation

1. Go to your Cpanel and click Git version control menu

2. It will let you create a git repo

3. Create the repo and pull it into your local machine

4. Remember, you will need to set up a SSH connection with the server to pull from the private repo you just created

5. Once the repo is creaed in the server (ideally create it on the same level as public_html, not inside the public folder), go to the folder, then into .git/hooks folder and click to edit 'post-receive' file, put the below code there-
    ```sh
    #!/bin/sh

    while read oldrev newrev ref
    do
        branch=`echo $ref | cut -d/ -f3`

        if [ "staging" == "$branch" ]; then
            git --work-tree=/home/cpanel_user/staging.site.com checkout -f $branch
            echo 'Changes pushed to staging site.'
        fi

        if [ "live" == "$branch" ]; then
            git --work-tree=/home/cpanel_user/public_html checkout -f $branch
            echo 'Changes pushed to production site.'
        fi

    done
    ```
    What's going on here is basically this-
    In your local machine, once you will create a branch named 'stagging' and push the changes from 'staging' (local) to 'staging' (remote), git will update the files in the git repo folder first. Then as we are hooking into git's hooks, it will see that we pushed into staging branch. So it will copy all the changes from the staging branch into the site's folder where we set up the staging copy of the site, /home/cpanel_user/staging.site.com folder in the case above.
    The same thing is happening when you will push from 'live' branch. Git will copy all the changes into public_html fodler, of the live version of the site. 

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->

## Usage

You can pretty easily push your local changes into Cpanel hosted sites, with fast and secure tunnel. This works like a charm and the best part, you don't have to upload the files, zip/unzip stuff, nothing. Only the changed files will be pushed, hence ultra-fast. Enjoy!

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- LICENSE -->

## License

Distributed under the MIT License. See `LICENSE.txt` for more information.

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- CONTACT -->

## Contact

Your Name - [@ParbezRipon](https://twitter.com/ParbezRipon) - rparbez@protonmail.com

<p align="right">(<a href="#top">back to top</a>)</p>
