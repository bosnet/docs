---
layout: post
permalink: /how-to-report-issue/
---
---
# How to report issues ?

All the development happens on Github, on public projects.
Depending on the nature of your issue (wallet, congress voting desktop app, or core nodes), you should report to the appropriate repository.
The list of repository can be found on [our organization page](https://github.com/bosnet).

For core issues, or if you are not sure which component is affected, please report on [Sebak](https://github.com/bosnet/sebak/issues/new).
You can also email us at [dev-support@blockchainos.org](dev-support@blockchainos.org) for questions, or if your issue contains sensitive informations.

If your issue concerns an exchange integration, please notify us and we'll set the [Exchange](https://github.com/bosnet/sebak/labels/Exchange) label.
In the error report, please include the following:

    * The question or a description of the issue
    * The affected environment, e.g. in [Testnet](https://testnet-sebak.blockchainos.org), [MainNet](https://mainnet.blockchainos.org/),  or your own
    * SEBAK version and commit hash, found in `node.version.git-commit` when accessing the root of the API
    * If applicable, the request you sent, and answer you got; when using the CLI's `sebak wallet` command, use the `--verbose` flag
