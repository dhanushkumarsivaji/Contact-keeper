
## Usage

Install dependencies

```bash
npm install
npm client-install
```

### Mongo connection setup

Edit your /config/default.json file to include the correct MongoDB URI

### Run Server

```bash
npm run dev     # Express & React :3000 & :5000
npm run server  # Express API Only :5000
npm run client  # React Client Only :3000
```

[alias]

lg = log --all --oneline --graph --decorate

st = status

ci = commit -m

cn = commit -nm

co = checkout

cb = checkout -b

b = branch

ba = branch -a

del = branch -D

d = diff

pl = pull

cg = config --global -e

ms = merge --squash

m = merge

sh = stash

dt = difftool

mt = mergetool

pm = pull origin master

shu = stash --include-untracked

[fetch]

prune = true
