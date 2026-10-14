# NanoGPT 噪声实验固定输入

这是 `nanogpt-noise-experiments` 的私有输入仓库，只保存正式运行需要的固定数据与冻结产物。二进制大文件由 Git LFS 管理；不要在 GitHub 网页上逐个下载，也不要修改、替换或重新生成这些文件。

## 目录结构

将本仓库检出到服务器的 `/data/260010171/inputs` 后，目录应为：

```text
/data/260010171/inputs/
├── openwebtext_subset/
│   ├── train.bin
│   ├── val.bin
│   └── meta.json
└── frozen-artifacts/
    ├── initial.pt
    ├── windows.pt
    └── identity.json
```

实验代码使用：

```bash
export DATA_DIR="/data/260010171/inputs/openwebtext_subset"
export ARTIFACT_DIR="/data/260010171/inputs/frozen-artifacts"
```

## 首次下载到服务器

仓库为私有仓库。运行者需要先接受 GitHub Collaborator 邀请，并在服务器上使用自己的 GitHub 账号完成认证。服务器还必须安装 Git LFS。

如果 `/data/260010171/inputs` 已存在且只包含项目方预先创建的空目录，可直接在该目录初始化并检出：

```bash
cd /data/260010171/inputs
git init
git remote add origin https://github.com/ZZZ-020069/nanogpt-noise-inputs.git
git lfs install
git fetch origin main
git checkout -b main --track origin/main
git lfs pull
```

如果目录已经是本仓库的工作区，则更新方式为：

```bash
cd /data/260010171/inputs
git pull --ff-only
git lfs pull
```

不要在这里运行实验，也不要把训练结果写进该仓库。

## 下载后校验

先确认 LFS 文件已经完整下载，而不是只有很小的指针文本：

```bash
cd /data/260010171/inputs
git lfs ls-files
ls -lh openwebtext_subset/train.bin \
       openwebtext_subset/val.bin \
       openwebtext_subset/meta.json \
       frozen-artifacts/initial.pt \
       frozen-artifacts/windows.pt \
       frozen-artifacts/identity.json
sha256sum -c SHA256SUMS
```

最后进入代码仓库执行项目级身份校验：

```bash
cd /data/260010171/nanogpt-noise-experiments
python -m nanogpt_noise.data \
  --data /data/260010171/inputs/openwebtext_subset \
  --artifacts /data/260010171/inputs/frozen-artifacts \
  --verify
```

命令应只输出：

```text
9cabd9c54c6d717354f8f6750715f9280c5502f0a85a9032806b9c7956cbd76e
```

出现缺文件、`hash mismatch`、LFS 下载失败或不同 ID 时，应停止运行并联系项目提供方。
