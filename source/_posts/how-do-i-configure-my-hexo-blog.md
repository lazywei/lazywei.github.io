title: How do I configure my Hexo blog
date: 2013-11-24 16:18:40
tags: [hexo]
---

Here is my basic setting for hexo.

### Init

- Add a repo named `lazywei.github.io` on [github](https://github.com/).
- Init hexo: `hexo init lazywei.github.io`.

### Git

- Init git: `cd lazywei.github.io; git init`.
- Use `source` for default branch name instead of `master`: `git branch -M master source`.
- Add github as origin remote `git remote add origin git@github.com:lazywei/lazywei.github.io.git`.
- Add `public` and `.deploy` into `.gitignore`.
- Commit: `git commit -m 'Init commit'`.

### Deployment

- Change the deployment section of `_config.yml` to:
```
# Deployment
## Docs: http://zespia.tw/hexo/docs/deploy.html
deploy:
  type: github
    repository: https://github.com/lazywei/lazywei.github.io.git
      branch: master
```
- Generate static files: `hexo g` or `hexo genereate`.
- Deploy to github: `hexo deploy`.

### Workflow

- Write post: `hexo new "post title"`.
- Commit source.
- Generate static files.
- Deploy to github.
