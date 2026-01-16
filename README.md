# mwt

一个小脚本：从当前仓库的 `HEAD` 创建一个新分支，并用 `git worktree` 在固定目录创建一个新的工作区，分支名和工作区目录都使用 6 位短 ID 避免重复。

默认工作区路径（不当作临时目录清理）：

`~/Library/Application Support/mwt/worktrees/<project>/<id6>`

例如：

- `~/Library/Application Support/mwt/worktrees/app1/abc123`
- `~/Library/Application Support/mwt/worktrees/app1/def456`
（示例中 ID 仅示意）

## 用法

### 方式 1：本地放到 PATH

把脚本放到 PATH（例如 `~/bin`）并赋予可执行权限：

```sh
chmod +x mwt
```

运行（默认会进入新 worktree 并打开一个交互 shell）：

```sh
./mwt
```

### 方式 2：通过 npx 使用（推荐）

发布到 npm 后，可以直接运行仓库里的脚本：

```sh
npx xsh mwt
```

也可以直接执行脚本同名的 bin（需要 npm 的 `exec`/`npx` 支持；更通用的写法是 `-p`）：

```sh
npx -p xsh mwt
```

## 发布到 npm

首次在本机发布需要登录：

```sh
npm login
```

发布（公开包）：

```sh
npm publish --access public
```

说明：

- 每次发布前会自动运行 `tools/sync-bins.mjs`，把当前目录（以及可选的 `scripts/` 目录）里所有“可执行文件”同步到 `package.json` 的 `bin` 字段；后续新增脚本只要 `chmod +x` 并重新发布即可通过 npx 使用。

删除指定分支对应的 worktree 并删除该分支：

```sh
./mwt delete wt-<id6>
```

强制删除（worktree 有未提交改动/分支不可 fast-forward 删除时用）：

```sh
./mwt delete wt-<id6> --force
```

删除当前项目在默认根目录下的所有 worktree（需要确认）：

```sh
./mwt delete-all
```

强制删除：

```sh
./mwt delete-all --force
```

只创建 worktree，不开启新 shell：

```sh
./mwt --no-shell
```

打印新 worktree 路径（方便你自己 `cd`）：

```sh
./mwt --print-path --no-shell
```

指定基于哪个 ref 创建（默认 `HEAD`）：

```sh
./mwt --base main
```
