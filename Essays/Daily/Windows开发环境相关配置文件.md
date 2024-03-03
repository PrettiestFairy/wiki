# Windows开发环境相关配置文件

## Python 相关

### 环境变量

```bash
# 配置workon的根路径
setx WORKON_HOME E:\Data\Development\PythonVirtualEnv /s
# 使用pipenv新建虚拟环境到项目路径下
setx PIPENV_VENV_IN_PROJECT 1 /s
# 运行python项目/文件不生产__pycache__
setx PYTHONDONTWRITEBYTECODE 1 /s

```

### Pip

路径: `%userprofile%\pip\pip.ini`

```ini
[global]
timeout = 3
cache-dir = E:\Data\Cache\pip

```

## Node JS 相关

### npmrc

路径: `%userprofile%\.npmrc`

```ini
; _auth=null
; access=null
; all=false
; allow-same-version=false
; also=null
; audit=true
; audit-level=null
; auth-type=web
; before=null
; bin-links=true
; browser=null
; ca=null
cache=E:\Data\Cache\npm
; cache-max=null
; cache-min=0
; cafile=null
; call=
; cert=null
; ci-name=null
; cidr=null
; color=true
; commit-hooks=true
; depth=null
; description=true
; dev=false
; 
; diff-ignore-all-space=false
; diff-name-only=false
; diff-no-prefix=false
; diff-dst-prefix=b/
; diff-src-prefix=a/
; diff-text=false
; diff-unified=3
; dry-run=false
; editor=C:\Windows\notepad.exe
; engine-strict=false
; fetch-retries=2
; fetch-retry-factor=10
; fetch-retry-maxtimeout=60000
; fetch-retry-mintimeout=10000
; fetch-timeout=300000
; force=false
; foreground-scripts=false
; format-package-lock=true
; fund=true
; git=git
; git-tag-version=true
; global=false
; globalconfig=
; global-style=false
; heading=npm
; https-proxy=null
; if-present=false
; ignore-scripts=false
; 
; include-staged=false
; include-workspace-root=false
; init-author-email=
; init-author-name=
; init-author-url=
; init-license=ISC
; init-module=~/.npm-init.js
; init-version=1.0.0
; init.author.email=
; init.author.name=
; init.author.url=
; init.license=ISC
; init.module=~/.npm-init.js
; init.version=1.0.0
; install-links=false
; install-strategy=hoisted
; json=false
; key=null
; legacy-bundling=false
legacy-peer-deps=true
; link=false
; local-address=null
; location=user
; lockfile-version=null
; loglevel=notice
; logs-dir=null
; logs-max=10
; long=false
; maxsockets=15
; message=%s
; node-options=null
; noproxy=
; offline=false
; 
; omit-lockfile-registry-resolved=false
; only=null
; optional=null
; otp=null
; 
; package-lock=true
; package-lock-only=false
; pack-destination=.
; parseable=false
; prefer-offline=false
; prefer-online=false
prefix=E:\Developments\Enviroments\node
; preid=
; production=null
; progress=true
; provenance=false
; proxy=null
; read-only=false
; rebuild-bundle=true
; registry=https://registry.npmjs.org/
; replace-registry-host=npmjs
; save=true
; save-bundle=false
; save-dev=false
; save-exact=false
; save-optional=false
; save-peer=false
; save-prefix=^
; save-prod=false
; scope=
; script-shell=null
; searchexclude=
; searchlimit=20
; searchopts=
; searchstaleness=900
; shell=C:\Windows\system32\cmd.exe
; shrinkwrap=true
; sign-git-commit=false
; sign-git-tag=false
; strict-peer-deps=false
; strict-ssl=true
; tag=latest
; tag-version-prefix=v
; timing=false
; tmp=E:\Data\Cache\User
; umask=0
; unicode=false
; update-notifier=true
; usage=false
; user-agent=npm/{npm-version} node/{node-version} {platform} {arch} workspaces/{workspaces} {ci}
; userconfig=~/.npmrc
; version=false
; versions=false
; viewer=browser
; which=null
; 
; workspaces=null
; workspaces-update=true
; yes=null

```

### cnpmrc

路径: `%userprofile%\.cnpmrc`

