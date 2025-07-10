
### Install Proto

Step 1: Install `proto`

```
apt-get install git unzip gzip xz-utils
```

```
bash <(curl -fsSL https://moonrepo.dev/install/proto.sh)
```

Step 2: create a `.prototools` file at the root of your repo:

Step 3: Install Tools (e.g., Node.js)

```
proto install node
```

If you want to set a specific version (per project)
In `.prototools`:

```
node = "20.11.0"
```

Then run:

```
proto install

to use tthis code
proto install node <version> --pin
```

 shims

```
pr 
## 20.0.0
```

```
node --version
## 20.0.0
```

```
which node
## ~/.proto/shims/node
```
### Install Moon

```
proto install moon
```


### Initialize Moonrepo

Run this in your project root:

```
moon init
```
This will ask a few questions and generate the Moonrepo configuration.



### Add Projects

Moon will detect projects based on their `package.json` by default (you can override this in `workspace.yml`).

Example `workspace.yml`:

```
projects:
  - node-apps/*
  - packages/*

```


### Define Tasks (in `moon.yml` inside each project)

Example `apps/maze/moon.yml`:

```
type: 'application'

language: 'typescript'

tasks:
  dev:
    command: 'tsx --no-watch --experimental-sqlite src/app.ts'
    local: true
    options:
      envFile: '/.env.shared'

  format:
    command: 'biome check --fix'
```



Displays detailed information about a project defined in your `Moonrepo` workspace.

```
moon project maze
```


install specific package in moon tool chain

```linux
moon init <tool>
e.g moon init npm
```


```
moon sync hooks
```