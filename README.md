# adiacov.github.io

A software development place.  
Blog, Ideas, Code... all published on [_adiacov.github.io_](https://adiacov.github.io/) website.

### Deployment

Usually, there are two repositories (at least two branches) involved in a publishing process: the branch containing the
source files, and the branch containing the build output to be served with GitHub Pages. In the following tutorial, they
will be referred to as "source" and "deployment", respectively.

Using this guide, the following branches are created:  
**main** - source (source code for the _gh-pages_ branch)  
**gh-pages** - deployment (the branch from where GitHub site is build)

[Docusaurus: deploying-to-github-pages](https://docusaurus.io/docs/deployment#deploying-to-github-pages)

### Workflow

The initial work is carried out on the _main_ branch. Once completed and tested, the changes are ready to be integrated
into the _gh-pages_ branch.  
Upon successful completion of this staging phase, the work is merged into _gh-pages_ branch. This branch specifically is
needed for GitHub Pages, where the website is generated and hosted. This meticulous process ensures a systematic and
well-organized transition from development to the final website.

### Tech

This project is build with [Docusaurus](https://docusaurus.io/) and [GitHub Pages](https://docs.github.com/en/pages)