```ini
; _auth=null
; access=null
; all=false
; allow-same-version=false
; also=null
; audit=true
; audit-level=null
; auth-type=web
; before=null
; bin-links=true
; browser=null
; ca=null
cache=E:\Data\Cache\npm
; cache-max=null
; cache-min=0
; cafile=null
; call=
; cert=null
; ci-name=null
; cidr=null
; color=true
; commit-hooks=true
; depth=null
; description=true
; dev=false
; 
; diff-ignore-all-space=false
; diff-name-only=false
; diff-no-prefix=false
; diff-dst-prefix=b/
; diff-src-prefix=a/
; diff-text=false
; diff-unified=3
; dry-run=false
; editor=C:\Windows\notepad.exe
; engine-strict=false
; fetch-retries=2
; fetch-retry-factor=10
; fetch-retry-maxtimeout=60000
; fetch-retry-mintimeout=10000
; fetch-timeout=300000
; force=false
; foreground-scripts=false
; format-package-lock=true
; fund=true
; git=git
; git-tag-version=true
; global=false
; globalconfig=
; global-style=false
; heading=npm
; https-proxy=null
; if-present=false
; ignore-scripts=false
; 
; include-staged=false
; include-workspace-root=false
; init-author-email=
; init-author-name=
; init-author-url=
; init-license=ISC
; init-module=~/.npm-init.js
; init-version=1.0.0
; init.author.email=
; init.author.name=
; init.author.url=
; init.license=ISC
; init.module=~/.npm-init.js
; init.version=1.0.0
; install-links=false
; install-strategy=hoisted
; json=false
; key=null
; legacy-bundling=false
legacy-peer-deps=true
; link=false
; local-address=null
; location=user
; lockfile-version=null
; loglevel=notice
; logs-dir=null
; logs-max=10
; long=false
; maxsockets=15
; message=%s
; node-options=null
; noproxy=
; offline=false
; 
; omit-lockfile-registry-resolved=false
; only=null
; optional=null
; otp=null
; 
; package-lock=true
; package-lock-only=false
; pack-destination=.
; parseable=false
; prefer-offline=false
; prefer-online=false
prefix=E:\Developments\Enviroments\node
; preid=
; production=null
; progress=true
; provenance=false
; proxy=null
; read-only=false
; rebuild-bundle=true
; registry=https://registry.npmjs.org/
; replace-registry-host=npmjs
; save=true
; save-bundle=false
; save-dev=false
; save-exact=false
; save-optional=false
; save-peer=false
; save-prefix=^
; save-prod=false
; scope=
; script-shell=null
; searchexclude=
; searchlimit=20
; searchopts=
; searchstaleness=900
; shell=C:\Windows\system32\cmd.exe
; shrinkwrap=true
; sign-git-commit=false
; sign-git-tag=false
; strict-peer-deps=false
; strict-ssl=true
; tag=latest
; tag-version-prefix=v
; timing=false
; tmp=E:\Data\Cache\User
; umask=0
; unicode=false
; update-notifier=true
; usage=false
; user-agent=npm/{npm-version} node/{node-version} {platform} {arch} workspaces/{workspaces} {ci}
; userconfig=~/.npmrc
; version=false
; versions=false
; viewer=browser
; which=null
; 
; workspaces=null
; workspaces-update=true
; yes=null

```

### yarnrc

路径: `%userprofile%\.yarnrc`

```bash
cache-folder "E:\\Data\\Cache\\Yarn"
lastUpdateCheck 1695673303535

```

## Git 相关

### gitconfig

路径: `%userprofile%\.gitconfg`

```ini
[user]
	name = AceProfessional
	email = fairylandhost@outlook.com
[http]
	sslVerify = true
	# proxy = 127.0.0.1:65353
[https]
	# proxy = 127.0.0.1:65353
[filter "lfs"]
	required = true
	clean = git-lfs clean -- %f
	smudge = git-lfs smudge -- %f
	process = git-lfs filter-process
[init]
	defaultBranch = ReleaseMaster
[core]
	autocrlf = input
	safecrlf = false
	eol = lf
[safe]
    directory = *
[credential "https://git.jetbrains.space"]
	provider = generic

```

## Anaconda 相关

### condrc

路径: `%userprofile%\.condarc`

```yaml
auto_update_conda: false
channels:
  - defaults
  - conda-forge
show_channel_urls: true
#envs_dirs:
#  - E:\Developments\VirtualEnvironments\Conda\envs
  # - D:\App\Anaconda\envs
pkgs_dirs:
  - E:\Data\Cache\Anaconda
# proxy_servers:
  # http: http://127.0.0.1:65353
  # https: http://127.0.0.1:65353
restore_free_channel: true
# channel_priority: strict
ssl_verify: false
report_errors: true

```

